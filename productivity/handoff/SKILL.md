---
name: handoff
description: Create a complete, redacted handoff for a future work session or a planning-to-building transition. Use when the user needs to preserve context, decisions, verified state, blockers, and a next objective for a person returning later.
---

# Handoff

Create a self-contained context-transfer document for a human reader. This is a standalone utility, not a workflow boundary: it does not launch an agent or assume a later tool or skill will run.

## Authority And Boundaries

- The user chooses the next-session objective and may choose a final storage path.
- Use the operating system temporary directory by default: resolve `TMPDIR`, then `TEMP`, then `TMP`, and otherwise use `/tmp`.
- If the user overrides the location, use only their approved path. Do not infer a repository location.
- Never launch an agent, create a commit, push, open a pull request, send a message, or publish the handoff.
- Reference canonical specifications, ADRs, issues, documents, and URLs. Do not copy settled artifacts into the handoff unless the user explicitly asks for an excerpt.

## Gather The Minimum Context

Ask only for information not already available:

1. What should the next reader accomplish first?
2. What decision, blocker, or uncertainty must they handle?
3. Which existing artifacts are canonical for this work?
4. Where should the handoff be saved, if not in the OS temporary directory?

Inspect the stated working tree, relevant artifact paths or URLs, and any reported verification commands when that information is available. Mark facts not directly checked as reported rather than verified.

## Stop Conditions

Stop and ask the user for direction when:

- a secret, credential, private key, authentication header, session cookie, access token, or similarly sensitive value cannot be safely removed;
- a canonical artifact location is unclear or does not resolve;
- the next objective is too vague to give a reader a usable first action; or
- the proposed handoff would require an external action.

Redact sensitive values rather than reproducing them. Replace them with a brief marker such as `[redacted: API token]`; retain only non-sensitive context needed to understand the work.

## Write The Handoff

Use this format:

```markdown
# Handoff: <short topic>

## Next Objective
<the user-selected first outcome>

## Current State
- Verified: <fact and evidence>
- Reported, not verified: <fact and source>

## Canonical Artifacts
- <artifact name>: <resolving path or URL> - <why it is authoritative>

## Decisions And Constraints
- <confirmed decision or constraint>

## Working Tree And Verification
- Working tree: <checked status, or unavailable>
- Verification: <command or check> - <result, or not run>

## Remaining Work
1. <concrete next action>
2. <concrete next action>

## Blockers And Open Questions
- <owner or decision needed>

## First Human Decision
<the next reader's first decision, if any>
```

Distinguish confirmed decisions, observed facts, reported claims, and open questions. Include enough orientation for a fresh reader to begin without relying on the prior conversation.

Save the document after completing the content. With no user-approved override, create a unique file named `handoff-<timestamp>.md` in the resolved OS temporary directory. Report the final absolute path in the response.

## Verify Before Reporting

- Confirm every listed local path exists and every supplied URL is syntactically complete.
- Confirm working-tree and verification statements match checks actually performed.
- Confirm settled work is referenced through canonical artifacts rather than duplicated.
- Scan the final content for credentials and sensitive personal or operational data; redact or stop if safe redaction is not possible.
- Read the handoff as a fresh reader: the next objective, first action, and unresolved decision must be clear.
