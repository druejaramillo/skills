---
name: to-issues
description: Break an understood plan, spec, or PRD into independent vertical-slice tickets with observable outcomes and dependencies. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.
---

# To Issues

Break an understood plan into independent tickets using vertical slices (tracer bullets). This skill does not automatically invoke triage, implementation, review, TDD, or a larger workflow.

## Issue tracker

Use the configured issue tracker when present. If configuration is absent, tell the user about `/setup-project-skills`; do not invoke it automatically.

**If no issue tracker is configured:** offer `ISSUES.md` at the repo root as a local fallback. Create it only after the user approves publication. See the format in [../setup-project-skills/issue-tracker-issues-md.md](../setup-project-skills/issue-tracker-issues-md.md) and auto-assign IDs sequentially from the next available ID.

## Process

### 1. Gather context

Work from the supplied plan, PRD, decision records, and conversation context. If the user passes an issue reference (issue number, URL, or `ISSUES.md` ID), fetch it from the issue tracker and read its full body and comments. Stop for a focused decision if the plan leaves product behavior or scope materially unresolved.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Issue titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

### 3. Draft vertical slices

Break the plan into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Each slice states an observable outcome and acceptance criteria without encoding ownership or a type of implementer
</vertical-slice-rules>

### 4. Use expand-migrate-contract for broad mechanical changes

When a change crosses a public interface, stored data, configuration format, or many callers, avoid one large migration ticket. Sequence the tickets as:

1. **Expand**: add the new capability compatibly while preserving the old path.
2. **Migrate**: move callers, data, fixtures, and documentation in observable increments; measure or search for remaining old-path usage.
3. **Contract**: remove the old path only after migration evidence shows it is unused and relevant checks pass.

Link the dependency edges `expand -> migrate -> contract`. A narrow change with no compatibility surface does not need this pattern. Do not turn it into horizontal layer tickets: each ticket must still deliver a verifiable state.

### 5. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories this addresses (if the source material has them)
- **Observable outcome**: what can be demonstrated or verified when this ticket is complete

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Does every slice have an observable outcome and concrete acceptance criteria?

Iterate until the user approves the breakdown. Before publishing, verify that blockers precede dependents, the graph is acyclic, and broad migrations use expand-migrate-contract where appropriate.

### 6. Publish the issues

For each approved slice, publish a new issue to the issue tracker using the template below. Use the configured neutral `needs-triage` state when the tracker supports states; do not apply ownership labels.

Publish issues in dependency order (blockers first) so you can reference real issue identifiers in the "Blocked by" field.

<issue-template>
## Parent

A reference to the parent issue (if the source was an existing issue, otherwise omit this section).

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Verification

How the observable outcome will be checked.

## Blocked by

- A reference to the blocking ticket (if any)

Or "None — can start immediately" if no blockers.

</issue-template>

Do NOT close or modify any parent issue.
