---
name: concept-implement
description: Translate an approved concept or named synchronization into working code while preserving semantic ownership. Use when implementing a concept, mapping its state model to modules or schemas, choosing dependencies and seams from evidence, creating APIs and tests from specs, refactoring code toward concepts, or implementing the synchronization layer between concepts. Uses Go examples unless another language is specified.
---

# Concept Implementation

This skill translates concept design specs into code. Conceptual independence maps to clear **logical ownership** and action seams in code, not automatically to one package, interface, table, or deployment unit per concept. A concept owns its behavior and invariants; named synchronizations own composed behavior. Do not hide cross-concept mutations inside a concept action.

## Conceptual Direction

```
Synchronization layer
  - imports concepts
  - owns cross-concept logic
  - does not hide that logic inside a concept

Concept A                         Concept B
  - owns local state                - owns local state
  - owns local actions              - owns local actions
  - no hidden mutation of B         - no hidden mutation of A
```

This is a semantic direction, not a required folder layout. Translate it into the existing codebase only when logical ownership, public action seams, and synchronization behavior remain clear.

**Concepts:**

- Own their logical state, invariants, and behavior; physical storage may be shared when ownership remains explicit.
- Expose actions through the smallest public seam the codebase needs.
- Treat external types as opaque identities at the concept boundary unless an approved spec says otherwise.
- Do not hide a composed business rule by directly invoking or mutating another concept's owned behavior.

**Synchronization layer:**

- Owns a named, composed user-facing rule such as authorization, cascades, or automation.
- Translates incoming requests into concept action sequences.
- Gives each sync function a corresponding named synchronization from the design.
- May be an existing use-case service, command handler, event consumer, or other boundary when its consistency and retry behavior are explicit.

## Implementation Mapping

| Concept spec part | Code artifact |
|---|---|
| Concept name | package/module/service/component namespace |
| Purpose | module responsibility and public action seam; comments only where the codebase needs them |
| State | logical ownership, models, schemas, repositories, and physical storage where relevant |
| Actions | functions, methods, commands, handlers, endpoints |
| Action inputs | request DTOs, command structs, function args |
| Action outputs | response DTOs, result types, events, errors |
| Failure cases | typed errors, validation errors, blocked actions |
| Operational principle | integration test, acceptance test, action-trace test |
| Invariants | constraints, validations, property tests, transaction checks |
| Synchronization | named mediator/workflow/event handler with explicit ownership, consistency, and retry behavior |

## Implementation Procedure

### 1. Read the Concept Spec

Extract the concept name and type parameters, purpose, state components, action success and error outputs, operational principle, and any implementation projection or synchronization/coupling ledger.

If the spec is incomplete, infer conservatively and list assumptions. Do not invent broad behavior beyond the purpose.

### 2. Identify Existing Architecture

Before editing code, inspect package/module layout, data models and migrations/schema, handlers/routes/controllers, service/application and repository/data-access layers, tests and naming conventions, dependency direction and error handling, and transaction and authorization/policy patterns.

Follow the existing style unless it obscures logical ownership, bypasses an invariant, or hides a composed rule.

### 3. Write the Architecture Translation

Before selecting files or abstractions, state the smallest translation that makes the approved semantics concrete:

```
Architecture translation
- Module responsibility: [which concept behavior and invariants this code owns]
- Public action seam: [the action/API/command callers use]
- Hidden realization: [persistence, transport, indexes, framework wiring]
- Logical state ownership: [state, authorized writers, protected invariants]
- Dependencies: [dependency -> category -> evidence -> direction]
- Adapter/port decision: [none, existing seam, or new seam and the meaningful variation it serves]
- Test seams: [concept action trace and named synchronization trace]
```

Use categories such as opaque identity, composed concept action, infrastructure, framework, or presentation. Reuse a design-provided synchronization/coupling ledger; do not invent a different synchronization contract in code.

### 4. Choose Dependencies and Seams from Evidence

Start with concrete dependencies and existing action seams when they are local, stable, and have one real implementation. Introduce an adapter, port, repository interface, or additional module boundary only for meaningful variation or an actual protocol boundary: multiple providers or transports; an external service, process, or unstable protocol; independently deployed or changing implementations; an explicit failure, retry, transaction, authorization, or observability boundary; or a public action that must stay stable while its realization changes.

Tests alone do not justify a general interface. Prefer the real local realization, a focused fake, or an existing boundary. Keep an existing interface when it carries a real contract; do not add or remove one mechanically. When evidence is incomplete, choose the simplest reversible implementation and name the uncertainty.

### 5. Choose the Narrowest Implementation Slice

Implement only what the requested concept or action needs, in this order:

1. State/storage needed by the action
2. Domain/application types
3. Action implementation
4. API/handler/CLI entry point, if needed
5. Synchronization mediator, if needed
6. Tests from the operational principle

Update documentation or an ADR only when the user requests it or the codebase's established change process requires it. Do not create either merely to record this translation. Do not implement every possible future action unless requested.

### 6. Preserve Concept Independence

Avoid concept packages importing each other to mutate state directly; actions with multiple concept purposes; endpoints that hide the concept action; shared storage with unclear logical owners, writers, or invariant boundaries; concept validation scattered across another concept; rules that live only in UI code; screen-only tests; and "Manager" or "service" code containing several concepts.

Prefer one package or module per concept where practical, one action function per concept action, typed command/result structs, concrete local dependencies unless real variation justifies a seam, explicit state owners even with shared records or tables, mediators for cross-concept synchronization, explicit transaction boundaries around synchronized actions, and tests phrased as action traces.

## Detailed Reference

Read [implementation patterns and examples](references/implementation-patterns.md) before choosing a package or file projection, mapping SSF state to storage, implementing a named synchronization, defining HTTP or Go error mappings, or writing tests. It contains the Go examples, schema mapping, synchronization rules, API guidance, test patterns, multiple-instantiation guidance, and validator-action examples for those situations.

## Common Mistakes

**Concepts importing each other to hide behavior:** Do not use a sibling concept package to mutate or inspect another concept's owned behavior. Pass opaque IDs at the concept boundary. A necessary implementation dependency must have an evidence-backed category and preserve the named ownership boundary.

**App logic inside a concept:** Concepts enforce only their own behavioral rules. "Only the author can delete" is a sync-layer concern. The concept's `Delete` action does not check authorship; the sync function does.

**Shared physical state without logical ownership:** Multiple concepts may use one table, record, or transaction. It is a problem when state owner, authorized writers, invariants, or synchronization boundary are unclear, not when storage is merely co-located.

**Sync logic in route handlers:** Keep route handlers thin. Move multi-concept logic into named sync functions so the intent stays readable.

**Habitual Store interface:** Do not introduce `Store` merely because persistence exists or tests might use a fake. Use a concrete local dependency when there is one realization. Add an interface or adapter only for evidenced variation, protocol adaptation, or a stable public seam; place it at the consuming boundary.

**Implementing every future action:** Implement only what the current spec and request require. Over-building blurs a concept's purpose and creates unused state.

## Authority Boundary

The user approves public seams and decides whether architecture work discovered during implementation is a blocker, a small enabler, or a deferrable health item. Stop when a concept rule, synchronization behavior, ownership decision, or public seam is unresolved. Do not create ADRs, architecture documents, or broad refactor plans by default.

## Implementation Output Format

When producing code changes, return:

1. Architecture translation: module responsibility, public and hidden behavior, logical ownership, dependency categories, adapter/port decision, and concept/synchronization test seams
2. Concept implementation summary
3. Files changed
4. How each concept action maps to code
5. Physical realization only where it matters to logical ownership or invariants
6. How named synchronization is handled, including consistency and retry owner, if any
7. Tests added or recommended
8. Any deviations from the spec
9. Remaining risks and decisions for the user

When editing files directly, keep the response shorter and focus on what changed and why.
