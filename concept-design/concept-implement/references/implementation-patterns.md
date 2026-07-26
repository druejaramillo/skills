# Implementation Patterns and Examples

Read this reference before choosing files or packages, mapping SSF state to physical storage, implementing a named synchronization, defining HTTP or Go error mappings, or writing tests. It provides optional Go-oriented projections; use the existing codebase layout when it expresses the required logical ownership and action seams.

## File Structure

Use the existing repository layout when it can express the architecture translation. This is one possible projection, not a required directory shape:

```
/concepts/
  label/
    label.go        <- concept: state + actions
    repository.go   <- concrete persistence realization, if needed
  session/
    session.go
/app/
  app.go            <- App struct wiring concepts together
  sync_auth.go      <- synchronizations for auth flows
  sync_content.go   <- synchronizations for content flows
main.go
```

Or using the `internal/` convention:

```
internal/
  invite/
    invite.go
    service.go
    repository.go
    errors.go
    service_test.go
  app/
    accept_invite_and_add_member.go
```

Group synchronizations by domain, not by concept. A sync file describes interactions, not any single concept. Name sync functions after the composed behavior, not after one concept, for example `AcceptInviteAndAddMember` or `SubscribeAndGrantAccess`.

## Concept as a Go Package

A concept can be a Go package when that makes its responsibility and action seam clearer. It can also be a distinct responsibility within an established module. Add a package comment only when that is an existing codebase convention or it clarifies a non-obvious boundary.

**Spec:**

```
concept Label [Item]

purpose
  organize and filter items by user-defined categories

state
  a set of Labels with
    a name String
    a owner String

  a set of Assignments with
    a label Label
    a item Item

actions
  create [name: String; owner: String] => [label: Label]
  create [name: String; owner: String] => [error: String]

  assign [label: Label; item: Item] => []
  assign [label: Label; item: Item] => [error: String]

  remove [label: Label; item: Item] => []

  find [name: String; owner: String] => [items: set of Item]
```

**Implementation:**

```go
package label

import (
    "database/sql"
    "errors"
)

type Label struct {
    ID    string
    Name  string
    Owner string
}

// Repository is concrete because this application has one Postgres realization.
// Add a port only if a meaningful alternate realization or protocol boundary appears.
type Repository struct {
    db *sql.DB
}

type Concept struct {
    repository *Repository
}

func New(repository *Repository) *Concept {
    return &Concept{repository: repository}
}

func (c *Concept) Create(name, owner string) (Label, error) {
    if name == "" {
        return Label{}, errors.New("label name cannot be empty")
    }
    return c.repository.CreateLabel(name, owner)
}

// Assign associates a label with an item. Item is an opaque ID string:
// Label does not inspect or mutate item state.
func (c *Concept) Assign(labelID, itemID string) error {
    if !c.repository.LabelExists(labelID) {
        return errors.New("label not found")
    }
    return c.repository.AssignLabel(labelID, itemID)
}

func (c *Concept) Remove(labelID, itemID string) error {
    return c.repository.RemoveAssignment(labelID, itemID)
}

func (c *Concept) Find(name, owner string) ([]string, error) {
    return c.repository.FindItemsByLabel(name, owner)
}
```

Key choices:

- Items are `string`: the concept does not import any "item" type.
- The repository is concrete because no alternate persistence behavior is evidenced.
- There are no imports from other concept packages and no direct item mutation.
- Introduce a port at the consuming seam only for meaningful variation.

## Translating State to Physical Storage

SSF declares logical state, not a mandatory table-per-declaration layout. Choose a physical representation that preserves each concept's logical owner, authorized writers, invariants, and action semantics.

| SSF pattern | Possible DB representation |
|-------------|-------------------|
| `a set of X with fields` | Table/collection `X`, one row/doc per object |
| `a Subset set of X with fields` | Table/collection `Subset`, FK/ref to `X` by ID |
| `an element Config with fields` | Table/collection `Config`, constrained to one row |
| Set-valued field `a tags set of String` | Junction table or embedded array |
| `optional` field | Nullable column or omitted key |
| Enum `of A or B` | String column with enum constraint |

**SSF to Postgres example:**

```sql
-- Labels concept
CREATE TABLE labels (
    id    TEXT PRIMARY KEY,
    name  TEXT NOT NULL,
    owner TEXT NOT NULL
);

-- item_id has no FK: the Label concept does not know what table items come from
CREATE TABLE label_assignments (
    label_id TEXT NOT NULL REFERENCES labels(id),
    item_id  TEXT NOT NULL,
    PRIMARY KEY (label_id, item_id)
);
```

**Logical-ownership rules:**

- Store only state needed for the concept's actions; avoid fields that exist in the domain but support no behavior.
- Assign each state field, relation, and invariant a logical owner and identify its authorized writers.
- Tables may reference opaque IDs from other concepts without importing their behavior.
- Use constraints, transactions, and validations where they enforce the owner's invariants.
- A shared table, record, or transaction is valid when it has explicit owners, write paths, and invariant boundaries.
- Treat shared physical storage as a problem only when it causes ambiguous ownership, bypassed invariants, contradictory actions, or hidden coupling.
- Record a consequential shared-storage decision in the architecture translation or coupling ledger, not automatically in a separate document or ADR.

## Implementing Synchronizations

Synchronizations are named application boundaries that compose multiple concept actions. Implement the semantics in the approved synchronization/coupling ledger, including state ownership, consistency, retry owner, and forbidden bypass path.

**Synchronization pattern:**

```
when ConceptA.actionA(args) occurs
  and ConceptB.actionB(mappedArgs) can occur
then perform both atomically
else block the initial action and return the relevant error
```

**Code placement:** Put synchronizations in one of:

- `internal/app`, `internal/workflows`, `internal/sync`, or `internal/usecase`
- A handler/cmd layer, or an event consumer if eventual consistency is acceptable

```go
// app/sync_tasks.go
package app

import (
    "myapp/concepts/label"
    "myapp/concepts/todo"
)

type App struct {
    Labels *label.Concept
    Todos  *todo.Concept
}

// CreateTask implements the named CreateTaskAndAssignPending synchronization.
// The application boundary owns atomicity and returns the first failed action.
func (a *App) CreateTask(name string) (*todo.Task, error) {
    var task *todo.Task
    err := a.InTransaction(func() error {
        var err error
        task, err = a.Todos.Add(name)
        if err != nil {
            return err
        }
        return a.Labels.Assign(a.pendingLabelID, task.ID)
    })
    return task, err
}

// CompleteTask implements the named CompleteTaskAndUpdateLabels synchronization.
func (a *App) CompleteTask(taskID string) error {
    return a.InTransaction(func() error {
        if err := a.Todos.Complete(taskID); err != nil {
            return err
        }
        if err := a.Labels.Remove(a.pendingLabelID, taskID); err != nil {
            return err
        }
        return a.Labels.Assign(a.doneLabelID, taskID)
    })
}
```

**Rules for sync functions:**

- Name the function after the user-facing operation, not the internal actions.
- Make the design-level intent, state ownership, consistency, and retry owner discoverable in code or its existing ledger.
- Keep a composed business rule at its named synchronization boundary; framework wiring or read-only rendering does not automatically need a new sync.
- If a sync is transactional, wrap it at the boundary that owns the transaction.
- Do not create a document or ADR merely to explain a synchronization.

**When to use synchronization:**

- One concept action should trigger another concept action.
- Access control or subscription gates another action.
- An accepted invite creates membership.
- Payment success activates a subscription.
- A deleted item moves to trash.
- A notification is sent after a job completes.

## API Design

Expose concept actions directly where possible.

**Good:**

```http
POST /invites
POST /invites/{token}/accept
POST /invites/{token}/decline
```

**Less good:**

```http
POST /workspace/member/add-by-email
```

The second hides the Invite concept and prematurely combines Invite and Membership.

## Mapping Error Overloads

The spec pattern `=> [error: String]` maps to Go's error return:

| Spec | Go |
|------|-----|
| `create [...] => [item: Item]` | `return item, nil` |
| `create [...] => [error: String]` | `return Item{}, err` |
| `delete [...] => []` | `return nil` |
| `find [...] => [results: set of Item]` | `return items, nil` |

**Typed errors** let the sync layer format messages appropriately:

```go
type NotFoundError struct {
    Resource string
    ID       string
}

func (e *NotFoundError) Error() string {
    return fmt.Sprintf("%s %q not found", e.Resource, e.ID)
}

type ValidationError struct {
    Field   string
    Message string
}
```

Standard typed errors to define per concept:

- `ErrNotFound`, `ErrInvalidInput`, `ErrAlreadyExists`, `ErrExpired`
- `ErrUnauthorized`, `ErrBlocked`, `ErrInvariantViolation`

## Testing

Every implemented concept needs at least one test derived from the operational principle.

**Test levels:**

1. **Action-seam tests**: call concept actions through their public seam.
2. **State transition tests**: verify state after action traces.
3. **Failure tests**: verify blocked actions and errors.
4. **Synchronization tests**: verify named composed behavior, consistency, and retry/error handling.
5. **API tests**: verify routes map cleanly to actions.

Use storage directly only to arrange fixtures or inspect a realization when necessary. A test that claims to verify a concept action or synchronization must exercise its action seam rather than bypass the logical owner with direct storage writes.

**Write test names in concept language:**

```
TestInvite_CreateThenAccept
TestSubscription_SubscribeThenCheckAccess
TestTrash_DeleteThenRestore
TestAccess_BlocksUnauthorizedAction
TestLabel_AssignThenFind
```

## Concept Reuse via Multiple Instantiation

A parameterized concept, for example `Label [Item]`, can be instantiated multiple times for different target types. Give each instance a distinct logical namespace and ownership boundary. Separate collections are one valid projection; a shared representation is also valid when instance identity, allowed writers, and invariants remain explicit.

```go
// Two separate instantiations of the same Label concept:
postLabels    := label.New(postLabelRepository)
commentLabels := label.New(commentLabelRepository)
```

## Validator Actions

Actions that check state without changing it, for example `checkPermission` or `isAuthor`, are still actions. In Go, return an error when the check fails so they chain naturally:

```go
func (c *Concept) CheckPermission(userID, resource, action string) error {
    ok, err := c.repository.HasPermission(userID, resource, action)
    if err != nil {
        return err
    }
    if !ok {
        return &PermissionDeniedError{User: userID, Resource: resource, Action: action}
    }
    return nil
}
```

In a synchronization:

```go
func (a *App) DeletePost(actorID, postID string) error {
    if err := a.Roles.CheckPermission(actorID, "post", "delete"); err != nil {
        return err // guard: synchronization aborts here if permission is missing
    }
    return a.Posts.Delete(postID)
}
```
