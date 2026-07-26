---
name: prototype
description: Build and assess one disposable UI, state, or interface experiment that answers a named product or technical question. Use when the user asks to prototype, spike, mock up, or test a single uncertainty before committing to a full implementation.
---

# Prototype

Build the smallest runnable artifact that lets a person answer one named question. A prototype is evidence for a decision, not an early production feature.

## Trigger

Use this skill when the user wants to test one uncertain UI interaction, state transition, integration seam, or interface shape. Do not use it to implement a feature, redesign an existing product area, or explore several questions at once.

## Inputs

Get agreement on the following before writing an artifact:

- **Named question:** one sentence that can be answered with `supported`, `rejected`, or `inconclusive`. Example: `Can a reviewer understand why a payment is blocked from one compact screen?`
- **Decision and observer:** who will inspect it and what decision the verdict informs.
- **Artifact type:** UI, state transition, or interface contract.
- **Allowed location and disposal rule:** an agreed scratch or prototype location, including when it will be removed.
- **One launch command:** one documented command that a person can run from the stated location to observe the artifact.
- **Success signal:** the observation or interaction that answers the question.

If the request contains multiple questions, help choose one and defer the rest. Do not turn a vague request into a broad product build.

## Artifact Contract

Create exactly one disposable prototype artifact. Its files may live in one agreed directory, but it must be one coherent experiment with one named question and one launch command.

The artifact must:

- Display or document the named question and clearly mark itself as disposable.
- Exercise the relevant UI, state, or interface boundary directly, using realistic but non-production data where data is needed.
- Be runnable or inspectable with the single documented command.
- Make the tested behavior observable without requiring unfinished product work.
- Include a compact verdict record with the question, command used, observer, evidence, verdict, and next decision.

Keep the scope narrow. Stubs, simulated responses, and hard-coded sample data are acceptable when they isolate the question. Do not add production integrations, migrations, telemetry, credentials, broad architecture, or unrelated polish to make the prototype look complete.

## Authority

- Inspect the relevant code and existing conventions to make the experiment believable.
- Create or edit only the agreed disposable artifact and its verdict record after the user approves the question, location, and launch command.
- Do not alter production code, shared configuration, dependencies, deployment settings, tests, persistent data, staging state, or commits unless the user separately authorizes those exact changes.
- The user records or confirms the verdict. Do not declare a question supported based on an unobserved run.

## Human Evaluation

Show the question, command, and expected observation before asking the observer to run it. Capture what happened, not what was intended. Record one verdict:

- `supported` when the observation supports the named question.
- `rejected` when it does not.
- `inconclusive` when the artifact or observation cannot answer it.

The recorded verdict is required even when it is inconclusive. It prevents a disposable experiment from quietly becoming a permanent implementation.

## Output

Report the named question, artifact path, one launch command, what was observed, the recorded verdict, and the disposal rule. State explicitly which assumptions were simulated.

## Stops

Stop and ask for direction if the question cannot be named, the result would not influence a decision, the one-command constraint cannot be met without changing shared project state, or the requested work is production implementation in disguise. Stop after recording the verdict; a follow-on implementation requires a separate request.

## Verification

Verify that the documented command reaches the intended artifact, the interaction or interface exposes the chosen signal, no production files were changed, and the verdict record contains evidence rather than a prediction. If the command cannot run, record `inconclusive` and explain why.
