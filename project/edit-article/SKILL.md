---
name: edit-article
description: Edit and improve articles by restructuring sections, improving clarity, and tightening prose. Asks for style and flow preferences before beginning, or uses defaults if the user says so. Use when user wants to edit, revise, or improve an article draft.
---

# Edit Article

## Step 0 — Configuration

Before doing any editing, ask the user whether they want to configure the editing style or use defaults.

Ask this as a single question:

> Before I start, would you like to configure the editing style (tone, paragraph length, voice, audience), or should I use the defaults?

If the user says to use defaults (or equivalent — "just go", "defaults are fine", etc.), skip to Step 1 immediately using all defaults below.

If the user wants to configure, ask the following questions **one at a time**, providing the default as a suggested answer for each:

1. **Paragraph length** — How long should paragraphs be?
   - Default: short (≤240 characters)
   - Other options: medium (≤400 characters), or long (no hard limit)

2. **Tone** — What tone should the writing have?
   - Default: neutral
   - Other options: casual or formal

3. **Voice** — Should I preserve the existing grammatical voice, or standardize it?
   - Default: preserve existing
   - Other options: first person ("I/we"), second person ("you"), third person ("they/one")

4. **Audience** — Who is this written for? (e.g. "general readers", "senior engineers", "non-technical founders")
   - Default: general readers

5. **Style note** — Any specific style preferences? (e.g. "punchy, no hedging", "journalistic, short sentences", "no bullet points")
   - Default: none

Confirm the full config back to the user before proceeding.

---

## Step 1 — Structure

Divide the article into sections based on its headings. Think about the main points made in each section.

Consider that information is a directed acyclic graph — pieces of information can depend on other pieces. Make sure the order of sections respects these dependencies.

Confirm the section order with the user before rewriting.

---

## Step 2 — Rewrite each section

For each section in order:

- Rewrite to improve clarity, coherence, and flow
- Apply the configured paragraph length, tone, voice, and style note
- Preserve technical accuracy and the author's intent — don't introduce new claims
- Cut filler and hedging unless formal tone is configured
- Adjust complexity and assumed knowledge to match the configured audience

Present each rewritten section to the user and wait for approval or feedback before moving to the next.

---

## Step 3 — Final pass

After all sections are approved, offer a final pass to check:

- Transitions between sections flow naturally
- The opening and closing are strong
- Terminology is used consistently throughout
