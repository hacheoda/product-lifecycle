---
name: pm-decision
description: |
  PM decision-making workflow for turning a rough idea or client signal into an
  evidence-backed product decision — framing the problem, researching evidence,
  shaping a spec, and planning validation. Use this skill whenever the user is
  evaluating whether to build something, scoping a new feature or project,
  writing a product brief or spec, researching client/user needs, weighing
  product trade-offs, or preparing a decision for stakeholder alignment — even
  if they don't say "PM" explicitly. Also offer this skill when a brand-new
  project is being kicked off and the direction isn't decided yet.
  Triggers: "should we build", "scope this", "write a spec", "product brief",
  "feature idea", "is this worth it", "shape this", "what should we build",
  "PM this", "product decision", "new project idea".
---

# PM Decision — Index

Four-phase workflow: rough idea/client signal → evidence-backed product decision.
**Output:** decision brief + validation plan.

## Claude's role
Thinking partner, not scribe, and **not the one who decides**. Push for clarity, challenge assumptions, surface trade-offs **with a recommendation**, question scope, keep tight. Don't proceed to next phase until current goal is met.
In Phase 3 the user assigns every RICE factor; you elicit, inform, challenge, and do the arithmetic.
Assigning the numbers yourself turns the score into a guess wearing a suit.

## Phases — load only the one you need
| Phase | File | Goal |
|---|---|---|
| 1. Frame | `phase-1-frame.md` | One-sentence problem statement |
| 2. Research | `phase-2-research.md` | Evidence base built |
| 3. Shape | `phase-3-shape.md` | Concrete buildable design |
| 4. Validate | `phase-4-validate.md` | Minimum-proof plan |

## Skip rules
- Problem clear + evidence exists → start at Phase 3
- Direction decided, just need spec → Phase 3 (spec section)
- Spec exists → Phase 4
- Pure exploration → never skip Phase 1

## Anti-patterns (avoid in every phase)
Solution-first framing, feature laundry lists, analysis paralysis, fake neutrality, re-litigating closed Qs, scope creep in Phase 3.

## Handoff — the router at the end of Phase 4

This workflow does not end in a document. **Classify** (which path) as soon as `spec-draft.md`
exists — the test only reads the epics. But don't **route** — leave this skill and hand off — until
`validation-plan.md` also exists: Phase 4 is the go/no-go gate on committing to build, not a data
input the classification needs. **`../../ROUTER.md` is the single source of truth for the
classification test and the full pipeline diagram — read it before routing** (a second copy of the
same test here is exactly how the two drifted apart before, discovered 2026-08-22).

**What each destination inherits from here**, once `ROUTER.md` has said where an epic goes:

| Destination | Inherits |
|---|---|
| `vlaeg-protocol` | P1+P2 → the Guiding Star · the RICE ranking → SOP build order · P4 → what the Gateway checks |
| `plan-architecture` → `piv-slice-epic` → the PIV loop | spec-draft → the input intent · one epic → one architecture pass → one slicing run |

**One exception, decided per epic, not per project:** if an epic above the cut line is itself an
architecture decision (the how IS the what — e.g. "unify two engines", "extract a module"), its own
Phase 4 hypothesis can't be well-formed before `plan-architecture` runs for it. Run architecture for
that epic first, write its Phase 4 after, and say so explicitly in `validation-plan.md`. Full
test: `ROUTER.md` → "When Phase 4 waits on architecture".

Never make the next skill re-derive what this one already answered — point it at `spec-draft.md`
and `validation-plan.md` instead of reopening the same ground. If neither path fits, this is
probably a task rather than a project: do it directly and do not install process on top of it.

## Closing the loop

The Phase 4 validation plan is not a deliverable, it is **debt** — someone has to come back and
check it. Once the thing is shipped and running, `pm-outcome-review` reads the criteria written
here and judges the result. Write Phase 4 knowing it will be quoted back verbatim, with no chance
to soften a criterion after seeing the number.

## Borrowed concepts
The RIGHT/WRONG hypothesis block, the engineering guard (Osmani's list), the MVP-as-thinnest-proof
distinction, and the two-way/one-way door check were adapted from `plan-create-prd` in
[coleam00/skills](https://github.com/coleam00/skills) (MIT). That skill is deliberately not
installed here: its triggers collide with this one's. The pieces were harvested instead.
