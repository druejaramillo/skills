---
name: grill
description: Relentless interview to stress-test a plan or design until every branch of the decision tree is resolved. Optionally updates CONTEXT.md and ADRs as decisions crystallize. Use when user wants to stress-test a plan, get grilled on their design, mentions "grill me", or wants to align with the agent before building.
---

# Grill

Interview the user relentlessly about every aspect of their plan until reaching shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask questions one at a time. If a question can be answered by exploring the codebase, explore it instead of asking.

## Invocation

- `/grill` — interview only; no doc updates
- `/grill --docs` — interview + update `CONTEXT.md` and ADRs inline as decisions crystallize

Check the user's invocation and set doc-update mode accordingly before starting.

---

## Doc-update mode (`--docs` only)

When `--docs` is passed, this session also maintains the project's living documentation. Before the first question, explore the codebase for existing docs:

### File structure to look for

Single-context repo (most repos):

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-another-decision.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives.

### CONTEXT.md — shared domain glossary

As terminology gets resolved during the interview, update `CONTEXT.md` with new or refined terms. Each entry:

```markdown
## term-name

One-sentence definition. What it is, not what it does.

**Avoid:** synonym-to-avoid, another-synonym
```

Only add terms that are *genuinely domain-specific* — not generic programming concepts. Prefer precision over volume. If a term is still fuzzy at the end of the session, flag it rather than writing a vague entry.

### ADRs — architectural decisions

When a significant architectural decision gets resolved (one that would be non-obvious to a future agent or developer), create an ADR under `docs/adr/`:

```markdown
# NNNN — Decision Title

**Date:** YYYY-MM-DD
**Status:** Accepted

## Context

What situation forced this decision.

## Decision

What we decided.

## Consequences

What gets easier, what gets harder, what constraints this creates.
```

Number sequentially. Only record decisions that are *genuinely architectural* — not implementation details. If the user explicitly rejects a direction, record it under "Consequences" or as a separate "Rejected alternatives" section so future sessions don't re-litigate it.

### After the session

In `--docs` mode, summarize at the end:
- New terms added to `CONTEXT.md`
- ADRs created or updated
- Any terminology that remains unresolved (flagged for a future session)
