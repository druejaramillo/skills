---
name: wayfinder
description: Create or update a human-owned map of a project's work, dependencies, decisions, and evidence. Use when the user needs to map a feature, initiative, backlog, or delivery path in a tracker or local Markdown without turning the map into execution.
---

# Wayfinder

Make the current work legible so a person can choose what to do next. A wayfinder map describes work and dependencies; it does not claim, assign, reserve, or execute work.

## Trigger

Use this skill to map an initiative, feature, backlog area, or delivery path. It is appropriate for questions such as "what has to happen for this launch?", "map this feature", or "show the dependencies and open decisions." Do not use it to implement tasks, triage a single defect, or change a tracker without approval.

## Inputs

Collect the stated outcome, scope boundary, constraints, known work items, material decisions, and desired time horizon. Ask whether a project tracker is configured and available. If it is not available, use local Markdown in a location the user approves.

Do not infer ownership from repository history, ticket fields, or conversation. The map has no ownership, assignee, or person-status fields.

## Map Location

Prefer an available tracker map when the user supplies or confirms an accessible tracker. A tracker map should use the tracker item identifiers and links, while keeping the same map structure below.

If no tracker is available, create a local Markdown map at an agreed path. A local map is a first-class result, not a degraded substitute. Do not create or update tracker items or local files until the user approves the target and proposed changes.

Do not launch, rely on, or make plans around automatic subagents. Do not claim, assign, reserve, or label work as belonging to any person, team, or system.

## Map Contents

Make every map contain:

- **Outcome and boundary:** what success means and what is explicitly outside the map.
- **Work items:** short, observable outcomes with a stable identifier or local heading.
- **Dependencies:** directional links between items and the condition that unblocks each link.
- **Decisions and questions:** the decision needed, available evidence, and what remains unknown.
- **Status:** `not started`, `in progress`, `blocked`, `ready to decide`, or `done`, backed by evidence where known.
- **Risks and verification:** the failure or uncertainty to watch and the evidence that would close the item.

Keep work items small enough to understand but do not manufacture detail. Highlight cycles, orphaned items, duplicate outcomes, blocked paths, and decisions whose answer changes several items.

## Human Control

First present a proposed map or delta for review. The user chooses which items exist, how they are split, which status is accurate, and whether to persist it. Preserve disagreements as questions rather than deciding them by inference.

When updating an existing map, retain stable identifiers and state what changed. If tracker access fails, report the failure and offer an agreed local Markdown map; do not silently switch locations.

## Authority

- Read code, documentation, and an available tracker to trace evidence and dependencies.
- Draft maps and update only the location and entries the user approved.
- Do not create, close, move, assign, comment on, or otherwise mutate tracker records without explicit approval for those changes.
- Do not implement mapped work, dispatch work, or represent a map as a delivery commitment.

## Output

Return the map location, outcome, ordered dependency paths, blocked items, decisions needed from the user, and the evidence behind statuses. End with the smallest set of user choices that would unblock the map, not a claim that work has begun.

## Stops

Stop when the outcome is not defined, tracker access or location is unresolved, a dependency has incompatible interpretations, or a proposed tracker change lacks approval. Record uncertainty rather than concealing it with a status.

## Verification

Confirm that every dependency points to an existing item, every non-terminal status has evidence or an explicit unknown, identifiers and links resolve in the selected location, and the map has no ownership or assignment fields. If a dependency cycle is intentional, mark it and name the decision required to break it.
