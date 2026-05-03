---
name: concept-implement
description: >-
  Use this skill when translating concept design specs into working code, or
  when structuring a codebase around concepts and synchronizations. Trigger
  when someone says "implement this concept", "how do I code this concept spec",
  "turn this concept into code", "how do I implement concept synchronization",
  "how should I structure my backend around concepts", "how do I keep concepts
  independent in code", "convert this concept design to Go/TypeScript/Python",
  "add a concept to an existing codebase", "refactor code to align with
  concepts", "create APIs, database tables, services, handlers, UI flows, or
  tests from concept specs", or "create code changes after a concept audit".
  Also use when someone asks how to translate a concept's state model into a
  database schema, or how to implement the synchronization layer that connects
  concepts. Uses Go for code examples unless another language is specified.
  This skill applies whether the user has a formal spec or is just thinking
  through implementation of a well-defined user-facing concept.
---

# Concept Implementation

This skill translates concept design specs into code. The core principle: modularity in the design maps directly to modularity in the code. Each concept becomes an independent module that owns its own state and exposes its actions as functions. Concepts never call each other. All cross-concept coordination lives in a separate synchronization layer.

---

## The Architecture

```
┌──────────────────────────────────────────────┐
│            Synchronization Layer              │
│  (route handlers, app services, mediators)    │
│  - imports concepts                           │
│  - implements cross-concept logic             │
│  - no direct concept-to-concept calls         │
└──────────┬────────────────┬──────────────────┘
           │                │
    ┌──────▼──────┐  ┌──────▼──────┐
    │  Concept A  │  │  Concept B  │
    │  own state  │  │  own state  │
    │  own actions│  │  own actions│
    │  no imports │  │  no imports │
    │  of B or    │  │  of A or    │
    │  sync layer │  │  sync layer │
    └─────────────┘  └─────────────┘
```

**Concepts:**
- Own their own persistent state (separate DB collections/tables)
- Expose actions as functions/methods
- Accept external types as opaque IDs — never import another concept's types
- Never call actions from other concepts

**Synchronization layer:**
- The only code that imports multiple concepts
- Implements application-level rules (auth, cascades, automation)
- Translates incoming requests into concept action sequences
- Each sync function corresponds to a named synchronization from the design

---

## Implementation Mapping

| Concept spec part | Code artifact |
|---|---|
| Concept name | package/module/service/component namespace |
| Purpose | package doc, service comment, README, ADR note |
| State | tables, models, structs, schemas, repositories |
| Actions | functions, methods, commands, handlers, endpoints |
| Action inputs | request DTOs, command structs, function args |
| Action outputs | response DTOs, result types, events, errors |
| Failure cases | typed errors, validation errors, blocked actions |
| Operational principle | integration test, acceptance test, action-trace test |
| Invariants | constraints, validations, property tests, transaction checks |
| Synchronization | mediator/workflow/event handler, not direct concept import |

---

## Required Procedure

### 1. Read the Concept Spec

Extract:
- concept name and type parameters
- purpose
- state components
- actions, success outputs, error outputs
- operational principle
- optional invariants or synchronization notes

If the spec is incomplete, infer conservatively and list assumptions. Do not invent broad behavior beyond the purpose.

### 2. Identify Existing Architecture

Before editing code, inspect:
- package/module layout
- data models, migrations/schema
- handlers/routes/controllers
- service/application layer
- repository/data access layer
- tests, naming conventions
- dependency direction, error handling pattern
- transaction pattern, authorization/policy pattern

Follow the existing style unless it violates concept independence.

### 3. Choose the Narrowest Implementation Slice

Implement only what the requested concept/action needs. Preferred order:
1. State/storage needed by the action
2. Domain/application types
3. Action implementation
4. API/handler/CLI entry point, if needed
5. Synchronization mediator, if needed
6. Tests from operational principle
7. Documentation

Do not implement every possible future action unless requested.

### 4. Preserve Concept Independence

**Avoid these code smells:**
- Concept package imports another concept package to mutate its state directly
- A concept action performs multiple concept purposes
- API endpoint names hide the concept action
- Storage schema mixes unrelated concept states without clear ownership
- Validation for one concept is scattered across another
- Concept-specific rules live only in UI code
- Tests verify screens but not action traces
- "Manager" or "service" code contains several concepts

**Preferred patterns:**
- One package/module per concept where practical
- One action function per concept action
- Typed command/result structs
- Repositories scoped to concept state
- Mediators for cross-concept synchronization
- Explicit transaction boundary around synchronized actions
- Tests expressed as "after action A, then action B returns C"

---

## File Structure

```
/concepts/
  label/
    label.go        ← concept: state + actions
    store.go        ← Store interface
    store_pg.go     ← Postgres implementation
  session/
    session.go
    store.go
/app/
  app.go            ← App struct wiring concepts together
  sync_auth.go      ← synchronizations for auth flows
  sync_content.go   ← synchronizations for content flows
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

Group synchronizations by domain, not by concept. A sync file describes interactions, not any single concept. Name sync functions after the composed behavior, not after one concept (e.g., `AcceptInviteAndAddMember`, `SubscribeAndGrantAccess`).

---

## Concept as Go Package

Each concept is a package. The package comment states the purpose.

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
// Package label implements the Label concept: organizing and filtering
// items by user-defined categories. Items are treated as opaque string IDs.
package label

import "errors"

type Label struct {
    ID    string
    Name  string
    Owner string
}

// Store abstracts persistence. The concept doesn't know about Postgres vs Mongo.
type Store interface {
    CreateLabel(name, owner string) (Label, error)
    LabelExists(id string) bool
    AssignLabel(labelID, itemID string) error
    RemoveAssignment(labelID, itemID string) error
    FindItemsByLabel(name, owner string) ([]string, error)
}

type Concept struct {
    store Store
}

func New(store Store) *Concept {
    return &Concept{store: store}
}

func (c *Concept) Create(name, owner string) (Label, error) {
    if name == "" {
        return Label{}, errors.New("label name cannot be empty")
    }
    return c.store.CreateLabel(name, owner)
}

// Assign associates a label with an item. Item is an opaque ID string —
// the concept places no constraints on what kind of thing it labels.
func (c *Concept) Assign(labelID, itemID string) error {
    if !c.store.LabelExists(labelID) {
        return errors.New("label not found")
    }
    return c.store.AssignLabel(labelID, itemID)
}

func (c *Concept) Remove(labelID, itemID string) error {
    return c.store.RemoveAssignment(labelID, itemID)
}

func (c *Concept) Find(name, owner string) ([]string, error) {
    return c.store.FindItemsByLabel(name, owner)
}
```

Key choices:
- Items are `string` — the concept doesn't import any "item" type
- `Store` interface makes persistence pluggable
- No imports from other concept packages
- Package comment restates the concept's purpose

---

## Translating State to Schema

Each SSF declaration maps to its own collection/table. Never share storage between concepts.

| SSF pattern | DB representation |
|-------------|-------------------|
| `a set of X with fields` | Table/collection `X`, one row/doc per object |
| `a Subset set of X with fields` | Table/collection `Subset`, FK/ref to `X` by ID |
| `an element Config with fields` | Table/collection `Config`, constrained to one row |
| Set-valued field `a tags set of String` | Junction table or embedded array |
| `optional` field | Nullable column or omitted key |
| Enum `of A or B` | String column with enum constraint |

**SSF → Postgres example:**
```sql
-- Labels concept
CREATE TABLE labels (
    id    TEXT PRIMARY KEY,
    name  TEXT NOT NULL,
    owner TEXT NOT NULL
);

-- item_id has no FK — the Label concept doesn't know what table items come from
CREATE TABLE label_assignments (
    label_id TEXT NOT NULL REFERENCES labels(id),
    item_id  TEXT NOT NULL,
    PRIMARY KEY (label_id, item_id)
);
```

**Database rules:**
- Tables may reference IDs from other concepts but should not require importing behavior from those concepts
- Store only state needed for the concept's actions
- Avoid adding fields because they exist in the domain but do not support behavior
- Use constraints for invariants when possible
- Avoid shared "status" fields that mean different things for different concepts
- If two concepts share a table for practical reasons, document ownership of each column

---

## Implementing Synchronizations

Synchronizations are functions in the app layer that call multiple concept actions in sequence.

**Synchronization pattern:**
```
when ConceptA.actionA(args) occurs
  and ConceptB.actionB(mappedArgs) can occur
then perform both atomically
else block the initial action and return the relevant error
```

**Code placement:** Put synchronizations in one of:
- `internal/app`, `internal/workflows`, `internal/sync`, `internal/usecase`
- Handler/cmd layer, or an event consumer if eventual consistency is acceptable

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

// CreateTask implements:
// sync createTask: when Todo.add(name) => task, Label.assign(pending, task.id)
func (a *App) CreateTask(name string) (*todo.Task, error) {
    task, err := a.Todos.Add(name)
    if err != nil {
        return nil, err
    }
    // Automation: new tasks automatically receive the PENDING label.
    _ = a.Labels.Assign(a.pendingLabelID, task.ID)
    return task, nil
}

// CompleteTask implements:
// sync completeTask: when Todo.complete(t), Label.remove(pending, t), Label.assign(done, t)
func (a *App) CompleteTask(taskID string) error {
    if err := a.Todos.Complete(taskID); err != nil {
        return err
    }
    _ = a.Labels.Remove(a.pendingLabelID, taskID)
    _ = a.Labels.Assign(a.doneLabelID, taskID)
    return nil
}
```

**Rules for sync functions:**
- Name the function after the user-facing operation, not the internal actions
- Comment each sync with the design-level intent
- The sync function is the only place that imports and calls multiple concepts
- If a sync is transactional, wrap in a DB transaction at the app layer

**When to use synchronization:**
- One concept action should trigger another concept action
- Access control or subscription gates another action
- Accepted invite creates membership
- Payment success activates subscription
- Deleted item moves to trash
- Notification sent after a job completes

---

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

---

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

---

## Testing

Every implemented concept needs at least one test derived from the operational principle.

**Test levels:**
1. **Action unit tests**: call concept actions directly
2. **State transition tests**: verify state after action traces
3. **Failure tests**: verify blocked actions and errors
4. **Synchronization tests**: verify composed behavior is atomic
5. **API tests**: verify routes map cleanly to actions

**Write test names in concept language:**
```
TestInvite_CreateThenAccept
TestSubscription_SubscribeThenCheckAccess
TestTrash_DeleteThenRestore
TestAccess_BlocksUnauthorizedAction
TestLabel_AssignThenFind
```

---

## Concept Reuse via Multiple Instantiation

A parameterized concept (e.g., `Label [Item]`) can be instantiated multiple times for different target types. Give each instance its own store and DB collection.

```go
// Two separate instantiations of the same Label concept:
postLabels    := label.New(label.NewStore(db, "post_label_assignments"))
commentLabels := label.New(label.NewStore(db, "comment_label_assignments"))
```

---

## Validator Actions

Actions that check state without changing it (e.g., `checkPermission`, `isAuthor`) are still actions. In Go, return an error when the check fails — this lets them chain naturally:

```go
func (c *Concept) CheckPermission(userID, resource, action string) error {
    ok, err := c.store.HasPermission(userID, resource, action)
    if err != nil {
        return err
    }
    if !ok {
        return &PermissionDeniedError{User: userID, Resource: resource, Action: action}
    }
    return nil
}
```

In a sync:
```go
func (a *App) DeletePost(actorID, postID string) error {
    if err := a.Roles.CheckPermission(actorID, "post", "delete"); err != nil {
        return err  // guard — sync aborts here if permission missing
    }
    return a.Posts.Delete(postID)
}
```

---

## Common Mistakes

**Concepts importing each other**: If `package notification` imports `package user`, that's a coupling violation. Pass user IDs as strings. The concept accepts opaque identifiers, not typed objects from other concept packages.

**App logic inside a concept**: Concepts enforce only their own behavioral rules. "Only the author can delete" is a sync-layer concern. The concept's `Delete` action doesn't check authorship — the sync function does.

**Shared state between concepts**: Each concept owns its own tables/collections. If two concepts write to the same table, one of them is doing the other's job.

**Sync logic in route handlers**: Keep route handlers thin. Move any multi-concept logic into named sync functions so the intent stays readable.

**Skipping the Store interface**: Hardcoding DB calls inside the concept makes it harder to test and impossible to swap databases. Always define a `Store` interface, even for simple concepts.

**Implementing every future action**: Only implement what the current spec and request require. Over-building a concept blurs its purpose and creates unused state.

---

## Implementation Output Format

When producing code changes, return:

1. Concept implementation summary
2. Files changed
3. How each concept action maps to code
4. How state is stored
5. How synchronization is handled, if any
6. Tests added or recommended
7. Any deviations from the spec
8. Remaining risks

When editing files directly, keep the response shorter and focus on what changed and why.
