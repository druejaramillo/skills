---
name: setup-project-skills
description: Sets up an `## Agent skills` block in CLAUDE.md/AGENTS.md and `docs/agents/` so engineering skills know this repo's issue tracker, neutral triage states, and domain-document layout. Use for explicit first-time setup or when these conventions are missing.
---

# Setup Drue's Skills

Scaffold the per-repo configuration that the engineering skills assume:

- **Issue tracker** — where issues live (GitHub, local markdown `.scratch/`, or flat `ISSUES.md`)
- **Triage states** — the strings used for neutral workflow states
- **Domain docs** — where `CONTEXT.md` and ADRs live, and the consumer rules for reading them

This is a prompt-driven skill, not a deterministic script. It is explicit setup only: other skills may point out missing configuration but must not silently invoke it. Explore, present what you found, obtain approval, then write.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` and `.git/config` — is this a GitHub repo? Which one?
- `CLAUDE.md` and `AGENTS.md` at the repo root — does either exist? Is there already an `## Agent skills` section?
- `CONTEXT.md` and `CONTEXT-MAP.md` at the repo root
- `docs/adr/` and any `src/*/docs/adr/` directories
- `docs/agents/` — does prior output from this skill already exist?
- `.scratch/` — sign that a local-markdown issue tracker convention is already in use
- `ISSUES.md` — sign that the flat file tracker is already in use
- Existing issue and PR templates, labels, milestones, and status fields. For GitHub repositories, inspect existing labels and a representative issue before proposing new vocabulary.
- Monorepo boundaries, workspace documentation, and tracker references in existing instructions. Do not assume root-level docs apply to every workspace.

### 2. Present findings and ask

Summarise what's present and what's missing. Walk the user through the three decisions **one at a time** — present a section, get the user's answer, then move to the next.

Assume the user does not know what these terms mean. Each section starts with a short explainer. Then show the choices and the default.

**Section A — Issue tracker.**

> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-issues`, `triage`, and `to-prd` read from and write to it — they need to know whether to call `gh issue create`, write markdown files, or append to `ISSUES.md`. Pick the place you actually track work for this repo.

Choices:

- **GitHub** — issues live in the repo's GitHub Issues (uses the `gh` CLI). Propose this if `git remote` points at GitHub.
- **Local markdown** — issues live as files under `.scratch/<feature>/` in this repo (good for solo projects)
- **ISSUES.md** — a single flat file at the repo root. Best for projects that don't need a full tracker. This is the default fallback if no remote is detected.
- **Other** (Jira, Linear, etc.) — ask the user to describe the workflow; record it as freeform prose

**Section B — Triage state vocabulary.**

> Explainer: The `triage` skill records an issue's workflow state. These states describe the condition of the work, not who should implement it. Map the canonical names below to labels, fields, or header values your tracker actually uses.

The four canonical states:

- `needs-triage` — maintainer needs to evaluate
- `needs-info` — waiting on reporter
- `planned` — accepted and sufficiently understood for scheduling
- `wontfix` — will not be actioned

Default: each state's string equals its canonical name. For `ISSUES.md` trackers, these appear in the issue header line and do not require external configuration. If the tracker has no state mechanism, record that explicitly rather than inventing labels.

**Section C — Domain docs.**

> Explainer: Skills like `diagnose` and `tdd` read `CONTEXT.md` for domain language and `docs/adr/` for past architectural decisions. Confirm whether this repo has one global context or multiple (e.g. a monorepo).

- **Single-context** — one `CONTEXT.md` + `docs/adr/` at the repo root. Most repos.
- **Multi-context** — `CONTEXT-MAP.md` at the root pointing to per-context `CONTEXT.md` files.

### 3. Confirm and edit

Show the user a draft of:

- The `## Agent skills` block to add to `CLAUDE.md` or `AGENTS.md`
- The contents of `docs/agents/issue-tracker.md`, `docs/agents/triage-labels.md`, `docs/agents/domain.md`

Let them edit before writing. Do not create labels, alter tracker settings, or write repository files until the user approves this draft.

### 4. Write

**Pick the file to edit:**

- If `CLAUDE.md` exists, edit it.
- Else if `AGENTS.md` exists, edit it.
- If neither exists, ask the user which to create — don't pick for them.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa).

If an `## Agent skills` block already exists, update it in-place rather than appending a duplicate.

The block:

```markdown
## Agent skills

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage states

[one-line summary of the neutral state vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout — "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

Then write the three docs files using the seed templates in this skill folder as a starting point:

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md) — local-markdown `.scratch/` tracker
- [issue-tracker-issues-md.md](./issue-tracker-issues-md.md) — flat `ISSUES.md` tracker
- [triage-labels.md](./triage-labels.md) — label mapping
- [domain.md](./domain.md) — domain doc consumer rules + layout

For "other" issue trackers, write `docs/agents/issue-tracker.md` from scratch using the user's description.

### 5. Verify and finish

Re-read every generated file. Verify documented paths exist or are intentionally created, commands match the selected tracker, and the `## Agent skills` block appears exactly once.

Tell the user which skills will read the configuration. They can edit `docs/agents/*.md` directly later; re-run setup only to switch tracker conventions or restart from scratch.
