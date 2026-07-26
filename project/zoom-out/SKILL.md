---
name: zoom-out
description: Produce an evidence-backed, read-only map of an unfamiliar subsystem, symbol, or behavior. Use when a user needs to understand how code fits into the larger system without changing it.
---

# Zoom Out

Map an unfamiliar area of a codebase without turning orientation into redesign, implementation, or triage. This method is read-only: do not edit files, change configuration, create tickets, run mutating commands, or recommend a solution as though it were established fact.

## Method

1. State the target path, symbol, behavior, or question. Ask for narrower scope when the requested area cannot be mapped honestly.
2. Read the relevant `CONTEXT.md`, `CONTEXT-MAP.md`, ADRs, and nearby documentation. Use the repository's defined domain vocabulary.
3. Trace from observed behavior to entry points, callers, modules, data or control flow, external dependencies, and tests. Use history only when it clarifies an observed relationship.
4. Keep an evidence log. Every asserted relationship needs a code, documentation, test, or history reference. Label interpretation as an inference and identify conflicting evidence rather than smoothing it over.
5. Stop if access is missing, the evidence conflicts materially, or the scope cannot be mapped without speculation.

## Output

Produce a concise map with:

```markdown
## Scope

Question and boundaries explored.

## Observed Behavior

Facts with evidence references.

## System Map

Entry points -> modules -> dependencies -> observable outcomes.

## Callers and Tests

Relevant callers, consumers, and tests with evidence references.

## Unknowns and Inferences

Open questions, access gaps, conflicts, and clearly marked inferences.

## Next Question

The smallest question the user could choose to investigate next.
```

Before completing, verify every relationship is evidenced, facts and inferences are distinguishable, and no files changed.
