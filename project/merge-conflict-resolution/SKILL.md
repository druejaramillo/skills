---
name: merge-conflict-resolution
description: Resolve an active Git merge, rebase, or cherry-pick conflict by tracing the intent of both sides and obtaining approval before any continuation, staging, or commit. Use when Git reports conflicted paths or the user asks to reconcile competing changes.
---

# Merge Conflict Resolution

Resolve conflicts by preserving or deliberately choosing between both changes' intent. A conflict-marker edit is not a resolution until its behavior and the operation state have been checked.

## Trigger

Use this skill for an active `git merge`, `git rebase`, or `git cherry-pick` conflict, or when the user asks to reconcile two concrete changes. Do not use it for an ordinary code review, a clean merge, or a request to implement a feature from scratch.

## Inputs

Identify the active Git operation from `git status`, conflicted paths, the two revisions or commits involved, the target behavior, and relevant test commands. State how Git names each side for the current operation; `ours` and `theirs` can have counterintuitive meanings during a rebase.

Before changing a file, inspect the common ancestor and each side's changes. Use conflict stages (`:1`, `:2`, and `:3`) when available, plus history, tests, call sites, and requirements. Do not infer intent from conflict markers alone.

## Trace Both Intents

For every conflicted path, prepare a short resolution record containing:

- Intent on one side, with its revision and evidence
- Intent on the other side, with its revision and evidence
- Shared invariants and any incompatible behavior
- Proposed merged behavior and the reason it preserves or chooses between the intents
- Open question or risk, if evidence cannot decide

Present this record before editing. If the conflict is semantic, changes a public contract, or lacks a clear precedence rule, stop for a user decision. Do not make a plausible-looking blend that changes both behaviors.

## Authority

- Inspect Git state, ancestors, revisions, conflict stages, code, tests, and documentation.
- Edit conflict files only after the user approves the proposed resolution for those paths.
- Never discard one side, use a force operation, reset, stash, or change unrelated files without explicit instruction.
- Never run an operation's continuation command, stage paths, or create a commit without explicit user approval at that point.

An approval to edit a resolution is not approval to continue the merge, rebase, or cherry-pick. An approval to continue is not approval to stage unrelated paths or create a separate commit. If continuation can create a commit, state that consequence and obtain explicit approval for it.

## Abort Is Always Available

Offer the applicable abort option before resolving and whenever the proposed resolution is disputed:

- `git merge --abort`
- `git rebase --abort`
- `git cherry-pick --abort`

Run an abort only when the user asks for it. Then verify the operation state with `git status` and report any files Git could not restore. Do not replace an abort request with a manual cleanup.

## Resolve And Verify

After approved edits, show the resolved diff and remove all conflict markers. Run `git diff --check` and the narrowest relevant tests or build checks. Report failures, skipped checks, and remaining risks.

Ask separately for:

1. Approval to stage the named resolved paths, if staging is required by the active operation.
2. Approval to run the named continuation command, including any commit effect it has.
3. Approval to create a separate commit, including its proposed message, when a separate commit is appropriate.

Do not continue automatically after checks pass. The user can instead revise the resolution or abort.

## Output

Report the operation, affected paths, both-intent records, user-approved resolution, verification results, Git status, and the next action awaiting approval. Make clear whether the repository remains in a conflicted operation.

## Stops

Stop when the active operation is unclear, either side's intent cannot be evidenced, the conflict changes behavior without a user decision, verification fails, or the required approval to stage, continue, or commit is absent.

## Verification

Before requesting continuation, confirm that no conflict markers remain in resolved files, `git diff --check` passes, intended behavior is covered by available checks or recorded as unverified, and the displayed diff matches the approved resolution. After an approved continuation or abort, inspect `git status` again and report the result.
