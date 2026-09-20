# Conventions

Read by `piv-commit` (the `## commit` section) and `piv-create-pr` (the `## pr` section).

To apply in a project, copy this file to `<project>/.claude/references/conventions.md` and adjust
the scopes. Skills read from the project, not from here.

## commit

Conventional Commits, one imperative subject line, lowercase, no trailing period, up to 72
chars.

```
<type>(<scope>): <what changes>

<optional body: the why, not the what (the diff already covers what changed)>
```

**Types:** `feat` `fix` `refactor` `perf` `test` `docs` `build` `ci` `chore`

**Rules:**
- One commit = one concern. If the subject needs an "and", that's two commits.
- The body explains the reason and the trade-off, never recaps the diff.
- No "wip", "tweaks", "various fixes".
- No AI co-author unless explicitly asked.

## pr

**Title:** same format as the commit subject.

**Body:**

```markdown
## What changes
<2-4 lines, in English, for someone who didn't follow the ticket>

## Why
<the problem, with a link to the ticket / PRD / spec>

## How to validate
<exact commands and what to expect to see>

## Validation status
<piv-validate's output: PASS/FAIL per check>

## Risks and what was left out
<what could break, what was deliberately deferred>
```

**Rules:**
- One PR per ticket, one branch per ticket.
- A PR without the validation section filled in doesn't open.
- What was deferred becomes a linked issue, never just a sentence in the PR body.
