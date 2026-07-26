---
name: grill
description: Resolve meaningful product, design, planning, or architecture decisions through evidence and a focused interview. Records confirmed decisions by default. Use when user wants to stress-test a plan, get grilled on a design, mentions "grill me", or needs to align before building.
---

# Grill

Resolve the decisions that materially affect a plan without turning every unknown into a question. Explore facts first, distinguish them from choices, recommend an option, and let the user decide. Do not treat this as mandatory prework for coding or as a substitute for a broader workflow.

If a question can be answered from the codebase, documentation, or a reliable external source, investigate it rather than asking. Cite the evidence that matters. Stop when evidence conflicts, the scope changes, or a material decision remains unresolved.

## Invocation

- `/grill` records confirmed decisions.
- `/grill --interview-only` does not create or update durable documentation.

Treat an explicit request for an interview-only session as the same opt-out. Do not infer the opt-out from a brief session or from the absence of existing documentation.

## Decision Practice

1. Frame the goal, affected users or systems, constraints, and decisions already made. Read relevant `CONTEXT.md`, `CONTEXT-MAP.md`, ADRs, and existing decision records before asking questions.
2. Build a small decision map. Label each item as a **fact**, **assumption**, **decision**, or **open question**. Facts need evidence; decisions need user confirmation unless the user has already confirmed them in the supplied material.
3. Resolve questions in the right shape:
   - Batch independent, unblocked, low-to-moderate-stakes questions. Keep a batch short, give a recommendation for each, and make clear that answers can be given independently.
   - Ask one question at a time when it depends on an earlier answer, changes scope or public behavior, is costly to reverse, or has material architectural, security, or data consequences.
   - After an answer is ambiguous, summarize the proposed decision and obtain confirmation before recording it.
4. Do not ask questions that have no meaningful effect on the selected direction. Do not silently make product, scope, public-behavior, or consequential architecture decisions.

## Record Confirmed Decisions

Unless interview-only mode is active, record each confirmed decision as it crystallizes. Prefer an existing repository decision-record convention. Otherwise create or append a focused record at `docs/decisions/YYYY-MM-DD-<topic>.md`.

Each entry contains:

```markdown
## Decision: <short title>

**Status:** Confirmed
**Source:** User confirmation on YYYY-MM-DD, plus relevant evidence

### Context

Why this decision was needed.

### Decision

What was chosen.

### Consequences

What this enables, constrains, or deliberately excludes.
```

Record decisions, not a transcript. Do not write assumptions as decisions. At the end, re-read the record and confirm it accurately reflects the user's choice.

### Domain glossary

Add or refine `CONTEXT.md` only for genuinely domain-specific terminology that was resolved:

```markdown
## term-name

One-sentence definition. What it is, not what it does.

**Avoid:** synonym-to-avoid, another-synonym
```

Do not use the glossary for generic programming terms or implementation prescriptions. Flag fuzzy terms rather than recording vague definitions.

### ADRs

Create or update an ADR only for a consequential, non-obvious, and difficult-to-reverse architectural trade-off. Number it sequentially under the repository's ADR location and include context, the decision, consequences, and rejected alternatives when useful. Do not create ADRs for ordinary implementation details.

## Completion

Summarize:

- Confirmed decisions and their evidence or confirmation source.
- Decision record, glossary, and ADR paths created or updated, unless interview-only mode was selected.
- Assumptions, unresolved questions, and the specific decision needed to proceed.
