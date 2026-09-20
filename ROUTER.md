# Router — which path after the decision

This file exists because the question "is this automation or product software?" is fuzzy, and a
fuzzy question answered by feel sends the project down the wrong pipeline.

**Where this decision gets made:** at the end of **Phase 4 (Validate) of `pm-decision`**. Two
separate things, easy to confuse: the TEST (which path, A/B/C) only reads `spec-draft.md`, the
epics. But you only *leave* `pm-decision` and route to the next skill once Phase 4 has also
closed, because Phase 4 is the gate on "is this worth investing in", not because the test needs
any data from `validation-plan.md`. Not at kickoff. At kickoff you still don't know what the
thing is, and guessing there is expensive. *(Exception per epic, not per project: see "When
Phase 4 waits on architecture" below. For an epic that already IS an architecture decision, the
order between Phase 4 and `plan-architecture` flips, just for that epic.)*

---

## The test

Apply it to the **epics above the cut line** in `spec-draft.md`, not to the project's general
idea.

### Path A — `vlaeg-protocol`

If **all three** are true:

1. Calls an external service you don't control (a third-party API, webhook, spreadsheet, CRM).
2. Moves or transforms data between systems.
3. Runs with nobody watching (cron, scheduled, event-triggered).

Decisive signal: **it can fail at 3am with nobody noticing.** That's what the Link phase and the
3-layer architecture exist to solve, and it's what the PIV loop doesn't cover.

### Path B — `plan-architecture` → `piv-slice-epic` → PIV loop

If the thing has an interface, users interacting in real time, and a codebase that grows ticket
by ticket over time.

Decisive signal: **the next increment is a ticket, not a pipeline step.**

### Path C — both

The most common case in client work, and the one a naive router gets wrong: a product with a
scheduled sync job, a SaaS that pulls data from an ERP every night, an app with a Slack bot.

Rule: **the product goes down B, and each component that passes the A test goes down A, inside
it.** The automation doesn't lose VLAEG treatment just because it lives inside a product. In
practice:

- `plan-architecture` decides the overall architecture and **marks which components are VLAEG**.
- `piv-slice-epic` slices the rest into tickets.
- Each VLAEG component runs its own Vision → Gateway cycle and re-enters the product as a
  finished piece.

### None of the three

If it fails both A and B, it's probably not a project. It's a task. Do it directly and don't
install process on top.

---

## When Phase 4 waits on architecture

A different question from the one above, same shape: it also applies **per epic**, not to the
whole project. A single `spec-draft.md` can mix both types. The test above classifies what TYPE
of thing an epic is; this one classifies WHEN that epic's validation (Phase 4) can be written
properly.

**Question:** for this epic, is the HOW already the WHAT? The technical decision is the epic
itself. There's no way to separate "what to build" from "how to build it", they're the same
sentence. Examples: unifying two calculation engines that currently diverge, extracting a module
out of a monolithic file, swapping the persistence mechanism. Compare with "add real-time
collaboration": you can validate whether the user wants that without deciding WebSocket vs.
polling. The how stays open after the what has already been validated.

- **If yes** (the how is the what): this epic's Phase 4 isn't well-formed before an architecture
  decision exists to test. Without one, the hypothesis can only measure the effect of a cheaper
  epic riding along with it (typically a mapping/prep epic), never the effect of the expensive
  epic it claims to be testing. Run `plan-architecture` for this epic before writing its Phase 4,
  and mark explicitly in `validation-plan.md` that the hypothesis was written post-architecture,
  and why. A small technical spike works as a minimal version of this, if the full architecture
  session is too expensive to run just for Phase 4's sake.
- **If no** (user/business behavior, architecture is a separate choice): Phase 4 runs before, in
  the diagram's standard order below. One-off technical feasibility uses a small spike
  (`phase-4-validate.md` → "Method by assumption type" table → "Technically feasible →
  Spike/PoC"), not the full architecture session.

**Sign the classification came out wrong, after the fact:** a Phase 4 whose RIGHT criterion can
only be satisfied by the cheap epic riding along, never by the expensive epic it claims to test.
And the opposite: a genuinely product-shaped epic that reaches the end of Phase 4 with no
hypothesis at all, because all the attention went to the architecture epics next to it.

---

## The full pipeline

```
                            pm-decision
        Phase 1 Frame → Phase 2 Research → Phase 3 Shape → Phase 4 Validate
                                  │
                    spec-draft.md + validation-plan.md
                                  │
                          ┌───────┴───────┐   ← ROUTER: classifies the epics
                          │               │
                A: automation       B: product software
                          │               │
                  vlaeg-protocol    plan-architecture
                  V → L → A → E → G       │
                          ▲               │  marks the components that
                          │               │  pass the 3am test
                          └───────────────┤
                          │   path C      │
                          └──────────────►│  each one runs the A cycle
                                          │  and comes back as a finished piece
                                          │
                                    piv-slice-epic
                                          │
                          ┌──── PIV loop, per ticket ────┐
                          │  prime-codebase              │
                          │  piv-plan-implementation     │
                          │  piv-implement               │
                          │  piv-validate                │
                          │  piv-review-changes          │
                          │  piv-fix-review-findings     │
                          │  piv-commit → piv-create-pr  │
                          └──────────────────────────────┘
                          │               │
                          └───────┬───────┘
                                  │
                           shipped, running
                                  │
                         pm-outcome-review
                       (did the hypothesis hold?)
                                  │
              evidence → back to Phase 2, or becomes the next Phase 1
```

**Why C isn't a third branch.** It isn't a sibling destination to A and B, it's B with A nested
inside. The whole product goes down B; each component that passes the 3am test steps out at
`plan-architecture`, runs the full VLAEG cycle on its own, and comes back as a finished piece with
a defined contract. None of that becomes a PIV ticket, which is why `piv-slice-epic` has Step 2.5.

**This is the pipeline's only diagram.** If it needs to change, change it here. `README.md`
points here instead of keeping its own copy, because two versions of the same drawing diverge
within a week.

**The Phase 3 → Phase 4 → router order in the diagram is the default, not a rigid rule per
epic.** When a specific epic is itself an architecture decision, see "When Phase 4 waits on
architecture" above. THAT epic's Phase 4 runs after `plan-architecture`, not before, even if the
rest of the project follows the diagram's order.

## What is NOT the router's call

- **Parallelism.** `worktree-create` / `worktree-merge` come in when `piv-slice-epic` marks
  tickets as independent. That's execution optimization, not a path choice.
- **Maintaining the system itself.** `skills-create` and `rules-check-drift` run outside the
  pipeline, when `pm-outcome-review` points to a method change.
