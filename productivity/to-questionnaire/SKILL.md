---
name: to-questionnaire
description: Turn a sender's unresolved decision needs into a clear asynchronous questionnaire for a person who holds missing context. Use when the user needs a human-to-human discovery document, not a PRD, issue, or message to be sent.
---

# To Questionnaire

Create a reply-ready questionnaire that helps a sender obtain context from a specific recipient. This is a standalone human-to-human artifact. It does not create project plans or tickets, and it never contacts the recipient.

## Authority And Boundaries

- The user is the sender and chooses the recipient, desired decisions, and final file path.
- Interview the sender enough to understand the decision each answer must support. Do not infer recipient answers.
- Draft in the conversation until the user explicitly approves an exact save path.
- Save only to that approved path. Do not create a default file, send the document, post it, attach it, or otherwise deliver it.
- Do not invoke another skill or require the questionnaire to feed another workflow.

## Interview The Sender

Establish the following before drafting:

1. Who is the recipient, what context do they uniquely hold, and can they answer these questions?
2. What decisions must the sender make after the reply?
3. What is already known, so it does not need to be asked again?
4. What terminology, deadline, privacy constraint, or response format will help the recipient answer?
5. What exact path has the user approved if they want the questionnaire saved?

Ask follow-up questions one at a time when a missing answer would make a question vague, compound, leading, or unanswerable.

## Stop Conditions

Stop and ask the sender to clarify when:

- there is no named recipient or no evidence that they can answer;
- a requested question cannot be mapped to a sender decision;
- the sender asks to invent, predict, or fill in the recipient's answer;
- confidential details cannot be safely included; or
- the user has not approved an exact save path but requests a file operation.

## Draft The Questionnaire

Use this format:

```markdown
# Questions For <recipient or role>

## Purpose
<what decisions the sender needs to make and why this recipient is being asked>

## How To Reply
<deadline, requested format, and privacy note if supplied>

## Questions

1. **<one atomic, answerable question>**
   - Why this matters: <sender decision it informs>
   - Answer: _<stub appropriate to the requested answer format>_
```

Order questions so shared context and definitions come before dependent choices. Use neutral wording, one answerable subject per question, and answer stubs that match the expected response. Keep the decision mapping in the document so the sender can audit why each question exists.

Present the draft for the sender's review. Save it only after they approve the exact path and the final wording. Report the saved path after a successful save; never report that it was sent.

## Verify Before Reporting

- Confirm every question is answerable by the named recipient.
- Confirm every question maps to a stated sender decision.
- Confirm no question contains an invented recipient answer, hidden assumption, or compound request.
- Confirm questions are ordered by prerequisite context.
- Confirm any file write used exactly the user-approved path and no delivery action occurred.
