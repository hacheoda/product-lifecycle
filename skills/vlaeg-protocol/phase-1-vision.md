# Phase 1 — V: Vision (and Logic)

**Goal:** confirm what comes in, what goes out, and how the system should behave — before any design or code.

## If a pm-decision spec-draft.md exists
Check for one before asking Discovery from scratch — re-deriving what's already been decided wastes the user's time and risks drifting from the decision they already made. Map it forward:
- **Guiding Star** ← the spec's Problem + Solution sections.
- **Delivery Payload** ← the spec's Solution section (what the output looks like) and "Who it's for" (who receives it / where it lands).
- **Behavioral Rules** ← the spec's Requirements (especially the epics above the cut line) and Out of Scope.
- **Integrations** and **Source of Truth** are still almost always open — a product spec rarely names the exact APIs or data source, so ask these two directly.
Confirm the derived answers with the user in one pass ("here's what I'm carrying over from the spec, correct me if I'm reading it wrong") rather than silently assuming — the spec covers *what* and *why*, not always the technical *how*.

## Discovery questions (ask the user)
1. **Guiding Star** — What is the desired single outcome?
2. **Integrations** — What external services do we need (Slack, Shopify, etc.)? Are the keys/credentials ready?
3. **Source of Truth** — Where does the primary data live?
4. **Delivery Payload** — How and where should the final result be delivered?
5. **Behavioral Rules** — How should the system "act"? (tone, logical constraints, explicit "what not to do" rules)

## Data-first rule
Define the JSON Data Schema (input/output formats) in `vlaeg.md` before writing any code. Coding only starts once the "Payload" format is confirmed.

## Research
Search GitHub and other sources for reusable resources relevant to this project.

## Done when
All 5 discovery questions are answered, the data schema is written into `vlaeg.md`, and `task_plan.md` has an approved blueprint. Only then may Phase 0's halt be lifted.
