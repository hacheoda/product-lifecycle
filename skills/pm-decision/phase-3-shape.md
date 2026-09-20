# Phase 3 — Shape the Solution

**Goal:** converge on a concrete, buildable design, with the scope ranked rather than asserted.
**Output:** `spec-draft.md`.

## How shaping works
1. Synthesize evidence (don't just list)
2. Propose a direction (concrete recommendation, not 3 options)
3. Force decisions ("new workflow or extend?", "self-serve or ops-assisted?")
4. Break the solution into **epics** and score them with RICE (below)
5. **Once every epic is scored, check it against every other one for dependency** — don't rely on
   catching this opportunistically while scoring. Record "none" explicitly rather than leaving it
   blank; blank reads as "not checked," not as "checked, no dependency." A dependency missed here
   surfaces later as a high-scoring epic that turns out it couldn't have started first anyway.
6. Draw the cut line, then group what's above it into **milestones**
7. Surface open questions (with owners), mark closed ones — don't re-litigate

## The unit is an epic, not a requirement

Each scope item here is **epic-sized**: a coherent body of work that `piv-slice-epic` will later cut
into tickets. Not an atomic requirement ("the system shall validate email format"), and not a whole
product either.

The test: if it can't be sliced into more than one ticket, it's a ticket, fold it into a neighbour.
If it needs its own architecture session, it's more than one epic, split it.

This is the load-bearing handoff of the whole pipeline. One epic here becomes one input to
`plan-architecture` and then one run of `piv-slice-epic`.

## RICE — the user scores, you challenge

Three buckets (must / should / nice) hide the difference between the best and worst thing inside a
bucket, and "must" is the bucket everything migrates into. Score instead:

```
RICE = (Reach × Impact × Confidence) ÷ Effort
```

**All four factors on a 1-5 scale.** Intercom's original RICE mixes units on purpose (Reach as a
raw count, Impact as an asymmetric 3/2/1/0.5/0.25, Confidence as a percentage, Effort as
person-months) — see intercom.com/blog/rice-simple-prioritization-for-product-managers. This
project departs from that deliberately: **scoring here is often a group exercise**, and a shared
1-5 scale with anchors is what different people actually converge on. A raw headcount or a
percentage forces a calibration argument before the real conversation can start; a bounded scale
with the same five points for everyone gets to consensus faster.

**Two costs of that choice, accepted rather than ignored:**
- **Reach and Effort lose magnitude.** A real count of 400 vs 40 is a 10x difference the raw number
  would show and 1-5 compresses. When a real number exists (a measured frequency, a clear size
  estimate), use it to decide *where* on the 1-5 an epic lands relative to the others, even though
  only the 1-5 gets recorded.
- **Impact loses its asymmetry.** The original scale weights "massive" 12x over "minimal"
  (3 ÷ 0.25); a linear 1-5 weights it only 5x. Scores will cluster more tightly than canonical RICE.
  Accepted for the same consensus reason.

**Who assigns the numbers: the user (or the group, in a team setting). Never you.**

If you assign the factors, you pick values that produce the ranking you already had in mind, and
the arithmetic becomes theatre dressing up a guess as objectivity. The user carries context you do
not have: what their client actually pays for, what burned them last time, how much a week of their
own effort really costs. In a group setting, the scoring conversation *is* the alignment — don't
shortcut it by proposing numbers for them to rubber-stamp.

**Your job around the scoring:**

- Propose the epic breakdown, and defend the split when challenged.
- Ask for the four factors, one epic at a time. Ask, then stop.
- Bring the evidence to each factor: quote what Phase 2 found, or what direct inspection of the
  code/data turned up, so the estimate is informed rather than a guess.
- **Challenge inconsistency.** Two epics with the same evidence and different Confidence, an
  Impact of 5 on something Phase 2 marked weak, an Effort that ignores a dependency you already
  surfaced: say so, with the specific evidence that contradicts the number.
- Do the arithmetic, show the ranking, and say what it implies — especially when it contradicts
  what the user expected.
- Never quietly adjust a number the user gave you. Argue for the change and let them make it.

| Factor | 1 | 3 | 5 | Anchor when scoring |
|---|---|---|---|---|
| **Reach** | almost never touched | touched regularly | touched constantly, or everything downstream depends on it | if a real frequency or dependency count exists, use it to place the epic relative to the others |
| **Impact** | minimal | medium | massive | how much it moves the outcome, per occurrence — not how urgent it feels |
| **Confidence** | pure hunch, no evidence | plausible, partially supported | backed by Phase 2 evidence, or verified by direct inspection | **read this from Phase 2 or from what you verified yourself** — never from how strongly the user wants to believe it |
| **Effort** | an afternoon | several sessions across a week or two | spans multiple weeks, or touches unknown/coupled territory | size the epic, not its tickets; unknown coupling pushes this up, verified-clean coupling pushes it down |

## Intuition is a legitimate input — Confidence is where it gets priced

The user is allowed to score from experience rather than from data. A senior PM's priors are real
information, and refusing them in favour of "only what is written down" produces worse decisions,
not more rigorous ones.

**But an intuited estimate and an evidenced one do not carry the same weight, and Confidence is what
separates them.** Reproduced in the engine, verified by direct inspection, or backed by the Phase 2
evidence base → 5. Plausible, partially supported → 3. "I just think so" → 1, and on a scale this
short that is a real penalty, not a footnote.

So intuition is welcome everywhere and is never free. The user can still rank an epic first on a
hunch; the score will simply show what that hunch is costing it, which is the honest picture rather
than a suppressed one.

**Two things to watch for and name out loud:**

- **Confidence inflation.** The single easiest number to fudge, because it feels like conviction
  rather than data. If Phase 2 marked the underlying assumption weak and the user assigns 5, say it
  plainly: the disagreement is with the evidence base they wrote, not with the scale.
- **Scoring after ranking.** If the user already knows the order they want, the factors will bend to
  produce it. Ask for the numbers before discussing the order, and if the ranking that comes out
  surprises them, treat that as the score doing its job.
- **An assumed dependency running backwards.** The default instinct is that cheap, low-risk work —
  a map of where things live, a naming pass, documentation — comes *after* the epic it describes,
  once things have settled. Often it is the reverse: if it is cheap enough, doing it first lowers
  the Effort of every epic that follows, because that low-cost item is what was making the others
  expensive to investigate in the first place. It only needs a small update afterward, folded into
  the later epic's own definition of done, not a full rewrite. Before accepting "wait until X is
  done" as given, ask whether X really needs to wait, or whether the cheap item is what has been
  making X hard to estimate.

## The cut line and the milestones

Rank by score, then draw one line: **what is committed to v1, and what is deferred.** Everything
below the line stays in the doc with its score, so a later "why isn't X in here" has an answer.

Group what's above the line into **milestones**: epics that ship together, in order. A milestone is
a delivery boundary, not a priority tier. It answers *what ships when*, where the score answered
*what matters most*.

**Always draw them, including on solo work with no external date.** The temptation is to skip
milestones when nobody is waiting, but they do two jobs that survive the absence of an audience:

- **They force a definition of "shipped".** A milestone is worthless unless you can say what has to
  be true for it to be done, and writing that down catches scope that was never actually decided.
- **They keep order separate from importance.** A high-scoring epic can still land in M2 because it
  depends on something in M1. Without a sequence axis, dependency pressure distorts the score
  instead of being recorded as what it is.

A personal project run as if it had a client is also deliberate practice for the ones that do.

## The engineering guard — what this spec must NOT decide

Shaping decides *what* and *why*. The moment it starts deciding *how*, it has stopped being a spec
and started being an implementation plan written by the wrong person at the wrong time.

Hand these to `plan-architecture` instead of settling them here: library and version choices · data
model relationships · security boundaries · testing architecture · error handling and retries ·
project structure.

Skipped engineering decisions do not disappear. They resurface as vulnerabilities. So they are not
buried, they are handed over deliberately.

## MVP — the thinnest line that proves the hypothesis

Distinct from the top-scoring epic, and the two get confused constantly:

- **The top epic** is what matters most once you have decided to build.
- **The MVP** is the thinnest slice that proves the hypothesis right or wrong, end to end.

If the hypothesis holds, build the ranked scope properly. If it fails, you threw away a slice rather
than six months.

## Door check — reversible or not

For each significant decision: **two-way door** (cheap to undo) means just build it and stop
deliberating. **One-way door** (expensive or impossible to undo) means spike first, then commit.

Spending equal deliberation on both kinds is the most common way shaping stalls.

## When alternatives genuinely needed
| | Option A | Option B |
|---|---|---|
| **Bet** | [assumption] | [assumption] |
| **Win if** | [condition] | [condition] |
| **Risk** | [breaker] | [breaker] |
| **Recommendation** | ← | |

## Spec format (write to `spec-draft.md`, <3 min read)
```
# [Name]
## Problem
[One sentence, grounded in Phase 1.]
## Who it's for
[Specific client/role/workflow.]
## Why now
[The forcing function.]
## Solution
[Proposed design — what, not how.]
## Scope — epics, scored
| Epic | Reach | Impact | Confidence | Effort | RICE | Depends on |
|---|---|---|---|---|---|---|
| ...  |       |        |            |        |      |            |
[Ranked by score. Confidence sourced from the Phase 2 evidence bar.]
## Cut line
[Committed to v1: ... | Deferred: ... — each deferred item keeps its score.]
## Milestones
[M1: which epics ship together, in order, and what has to be true for the milestone to be done.]
## MVP
[The thinnest slice that proves the hypothesis.]
## Open Questions
[Why each matters + who owns answer.]
## Closed Questions
[Decisions made + source.]
## Out of Scope
[What we're NOT building and why. Different from deferred: this is never.]
```

## Done when
Problem is one sentence. Every epic scored **by the user**, with each Confidence traceable to
Phase 2 or explicitly owned as intuition. Cut line drawn, and what is above it grouped into
milestones with a stated definition of done. Open Qs have owners.
