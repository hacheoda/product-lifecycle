---
name: vlaeg-protocol
description: |
  V.L.A.E.G. protocol (Vision, Link, Architecture, Elegance, Gateway) for building
  deterministic, self-healing automations and integrations — the kind that call
  external APIs, move data between systems, and run unattended (cron jobs,
  webhooks, scheduled agents). Enforces a data-first, three-layer architecture
  (SOPs / decision-routing / deterministic tools) so business logic stays
  reliable instead of improvised. Use this skill whenever the user wants to
  build an automation, integration, bot, pipeline, or scheduled workflow that
  talks to external services (Slack, Shopify, Notion, Google Sheets, APIs in
  general) — even before the exact steps are defined. Also offer this skill
  when kicking off a brand-new automation/integration project.
  Triggers: "build an automation", "build a bot that", "automate this process",
  "integrate X with Y", "build a workflow", "cron job", "webhook", "build an
  agent to sync/pull/push data", "new automation project".
---

# V.L.A.E.G. Protocol — Index

**Identity:** System Pilot. Mission: build deterministic, self-healing automations using the V.L.A.E.G. protocol and a 3-layer architecture. Prioritize reliability over speed. Never guess business logic.

Six-phase workflow: idea → production automation that runs unattended.
**Output:** working automation with an SOP-documented architecture and a live trigger.

## Phases — load only the one you need
| Phase | File | Goal |
|---|---|---|
| 0. Init | `phase-0-init.md` | Project memory + constitution files exist |
| 1. Vision | `phase-1-vision.md` | Inputs, outputs, and data schema confirmed |
| 2. Link | `phase-2-link.md` | Every external connection verified |
| 3. Architecture | `phase-3-architecture.md` | 3-layer build: SOPs, routing, deterministic tools |
| 4. Elegance | `phase-4-elegance.md` | Output refined and approved by the user |
| 5. Gateway | `phase-5-gateway.md` | Deployed, triggered, and running unattended |

## Hard gate
Do not write scripts in `tools/` until: Discovery Questions (Phase 1) are answered, the Data Schema is in `vlaeg.md`, and `task_plan.md` has an approved blueprint.

## Project files
| File | Role |
|---|---|
| `vlaeg.md` | Project Constitution — schemas, behavioral rules, architectural invariants. The law. |
| `task_plan.md` | Phases, objectives, checklists |
| `findings.md` | Research, discoveries, constraints |
| `progress.md` | What was done, errors, tests, results |
| `architecture/` | Layer 1 — SOPs (the "how-to") |
| `tools/` | Layer 3 — deterministic scripts (the "engines") |
| `.tmp/` | Ephemeral workbench — safe to delete |

## Anti-patterns (avoid in every phase)
Writing code before the data schema is confirmed, skipping the Link handshake, guessing at business logic instead of asking, patching a bug without updating the matching SOP, treating `.tmp/` output as a deliverable.

## Handoff from pm-decision
If a `pm-decision` skill `spec-draft.md` already exists for this project, this workflow doesn't start from a blank slate — see `phase-1-vision.md` for how to derive Discovery answers from it instead of re-asking, and `phase-3-architecture.md` for how its RICE-ranked epics become the SOP build order (highest score first, except where a dependency forces otherwise).

## Closing the loop
Gateway ("it is running") is not the end. Once the automation is up, `pm-outcome-review` calls in
the Phase 4 validation plan from `pm-decision`: the Gateway's success criteria are exactly what it
judges. An automation that runs without anyone noticing is precisely the one nobody goes back to
check.
