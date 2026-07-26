---
name: to-prd
description: Turn already-discussed work into a faithful, durable PRD. Use when user wants to create a PRD from the current context, a decision record, a prototype result, or an existing issue.
---

This skill takes the available conversation context and codebase understanding and produces a PRD. It is synthesis, not a new interview: do not reopen settled questions or invent decisions. It may follow a conversation, grill, prototype, or issue, but none is required.

An issue tracker is needed only to publish the approved PRD. If tracker configuration is absent, tell the user and ask whether they want to run `/setup-project-skills`; do not invoke it automatically.

## Process

1. Collect the sources: relevant conversation, confirmed decision records, prototype results, issue history, and codebase evidence. Explore the repository as needed to verify current constraints. Use the project's domain glossary vocabulary throughout the PRD and respect applicable ADRs.

2. Trace each material requirement to a source decision or evidence. If scope, public behavior, an architectural trade-off, or a test seam is materially unresolved, stop and ask the focused question needed to complete the PRD. Do not convert uncertainty into an implementation prescription.

3. Describe the likely modules and public seams at a durable level. Look for opportunities to encapsulate complexity behind simple, testable interfaces, but do not prescribe file paths, code snippets, or a speculative internal structure.

4. Draft the PRD below and show it for approval. Publish only after the user approves the draft. Do not automatically add a workflow state or label; if the selected tracker requires one, use a configured neutral state only with the user's approval.

Before completion, check that material requirements have traceable sources, behavior and test seams are concrete, and no stale file-path prescription was invented.

<prd-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As a <user>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Decision Traceability

For each material requirement or exclusion, cite the relevant conversation decision, decision record, issue, prototype result, or codebase evidence. Mark any assumption explicitly.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this PRD.

## Further Notes

Any further notes about the feature.

</prd-template>
