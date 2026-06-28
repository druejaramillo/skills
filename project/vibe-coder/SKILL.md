---
name: vibe-coder
description: Seven-phase interactive coding workflow for implementing a feature or fix from a spec, issue, or rough prompt. Use when the user wants the agent to build something step-by-step with approval gates between phases, especially when they want planning first and strict one-slice-at-a-time TDD after design.
---

# Vibe Coder

Implement through explicit gated phases. Do one phase, present the artifact for that phase, then stop for user approval, edits, or corrections before moving on.

The user may provide a full spec, a bug report, an issue, partial design notes, or only a rough prompt. Always still walk phases `0-6`. Treat user-provided material as draft input to validate and refine, not as permission to skip phases.

If the target code is Go, read [references/go-testing.md](./references/go-testing.md) before phase `4`. Follow its testing guidance, especially table-driven tests with subtests for organizing related scenarios.

## Hard Rules

- Explore the codebase first. Do not invent local conventions, architecture, or APIs without checking.
- Ask questions only when codebase exploration cannot answer them. Ask them one at a time.
- Stop after every phase and wait for user approval before continuing.
- Never silently jump from one phase to another in the same response.
- Phases `4-6` are a strict TDD loop on exactly one TODO slice at a time.
- No horizontal slicing. Never write multiple tests first or implement multiple TODO bullets at once.
- Tests must verify observable behavior through public interfaces, not implementation details.
- Before phase `4`, create or confirm a runnable feedback loop: the exact test command or narrow verification command that proves fail/pass.
- Scaffolding is allowed in phases `1-3`, but do not implement full behavior ahead of the red test.
- In phases `4-6`, always report the command run and a short result summary.

## Default Interaction Pattern

At the end of each phase:

1. Show the artifact for that phase.
2. Call out assumptions, open questions, and any recommended edits.
3. Ask for approval to proceed to the next phase.

Use direct phase handoffs such as: `Approve phase 2, or tell me what to change before I continue.`

## Phase 0 - Research And Plan

Understand what is being built and how it fits the existing codebase.

Do this:

- Read the relevant code paths, tests, docs, glossary, and ADRs if present.
- Restate the request in project language.
- Identify the affected modules, callers, data flow, and constraints.
- Resolve obvious ambiguity from the codebase before asking the user.
- Identify the intended feedback loop for later TDD work.
- Propose an implementation plan with risks and tradeoffs.

Output:

- Problem statement
- Relevant code map
- Constraints and assumptions
- Proposed plan
- Proposed feedback loop for phases `4-6`

Stop and wait for approval.

## Phase 1 - Define Data Structures

Design the core data model before fleshing out behavior.

Do this:

- Define entities, value objects, state shapes, payloads, and error/result shapes.
- Prefer small interfaces and deep modules.
- Simplify parameters where possible.
- If useful, add light scaffolding such as type definitions or empty structural shells.
- Avoid speculative structures that are not needed by the plan.

Output:

- Proposed data structures and invariants
- Any type or schema scaffolding added
- Why these structures are sufficient

Stop and wait for approval.

## Phase 2 - Define API, Skeleton, And Function Signatures

Lock the public seams before detailed implementation.

Do this:

- Define public APIs, module boundaries, function signatures, and class or component skeletons.
- Design for testability: accept dependencies, return useful results, keep the surface area small.
- Prefer behavior-facing seams over internal helper seams.
- If useful, add skeleton files, stubs, or placeholder functions without real behavior.

Output:

- Public APIs and signatures
- Module/file layout changes
- Any scaffolding added
- Why these seams are the right test targets

Stop and wait for approval.

## Phase 3 - Define Detailed TODOs For All Functions

Turn the design into an ordered sequence of vertical slices.

Do this:

- Break the work into detailed TODO bullets.
- Cover every function or behavior implied by phase `2`.
- Order TODOs so each item can be implemented as one tracer-bullet slice.
- Phrase each TODO in terms of observable behavior, not internal mechanics.
- Mark dependencies between TODOs.
- Recommend which TODO should be first.

Every TODO should be small enough to run through phases `4-6` by itself.

Output:

- Ordered TODO list
- Recommended first slice
- Any slices that feel too large and need to be split further

Stop and wait for approval.

## Phase 4 - Create Red Test

Pick the next unchecked TODO and write exactly one test for it.

Do this:

- Choose one TODO slice.
- Write one behavior-focused test through the public interface.
- Run the narrowest relevant test command.
- Confirm the test fails for the expected reason.
- If the test passes immediately, the test is wrong or the slice is already implemented. Fix the phase before continuing.

Output:

- The TODO slice being implemented
- The new or updated test
- The command run
- Short summary of why it failed as expected

Stop and wait for approval.

## Phase 5 - Write Minimal Code To Make The Test Green

Implement only enough behavior for the current failing test.

Do this:

- Change the minimum amount of production code needed for the current test.
- Do not implement future TODOs.
- Do not broaden abstractions early unless the current test requires it.
- Run the same narrow test command first.
- If needed, run a slightly broader relevant command to catch obvious breakage.

Output:

- The code change for this slice
- The command run
- Short summary of why the test now passes
- Any follow-on risks noticed but intentionally deferred

Stop and wait for approval.

## Phase 6 - Refactor If Worthwhile

Improve the code only after the current slice is green.

Do this:

- Refactor only if it clearly improves duplication, naming, module depth, or readability.
- Keep the public behavior the same.
- Re-run the relevant tests after each meaningful refactor.
- If no refactor is justified, say so explicitly.

Output:

- Refactor performed, or explicit statement that no refactor was worthwhile
- The command run
- Short summary confirming tests stayed green

Stop and wait for approval.

## Loop Rule After Phase 6

If TODOs remain:

1. Return to phase `4`.
2. Pick the next unchecked TODO.
3. Repeat phases `4 -> 5 -> 6` for that single slice.

If no TODOs remain:

- Summarize completed slices.
- Report final verification status.
- Ask whether the user wants any follow-up cleanup, broader test runs, or documentation updates.

## Quality Bar

Before leaving any phase, check:

- The current artifact matches the current phase only.
- Work from future phases has not leaked in unnecessarily.
- The user has enough context to approve or correct the phase.
- The next phase is clear and bounded.

## Anti-Patterns

- Writing all tests before writing implementation
- Defining TODOs as internal helper tasks instead of behaviors
- Testing private methods or internal call sequences
- Implementing several TODO bullets under one red test
- Refactoring while still red
- Continuing past a phase boundary without user confirmation
