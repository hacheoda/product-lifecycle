# Product Lifecycle

My method for developing a product, from the first signal to judging the result.

A composite master skill: 17 skills that form a single pipeline, versioned here and
**symlinked** into `~/.claude/skills/`. They stay active in every project, but exist in one
place only. Edit here and it's true everywhere, instantly.

```bash
# recreate the symlinks (new skill, or new machine)
for d in ~/Documents/product-lifecycle/skills/*/; do
  ln -sfn "$d" ~/.claude/skills/"$(basename "$d")"
done
```

Never copy a skill from here into a project. The symlink is what keeps two copies from
diverging. An agent opening this repo cold starts at [AGENTS.md](AGENTS.md), the one file every
tool auto-loads on its own.

## The pipeline

```
pm-decision → [ROUTER] → A: vlaeg-protocol
                       └ B: plan-architecture → piv-slice-epic → PIV loop
                                                      └ C: VLAEG components leave and come back here
                                    ↓
                            pm-outcome-review
```

**The full diagram, with path C drawn out, is in [ROUTER.md](ROUTER.md)**, the one and only, on
purpose. The branch happens at the end of **Phase 4**: the test itself only reads
`spec-draft.md`, but you only leave `pm-decision` once Phase 4 (the "is this worth investing in?"
gate) has also closed. Not at kickoff, where you don't yet know what the thing even is.
*(Exception per epic: see `ROUTER.md` → "When Phase 4 waits on architecture".)*

## The stages

### 1. Decide what to build — `pm-decision`

**Why it exists:** so you don't build the wrong thing competently. It's the phase that separates
*the problem* from *the first solution that showed up*, and the only one that produces a
criterion the result can later be judged against.

**How it runs:** four phases, each with a gate. No phase advances until the current one's goal is
met.

| Phase | The question | Ships with |
|---|---|---|
| Phase 1 Frame | what's the problem, whose, why now, and what we're **not** solving | problem statement in one sentence, with boundaries |
| Phase 2 Research | how do we know this is real | evidence base, with what's weak marked as weak |
| Phase 3 Shape | what's the solution, and what's left out | `spec-draft.md`: epics scored with RICE, cut line, milestones |
| Phase 4 Validate | what has to be true, and how we'd know we were wrong | `validation-plan.md` with a RIGHT/WRONG hypothesis |

**Skip rules:** problem is clear and evidence already exists → start at Phase 3. Spec already
exists → Phase 4. Pure exploration → never skip Phase 1.

### 2. Route — `ROUTER.md`

**Why it exists:** automation and product software break in different ways and need different
disciplines. Choosing wrong is expensive, and choosing at kickoff is guessing.

**How it runs:** at the end of Phase 4, classifies the **epics above the cut line** (not the
whole project). The test is in [ROUTER.md](ROUTER.md). Path A's decisive signal: it can fail at
3am with nobody noticing.

### 3A. Build automation — `vlaeg-protocol`

**Why it exists:** what runs unsupervised fails silently. The causes are always the same, an
external connection that changes without warning and business logic improvised inside the code.

**How it runs:** five phases, and one hard gate: no script gets written before the data schema is
confirmed.

| Phase | What it guarantees |
|---|---|
| **V**ision | inputs, outputs, and data schema confirmed |
| **L**ink | every external connection actually tested before any logic |
| **A**rchitecture | three layers: SOPs (the how-to), decision-routing, deterministic tools |
| **E**legance | refined and approved output |
| **G**ateway | live, firing, running on its own |

### 3B. Build product software — the PIV loop

**PIV = Plan, Implement, Validate.** The cycle that runs **once per ticket**. The other `piv-*`
skills are the steps around it: review, fix, commit, open a PR.

**Why it exists:** an AI agent makes fewer mistakes when the plan is written and reviewed
*before* the code, and when each ticket is small enough to be proven on its own. The loop exists
to prevent the two classic failures: implementing without a plan, and committing without proof.

**Before the loop, only twice:**

| Skill | What it does | Why |
|---|---|---|
| `plan-architecture` | decides stack, data model, boundaries, and spikes | these are expensive decisions to reverse, and the spec deliberately didn't make them. Also marks which components go to VLAEG (path C) |
| `piv-slice-epic` | slices into tickets with a dependency graph | a ticket that's too big loses the agent's thread; too small becomes ceremony. Also marks what can run in parallel |

**Inside the loop, per ticket:**

| Skill | What it does | Why |
|---|---|---|
| `prime-codebase` | orients the agent in the code before planning | a plan made without reading the code plans against a guess |
| `piv-plan-implementation` | **P** — a detailed plan for the ticket, with the files, the patterns to follow, and the tests | this is where you review the reasoning, while it's still cheap to change |
| `piv-implement` | **I** — executes the plan, task by task | follows the approved plan instead of improvising |
| `piv-validate` | **V** — runs the project's suite, one PASS/FAIL verdict | without this, "it worked" is an opinion. Reads the commands from `<project>/.claude/references/validation.md` |
| `piv-review-changes` | technical review of what changed | a fresh look before the commit, not after the merge |
| `piv-fix-review-findings` | triages the findings: fix now or defer with a record | keeps a review from becoming a list nobody addresses |
| `piv-commit` | one atomic commit, with a conventional message | a readable history is what makes the mistake reversible |
| `piv-create-pr` | push and a PR with a real body | the PR carries the why, not just the diff |

**When tickets are independent:** `worktree-create` spins up several copies of the repo in
parallel, each on its own branch, and `worktree-merge` integrates them through one integration
branch, validating at every step.

### 4. Judge the result — `pm-outcome-review`

**Why it exists:** a decision that is never judged is not a decision, it's a preference. It's the
phase everyone skips, and the one that closes the loop.

**How it runs:** reads Phase 4's criteria **verbatim, before looking at any number**, and gives a
verdict per bet: confirmed, falsified, or inconclusive. Then a decision: double down, adjust,
watch until a date, or kill. Ends by naming what the result teaches about the method itself.

**The hard guard:** a criterion never gets rewritten after seeing the result. If the criterion
was badly written, that becomes a finding about the method, never an excuse to move the
scoreboard.

### 5. Maintain the system itself

| Skill | What it does | When |
|---|---|---|
| `skills-create` | creates or refactors a skill | when `pm-outcome-review`, or plain friction, reveals the method is wrong |
| `rules-check-drift` | checks whether `CLAUDE.md` still holds true after changes | before a merge, or alongside a review |

## What lives here

| Folder | What |
|---|---|
| `skills/` | The 17 in the pipeline. Active via symlink |
| `references/` | Commit and PR conventions |
| `templates/` | Blank forms and the new-project bootstrap |
| `reference/cole-skills-not-installed/` | Storage. 19 skills outside the workflow, **none active** |

**Doesn't live here:** real project artifacts. `spec-draft.md`, a client's PRD and architecture
doc live in that project's own repo. Neither does the validation command list, which lives in
`<project>/.claude/references/validation.md`.

## How this evolves

`pm-outcome-review` ends with a section on what the outcome teaches about the method. When it
points to a concrete change, that change becomes a commit here, via `skills-create`. A finding
that doesn't become a file doesn't exist.

It's already happened once: running `pm-decision` on a real project surfaced four missing
concepts, harvested from a skill that had been discarded (see "Borrowed concepts" in
[pm-decision](skills/pm-decision/SKILL.md)).

## License

MIT, see [LICENSE](LICENSE).

## Credit

`pm-decision` and `pm-outcome-review` are mine. `vlaeg-protocol` implements the V.L.A.E.G. method
taught by [Enzo Sparo](https://www.youtube.com/@EnzoSparo); this skill is my own write-up of it as
a Claude Code skill, not a copy of his material. The rest were adapted from
[coleam00/skills](https://github.com/coleam00/skills), also MIT
([original notice preserved here](skills/LICENSE-cole-medin-MIT)). `piv-validate` was rewritten
to work centralized.
