---
name: pm-outcome-review
description: |
  Closes the product loop after ship: takes the validation plan and hypothesis
  that were written BEFORE building (pm-decision Phase 3/4) and judges them
  against what actually happened. Produces an honest verdict per bet
  (confirmed / falsified / inconclusive) and a decision (double down, adjust,
  or kill), plus what the outcome teaches about how the decision was made.
  Use this whenever something that was shipped needs its result judged — a
  feature that has been live for a while, an automation running in production,
  a bet whose success criteria were set upfront. This is NOT a code retro and
  NOT a test suite: it judges the product bet, not the implementation.
  Triggers: "did it work", "did the hypothesis hold", "post-launch review",
  "we shipped it, now what", "was it worth it", "should we keep investing in
  this", "product retro", "outcome review", "check the success criteria",
  "kill or double down".
---

# PM Outcome Review — did the bet pay off?

The last phase of the product lifecycle, and the one everyone skips. A decision that is never
judged is not a decision, it is a preference. **Output:** `outcome-review.md`.

## Claude's role
Honest scorekeeper, not defence lawyer. You did not make this bet, so you have nothing to protect.
Push back when the user rewrites history, softens a criterion, or treats activity as evidence.

## Inputs — find them before asking
Look for these in the project before interviewing the user:
| Artifact | Where it comes from | What you need from it |
|---|---|---|
| `spec-draft.md` | `pm-decision` Phase 3 | the epics above the cut line, and their RICE scores — including the Confidence that was claimed |
| validation plan | `pm-decision` Phase 4 | assumptions, tests, success criteria, falsification conditions, owners |
| `vlaeg.md` + Gateway checks | `vlaeg-protocol` | for automations: what "done" was defined as |
| shipped reality | git log, PRs, the running system | what actually got built vs what was specified |

If the pre-build criteria genuinely do not exist, say so plainly and stop. **You cannot score a
bet whose criteria were never written.** Offer to write them now for the *next* cycle instead of
inventing them retroactively.

## The one hard guard — no moving the goalposts

Read the success criteria and the falsification condition **before** looking at any result data,
and quote them verbatim in the output. Once you have seen the numbers you are no longer allowed to
decide what "success" meant.

If a criterion turns out to have been badly written (unmeasurable, ambiguous, gamed by a proxy),
that is a **finding about the method**, logged in its own section. It is never a reason to restate
the criterion so the result passes.

## Process

### 1. Recover the bets
List every assumption from the validation plan with its success criterion, its falsification
condition, and its timeframe. Verbatim. No paraphrase, no rounding.

### 2. Collect what actually happened
Evidence, not impressions. Per bet: the measurement, the source, the window it covers. Where a
number does not exist, write **"not measured"** rather than a qualitative substitute. A wall of
"it seems better" is the failure mode this whole skill exists to prevent.

Watch for the two common frauds:
- **Activity as outcome** — shipped, used, clicked. None of these are the outcome unless the
  criterion said so.
- **Survivor evidence** — the clients who stayed and liked it. Ask who churned or never adopted.

### 3. Verdict per bet
Exactly one of three, and **inconclusive is the most common honest answer:**
- **CONFIRMED** — the leading signal hit inside the timeframe.
- **FALSIFIED** — the counter-signal fired, or a guardrail moved the wrong way.
- **INCONCLUSIVE** — not measured, too early, or the test was confounded. Say what would have to
  be measured, and by when, to resolve it.

Never average the verdicts into an overall grade. A product can confirm its main bet and still be
falsified on the guardrail that matters more.

### 4. The decision
One of four, with reasoning, and it must follow from the verdicts:
**Double down** (evidence supports more investment) · **Adjust** (the problem is real, the
solution is not) · **Watch** (inconclusive, with a named date and metric to re-check) · **Kill**
(falsified, or confirmed-but-not-worth-it).

Killing something that works but does not earn its maintenance is a legitimate outcome. Name it.

### 5. What this teaches about the method
The part that makes the folder a system instead of an archive. Two questions:
- **Where was the decision process wrong?** A criterion that could not be measured, evidence
  skipped in Phase 2, scope that grew silently in Phase 3, a risky assumption never listed.
- **What should change in the AI layer?** If a specific step would have caught it, say which skill
  or rule should change, and how. Route real changes through `skills-create`.

## Output format — `outcome-review.md`, under a 3 minute read

```
# Outcome Review — <name> · <date> · window covered: <period>

## What was promised
<one line per epic above the cut line, and whether it shipped or not. Where RICE's Confidence
missed badly, that's a finding about the method, log it in the learning section>

## Bet scoreboard
| Bet | Criterion (verbatim) | What happened | Source | Verdict |

## Decision
<double down | adjust | watch until <date> | kill>, because <reasoning tied to the verdicts>

## Measurement gaps
<what wasn't measured and should have been>

## What this teaches about the method
<where the decision process failed, and the concrete proposed change>
```

## Anti-patterns
Rewriting the criterion after seeing the number · treating delivery as the outcome · forcing a
binary verdict when the data doesn't exist · confusing this with a code retro
(`system-execution-report`) or a test suite (`piv-validate`) · turning the method section into
generic self-criticism with no concrete proposed change.

## Feeds back into
A CONFIRMED verdict becomes reusable evidence in Phase 2 of
[pm-decision](../pm-decision/phase-2-research.md).
A FALSIFIED verdict becomes the next Phase 1 (the problem is still there, the solution wasn't it).
Method changes become real edits to the skills in this folder, never a loose note.
