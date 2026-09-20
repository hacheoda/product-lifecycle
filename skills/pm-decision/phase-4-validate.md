# Phase 4 — Validate

**Goal:** minimum evidence needed before committing to build.

⚠️ **Before writing this phase for an epic, check if it needs architecture first.** If the epic
itself IS an architecture decision (the how is the what — e.g. "unify two engines"), its hypothesis
can't be well-formed yet. See `ROUTER.md` → "When Phase 4 waits on architecture".

## Build the plan — for every major assumption
1. What needs to be true for this to work? (top 2–3 risky assumptions)
2. Cheapest way to test each? (prototype, customer call, wizard-of-oz, data pull)
3. What would change our mind? (falsification condition upfront)
4. Who do we need to talk to? (specific clients)

## Write the hypothesis with both conditions

A falsification condition stated as prose gets softened later. Use the block, and fill **both**
lines. The WRONG line is the one people skip, and it is the one that makes the bet falsifiable.

```
We believe [change] will cause [these users] to [do Y], resulting in [outcome].
We'll know we're RIGHT if [leading signal] within [timeframe].
We'll know we're WRONG if [counter-signal, or a guardrail moves].
```

**No hypothesis ships without a WRONG line.** `pm-outcome-review` reads these back verbatim after
the thing is live, so write them knowing they will be quoted, not paraphrased. A criterion that
cannot be measured is a criterion that will be quietly re-interpreted in your favour.

**Metrics must be outcome-shaped**, never activity-shaped. "Engagement", "adoption", and "usage"
are activity. Name the metric, the target, and how it gets measured.

## Method by assumption type
| Assumption | Best method |
|---|---|
| "Clients want this" | 5 problem-framed client calls |
| "Technically feasible" | Spike / PoC |
| "Workflow works" | Wizard-of-oz with one client |
| "Clients will pay" | Pricing conversation in discovery |
| "Better than alternatives" | Comparative usability test |

## Done when
Plan has specific assumptions + specific tests + specific success criteria + specific owners.
