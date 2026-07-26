---
name: code-review
description: Review a proposed code change against its requirements and engineering standards. Use when the user asks for a code review, PR review, diff review, pre-merge review, or an independent check of a branch or commit range.
---

# Code Review

Review is an evidence-based, read-only assessment unless the user explicitly asks for a repair. Keep the user in control of scope, requirements, and any change to the worktree.

## Trigger

Use this skill for a review of a proposed change, pull request, branch, commit range, or patch. Do not use it to diagnose a running failure, implement a feature, or resolve an active merge conflict.

## Inputs

Establish these inputs before making a conclusion:

- A base and head revision, expressed as `BASE...HEAD`. If either is missing, ask for it; do not guess a target branch.
- The change's intended behavior: an issue, acceptance criteria, design note, or the user's stated goal. If none exists, label the specification assessment as incomplete rather than inventing one.
- Applicable local standards: repository instructions, formatter/linter configuration, tests, architecture decisions, and public-interface expectations.
- Any requested focus or exclusions, such as security, migration safety, or generated files.

Uncommitted work is outside a revision range. Identify it separately and ask whether it should be included; never silently fold it into the review.

## Authority

- Inspect revisions, history, code, tests, and documentation. Run read-only checks and existing verification commands only when they do not change project state.
- Do not edit files, alter revisions, stage, commit, publish, or change tracker state while reviewing.
- A repair is optional. Make one only after the user identifies the findings to address and authorizes the affected files. Do not broaden that authorization.
- Recheck only when the user asks for it or supplies a changed `BASE...HEAD` range. A recheck is a new review, not confirmation of an earlier result.

## Review Boundary

Use `git diff BASE...HEAD`, not `BASE..HEAD`, for every review pass. The three-dot range compares `HEAD` with the merge base of `BASE` and `HEAD`, which keeps the assessment limited to the proposed change.

Record the exact range, resolved merge-base revision, paths reviewed, and exclusions. Read the changed code in context, its callers and consumers, relevant tests, and the stated requirements. Follow data and control flow across changed boundaries; a line-level diff alone is not enough.

Use a fixed-point three-dot review:

1. Ground every finding in the current `BASE...HEAD` diff and surrounding evidence.
2. Before reporting, re-read the final three-dot diff and reconcile every finding with it. Remove findings that no longer apply and add newly exposed effects.
3. If the head revision changes, discard the prior clean conclusion and repeat the same three-dot assessment for the new range.

Call a review clean only when the latest fresh pass has no reportable findings in either report. Never call a diff clean merely because an earlier revision was clean.

## What To Check

Evaluate observable behavior, failure paths, data boundaries, compatibility, concurrency or lifecycle effects where relevant, security and privacy boundaries, tests, and maintainability. Prefer concrete evidence over generic style advice.

Run the narrowest relevant checks when safe and available. `git diff --check` is a baseline diff check, not behavioral verification. State checks that were not run and why.

## Required Reports

Return two separate reports. Do not merge their findings into one list.

### Standards Report

Assess the change against codebase conventions and engineering quality. For each finding include:

- Severity: `blocking`, `major`, `minor`, or `nit`
- Location and evidence
- Standard or convention affected
- Consequence
- A bounded recommendation

If no standards issue is found, state `No standards findings` and list standards that could not be assessed.

### Spec Report

Trace each stated requirement to evidence in the diff, code context, and verification. Use one of `met`, `not met`, `uncertain`, or `not specified` for each requirement. For `not met` and `uncertain`, name the missing evidence or behavior and its consequence.

If requirements are absent or contradictory, say so here. A well-styled change is not evidence that it meets an unstated specification.

### Review Record

After both reports, provide:

- Review range, merge base, paths, and exclusions
- Checks run and their results
- Residual risks and unverified assumptions
- A clear conclusion: findings remain, no findings, or unable to conclude

## Optional Repair And Recheck

When findings remain, offer choices: leave the report as-is, clarify a finding, repair selected findings, or recheck a revised range. Do not repair by default.

For an approved repair, restate the selected findings, authorized files, and intended behavior before editing. Run relevant verification, report the resulting diff, and ask whether to recheck. A requested recheck repeats both the Standards Report and Spec Report against the current `BASE...HEAD`; it does not inherit a prior pass.

## Stops

Stop and ask the user when the revision boundary, intended behavior, or authority to include sensitive/generated material is unclear. Stop with an `unable to conclude` result when required evidence, access, or safe verification is unavailable. Do not substitute assumptions for a specification.

## Verification

Before closing a review, confirm that every finding cites current evidence, the report names the exact three-dot range, both reports are present, and all check results are distinguished from unrun checks.
