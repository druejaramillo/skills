---
name: concept-audit
description: >-
  Use this skill when analyzing existing software, codebases, APIs, schemas,
  or system descriptions through the lens of concept design (Daniel Jackson's
  The Essence of Software). Trigger when someone says "audit this codebase",
  "review the design of this system", "analyze my app's concepts", "are these
  concepts well-designed", "what concepts does this system have", "review my
  API design", "find overloaded concepts", "is my design modular", "identify
  the concepts in a project", "evaluate whether code matches concept specs",
  "find missing concepts", "find duplicate/redundant concepts", "map features
  to concepts", "refactor toward concept design", "analyze domain-driven design
  boundaries through concepts", or "implement audit recommendations". Also
  trigger when someone pastes or uploads code, schemas, API docs, or a system
  description and wants a conceptual design review, or asks whether features
  are well-bounded, whether functionality is correctly separated, or whether
  there are modularity or mental-model problems. Produces structured output
  covering concept inventory, quality assessment, composition analysis,
  genuine strengths, and targeted recommendations — never manufacturing
  findings where the design is sound.
---

# Concept Audit

A concept audit analyzes a system at the **conceptual level** — the level of user-facing functionality — to answer: what are the units of behavior here, are they well-formed, and where are the design problems?

A codebase is conceptually healthy when:
- user-facing functionality is organized around clear concepts
- each concept has one purpose
- concept state is local and minimal
- actions are explicit and atomic
- concepts do not directly mutate each other
- cross-concept behavior lives in synchronization/mediator code
- UI/API language maps cleanly to concept language
- tests verify operational principles and action traces
- code can be explained as a composition of independent concepts

---

## Audit Levels

Choose the lightest level that satisfies the request.

| Level | Use when asked | Output |
|-------|---------------|--------|
| **1 — Concept inventory** | "What concepts are here?" | Candidate concepts, purposes, evidence, confidence, obvious problems |
| **2 — Concept/code map** | "How does code map to concepts?" | Inventory + actions/state/files/routes/tables/synchronizations per concept |
| **3 — Design misfit audit** | "What's wrong?" or "How do I improve?" | Inventory + misfit report, severity, evidence, recommended spec changes and code changes |
| **4 — Implementation audit + patches** | "Fix the code" | Audit summary, selected changes, patches, tests, migration notes, unresolved risks |

---

## Step 1: Gather Material

Work with what's available. In rough order of usefulness:

- **Database schema / data model** — reveals state; strong indicator of concept boundaries
- **API routes or service interfaces** — reveal actions; what users can do
- **Module/service/package names** — often reflect (or violate) concept structure
- **Feature documentation or user stories** — reveal purpose; useful for naming concepts
- **Source code** — most complete but most time-consuming

Also inspect: README/product docs, UI pages/components, CLI commands, public SDK/API, tests and fixtures.

Aim to identify: what state does this system maintain? What actions can users perform? What purposes do those actions serve?

---

## Step 2: Identify Concepts

Look for coherent clusters of state + behavior that serve a recognizable user-facing purpose. For each candidate:

1. Can you name it? (one or two words)
2. Can you write a one-sentence purpose?
3. Can you sketch an operational principle?

If all three are yes, it's a concept. Record each candidate as:

```
## Candidate: Name

Purpose:
State evidence:
Action evidence:
Operational principle evidence:
Files:
Confidence:
```

Look for: nouns in routes and UI labels, verbs in handlers and service methods, tables and migrations, repeated action traces in tests, user-visible status/state, policy gates, workflows, background jobs that produce user-visible state.

**Do not** treat every table, struct, class, package, or service as a concept.

Classify each candidate:

| Classification | Meaning |
|----------------|---------|
| **Strong concept** | Clear purpose, independent, behavioral, end-to-end |
| **Weak concept** | Present but structurally scattered or partially overloaded |
| **Implementation detail** | Internal mechanism users don't experience |
| **Entity only** | Identity-bearing object but no meaningful behavior |
| **Workflow/synchronization** | Cross-concept coordination, not a standalone concept |
| **Overloaded concept** | Serves multiple separable purposes |
| **Redundant concept** | Duplicates another concept |
| **Missing concept** | User need exists but has no named, bounded representation |

---

## Step 3: Assess Each Concept

Evaluate against the **seven criteria**:

| Criterion | Test question |
|-----------|---------------|
| **User-facing** | Would a user ever directly interact with this, or is it purely internal infrastructure? |
| **Semantic** | Is it about abstract behavior and state, or about UI layout, colors, or widgets? |
| **Independent** | Can you explain what it does without mentioning another concept? Is it polymorphic over external types? |
| **Behavioral** | Does it have actions that change or observe state? Or is it just a data record? |
| **Purposive** | Does it serve one specific, valuable purpose — not two, not zero, not "manage X"? |
| **End-to-end** | Does its functionality fully deliver on its stated purpose, or is half the workflow missing? |
| **Familiar / Reusable** | Would this concept appear in a similar form in other apps? Can it be named without referencing this app? |

For each criterion that fails, note specifically why and what it implies.

Also check for overloading sub-patterns:
- **False convergence**: Two purposes that seemed like one thing
- **Piggybacking**: A new feature bolted onto an existing concept because it was convenient
- **Denied purpose**: A user need embedded in something else rather than given its own concept
- **Emergent purpose**: Users found a use the designer never intended; concept now serves two purposes

**Eigensolution check**: Does the concept's design handle a broad range of similar scenarios naturally, or does it require workarounds and special cases for near-identical situations?

**Map concept state:**
- Which tables/collections hold its state?
- Which fields are required by the concept's actions vs. accidental?
- Which state is duplicated across concepts?
- Which invariants are enforced by code, database, tests, or not at all?

**State smells:** one table contains several unrelated concepts; concept state scattered across many packages; status fields mix different lifecycles; concept state can be changed through unrelated code paths.

**Map concept actions:**
- List action names from code
- Map handlers/routes/methods/jobs to actions
- Identify blocked/error cases and tests

**Action smells:** one method performs several concept actions; CRUD-only names but behavior is richer; same action implemented in multiple places; background job mutates concept state with no corresponding concept action.

---

## Step 4: Assess Composition

Look at how concepts are combined. Classify each synchronization:

| Classification | Meaning |
|----------------|---------|
| **Healthy synchronization** | Mediator/workflow composes independent concept actions |
| **Under-synchronization** | Expected automation is missing; user must manually coordinate |
| **Over-synchronization** | Automation removes user control or creates surprise |
| **Hidden coupling** | One concept directly mutates another |
| **Integrity violation** | Composition causes a concept to behave contrary to its own spec |

**Synchronization smells:**
- Concept package imports sibling concept package for mutation
- Service method named after one concept performs another concept's behavior
- UI relies on action order not enforced by backend
- Transaction spans unrelated purposes without a named workflow
- Event handler changes state in ways no user-facing action explains

**Integrity violations**: A concept behaves differently inside the app than its standalone definition would predict. Users encounter unexpected behavior that violates their mental model.

**Concept mapping issues**: The way a concept is presented in the UI doesn't match its underlying state model. (Gmail's Label concept is a good example — labels exist on messages but the UI shows them on conversations.)

**Design rule violations** (if reviewing code):
- Does one concept's implementation import another concept's package?
- Does business logic live inside concept code instead of the sync layer?
- Do multiple concepts share a database table?

---

## Step 5: Compare to DDD Boundaries

When the codebase uses DDD or domain language:

- Compare concepts to bounded contexts
- Compare concept actions to aggregate methods
- Compare concept state to aggregate consistency boundaries
- Identify entities that are not concepts
- Identify concepts split across aggregates
- Identify aggregates with multiple concept purposes
- Identify domain services that are actually synchronization mediators

Do not force DDD terminology onto the audit. Use it only to explain current architecture and refactor options.

---

## Step 6: Rank Findings

Use severity:

- **Critical**: causes user-visible contradictions, data loss, security errors, billing/access errors, or impossible mental model
- **High**: overloaded concept, hidden coupling, missing invariant, or action bypass likely to cause bugs. The flaw is observable by users or creates genuine confusion.
- **Medium**: unclear naming, scattered state/action, missing OP tests, redundant behavior. Works but will cause maintenance pain or limit future extensibility.
- **Low**: documentation, package naming, minor concept/code mismatch. A refinement that would improve clarity but current design functions fine.

Each finding:
```
### Finding: title

Severity:
Concept(s):
Evidence:
Why it matters:
Recommendation:
Design move: Split | Merge | Unify | Specialize | Tighten | Loosen | None
```

---

## Step 7: Write the Report

Use this structure. Be honest. If a concept is well-formed, say so and move on. Don't manufacture findings.

```markdown
# Concept Audit Report: [System Name]

**Date:** [date]
**Source material:** [what was analyzed]

---

## Summary

[2–4 sentences. Overall assessment: is the conceptual design sound, mixed, or problematic?
Name the 1–2 biggest strengths and the 1–2 most important concerns.]

---

## Identified Concepts

### [Concept Name]

**Type:** Explicit | Implicit
**Inferred purpose:** [one sentence]
**Assessment:** Strong | Acceptable | Needs work | Problematic
**Criteria:** [list any that fail, or "all pass"]
**Notes:** [specific observations — keep tight]

[repeat for each concept]

---

## Composition Analysis

[How are concepts composed? Any synchronization problems (over/under-sync)?
Any integrity violations? Any mapping issues between concept state and UI presentation?
If composition is clean, say so.]

---

## Strengths

[What is genuinely well-designed. Be specific. "The Label concept is cleanly polymorphic
and correctly treats items as opaque IDs" is useful. "Good separation of concerns" is not.]

---

## Findings & Recommendations

### [Finding Title]

**Severity:** Low | Medium | High | Critical
**Problem:** [What's wrong and why it matters]
**Evidence:** [Where specifically — route, schema, feature name, etc.]
**Recommendation:** [Concrete suggestion]
**Design move:** Split | Merge | Unify | Specialize | Tighten | Loosen | None

[omit this section entirely if no findings]

---

## Concept Dependence

[Map which concepts depend on which others.
ConceptA → ConceptB  (reason: "A was included to allow users to X on B")]

[Use to show: natural explanation order, viable scope subsets, development ordering]

---

## Verdict

[One paragraph. Is this a sound conceptual design? What's the single most important thing
to address, if anything? If it's good, say it's good.]
```

---

## Codebase Audit Heuristics

### Route Heuristic

Routes often reveal concepts and actions:
- `POST /sessions` → `Session.create`
- `DELETE /sessions/{token}` → `Session.invalidate`
- `POST /invites/{token}/accept` → `Invite.accept`
- `POST /documents/{id}/restore` → `Trash.restore` or `Document.restore`
- `POST /subscriptions/{id}/cancel` → `Subscription.cancel`

If route names do not map to concept actions, investigate.

### Table Heuristic

Tables reveal state, not necessarily concepts:
- `users` may support User
- `sessions` may support Session
- `role_permissions` may support Role or Access
- `jobs` may support Job, Generation, Import, Sync, or several overloaded concepts

If one table supports many concepts, document ownership of each column.

### Service Heuristic

Services often hide overloaded concepts. A `ProjectService` with `addMember`, `uploadDocument`, `generateVideo`, `sendInvite`, and `trackUsage` is not one concept — it is likely a workflow/application service coordinating several concepts.

### Test Heuristic

Good concept tests read like operational principles:

Bad: `TestCreateUpdatesDatabaseRows`

Good: `TestInvite_CreateThenAcceptAllowsMembershipSynchronization`

### Naming Heuristic

Concept language should appear consistently in specs, routes, handlers, service methods, tests, user-facing copy, and docs. Different names for the same purpose usually indicate redundancy or conceptual drift.

---

## Common Findings and Prescriptions

### Overloaded concept
**Symptom**: One concept has several purposes.
**Prescription**: Split into independent concepts and synchronize actions.
**Example**: "Like" may be Reaction + Upvote + Recommendation.

### Hidden implementation detail pretending to be concept
**Symptom**: Concept named Queue, Cache, Worker, Blob, Row, TokenStore, or Manager but users don't experience it directly.
**Prescription**: Move it to implementation notes for the real concept.

### Entity mistaken for concept
**Symptom**: Spec only describes fields, not behavior.
**Prescription**: Identify the user-facing behavior that gives the entity purpose.

### Missing concept
**Symptom**: Behavior appears repeatedly but has no name, spec, package, or tests.
**Prescription**: Introduce concept spec, then align code around its actions.

### Concept coupling
**Symptom**: Concept action mutates another concept's state directly.
**Prescription**: Extract a synchronization mediator.

### Under-synchronization
**Symptom**: User must manually do a follow-up action that should naturally follow.
**Prescription**: Add synchronization if it preserves user control.

### Over-synchronization
**Symptom**: One action unexpectedly triggers another action users may not want.
**Prescription**: Decouple actions, add confirmation, or make automation configurable.

### Overfit feature
**Symptom**: Many special cases for similar user needs.
**Prescription**: Search for a more polymorphic concept that covers the cases with fewer primitives.

---

## Quick Reference: Design Moves

| Move | Use when |
|------|----------|
| **Split** | Concept serves multiple separable purposes |
| **Merge** | Two concepts are always used together and coupling is fundamental |
| **Unify** | Multiple similar concepts could be one general parameterized concept |
| **Specialize** | General concept is too unconstrained for a specific purpose |
| **Tighten** | Add sync: automate a workflow users always execute manually |
| **Loosen** | Remove sync: automation is removing user control they want back |
| **None** | Design is fine; note this explicitly |

---

## When Making Code Changes

When performing a Level 4 audit with code changes:

1. Select the smallest safe change.
2. Preserve public behavior unless behavior change was requested.
3. Add or update tests first when practical.
4. Extract concept actions before reorganizing packages.
5. Extract synchronization mediators before changing data models.
6. Avoid large rewrites unless the audit shows severe conceptual damage.
7. Report any migration risk.

Distinguish in the output:
- Required for correctness
- Recommended for conceptual clarity
- Optional cleanup

---

## Final Rules

Be direct. Do not merely describe architecture — judge it.

Use confidence levels where evidence is incomplete.

Do not recommend a rewrite unless the concept/code mismatch is systemic and severe.

A report where every concept has a finding is not a rigorous audit — it's manufacturing problems. If a concept passes all seven criteria, the assessment is "Strong / all pass" and you move on. If the design is sound, say so.

High-severity patterns to watch for:
- A concept whose purpose can't be stated without mentioning another concept (independence failure)
- Two clearly distinct user needs served by a single concept (overloading)
- Actions that enforce app-specific rules that belong in the sync layer (integrity violation)
- State that means different things in different contexts within the same concept (false convergence)
- A "concept" that is purely a data record with no meaningful behavior

Low-severity patterns:
- Polymorphism that could be pushed further but works as-is
- A familiar concept given an app-specific name
- A sync that could be automated but users can manage manually without friction
- Minor naming inconsistencies
