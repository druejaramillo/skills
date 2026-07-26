---
name: domain-modeling
description: Refine a project's shared terminology and detect contradictions between that language and its code, interfaces, and documentation. Use when users need a domain glossary, clearer business concepts, vocabulary alignment, or a decision about whether a terminology change needs an ADR.
---

# Domain Modeling

Make the project's language precise enough for people and code to mean the same thing. The user owns domain meaning; this skill gathers evidence, proposes definitions, and makes contradictions visible.

## Trigger

Use this skill when terms are overloaded, teams use competing names, a new concept needs definition, code and product language appear to disagree, or a glossary needs refinement. Do not use it for a mechanical rename, routine API implementation, or architecture decision that does not change domain meaning.

## Inputs

Establish the bounded area to model, the people or documents that define business meaning, the terminology already in use, relevant code and interfaces, and any existing glossary or decision records. Ask what decision the terminology must enable.

Treat existing words as evidence, not authority. Do not import generic industry definitions when local evidence gives a different meaning.

## Glossary First

Use one glossary as the normal home for stable terminology. For each term, propose:

- Canonical term and concise definition
- Scope boundary and non-examples
- Allowed aliases, deprecated names, and forbidden ambiguity
- Concrete examples or lifecycle states when useful
- Code, interface, document, or test references that use the term

Prefer a small, maintained glossary over duplicate term lists. Preserve uncertain terms as questions with their competing meanings and evidence. A glossary defines vocabulary; it is not a record of every implementation choice.

## Check Code-Language Contradictions

Inspect names and observable language in relevant types, modules, endpoints, events, schemas, errors, tests, documentation, and user-facing copy. Compare each with the proposed canonical term and its boundary.

Report contradictions with:

- The canonical definition and source evidence
- The conflicting code or interface language and location
- Classification: terminology drift, overloaded term, intentional translation, or possible behavioral mismatch
- User impact and a bounded resolution option

Do not automatically rename symbols, change APIs, or normalize text. Some mismatch is intentional for compatibility or translation; ask before treating it as a defect.

## Glossary Versus ADRs

Keep terminology in the glossary by default. Create or update an ADR sparingly, and only with user approval, when choosing between durable alternatives changes system boundaries, ownership of a concept, persistence semantics, public contracts, or another hard-to-reverse architectural commitment.

Do not create an ADR merely because a term has been defined, renamed, or clarified. If an ADR is warranted, it should reference the glossary term and record the decision, alternatives, consequences, and review trigger. The glossary remains the source for the term's definition.

## Authority

- Read the approved modeling scope, code, tests, documentation, existing glossary, and ADRs.
- Draft terminology, contradiction reports, and proposed document changes.
- Do not make persistent glossary or ADR edits without the user's approval of location and content.
- Do not change code, schemas, API contracts, migration data, or public copy without separate explicit authorization.
- Do not use a glossary decision to imply approval for a broad rename or architectural change.

## Output

Return a terminology proposal, contradiction table, unresolved questions, and a recommendation of `glossary only` or `ADR needed` for each decision. For an approved update, name the glossary or ADR location and the exact changes made.

## Stops

Stop when an authoritative meaning cannot be identified, two meanings cannot coexist safely, a contradiction may alter behavior, or an ADR-level decision lacks user direction. Keep competing definitions visible rather than selecting one by guesswork.

## Verification

Confirm every defined term has a boundary, every reported contradiction has code or document evidence, aliases do not silently reintroduce ambiguity, and each ADR recommendation meets the durable-decision threshold. Re-search the agreed scope after an approved terminology update to report remaining contradictions separately from intentionally retained compatibility names.
