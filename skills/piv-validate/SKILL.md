---
name: piv-validate
description: Runs this project's full validation suite — tests, type checks, linting and build across every part of the stack — then reports one PASS/FAIL verdict. Reads the project's own command list from .claude/references/validation.md, and discovers it from the project if that file does not exist yet. Use before committing, before opening a PR, or after finishing a chunk of work to confirm zero regressions.
---

# Validate

Runs every check for this project and returns **one PASS/FAIL verdict**.

> Adapted from Cole Medin's original (MIT) to live centralized in `product-lifecycle/skills/`.
> The original shipped a placeholder command list that had to be edited per project. Since this
> copy is symlinked and shared across every project, editing the list here would break the
> others. So the list lives in the project, and this skill reads it.

## 1. Load the project's command list

Read `.claude/references/validation.md` at the project root.

**If the file exists:** it's the source of truth. Run exactly the listed commands, in order, with
the `cwd` each one declares. Don't improvise, don't add checks that aren't there, don't swap a
command for an "equivalent" one.

**If it doesn't exist:** don't guess and don't run a random `npm test`. Discover and propose:

1. Look for the real commands in `package.json` (scripts), `pyproject.toml`, `Makefile`,
   `justfile`, `docker-compose.yml`, the CI workflow in `.github/workflows/`, and the README. CI
   is the best source: it's what already has to pass for real.
2. Build the list in the format from section 3 below.
3. **Show the list to the user and ask for confirmation** before running anything.
4. Once confirmed, write it to `.claude/references/validation.md` for next time.

## 2. Run

- In the declared order. **Keep going after a failure** so the report covers everything.
- Capture the full output of every command that fails.
- Never "fix" code in the middle of a validation run. This skill measures, it doesn't correct.

**The two classic reasons a validator lies:**

1. **Wrong working directory.** Most tools only find their config from the current directory. If
   the config lives in `backend/pyproject.toml`, the command is `cd backend && uv run pytest`,
   not `uv run pytest` from the root. A command that finds no tests exits with code 0 and looks
   like a PASS.
2. **A command that doesn't exist.** `npm run typecheck` in a project without that script fails
   for the wrong reason. If a command fails with "script not found" or "command not found", that's
   a problem with the list, not the code. Report it separately.

## 3. Format of `.claude/references/validation.md`

```markdown
# Validation

| # | Check | cwd | Command | Expected |
|---|---|---|---|---|
| 1 | Tests | backend | uv run pytest -q | all pass |
| 2 | Types | backend | uv run mypy . | zero errors |
| 3 | Lint | backend | uv run ruff check . | zero violations |
| 4 | Tests | frontend | npm test -- --run | all pass |
| 5 | Types | frontend | npx tsc --noEmit | zero errors |
| 6 | Build | frontend | npm run build | build completes |

## Notes
<services that need to be up, environment variables required, slow checks>
```

Avoid POSIX-only syntax (`lsof`, `python3`, background-and-`kill`) if anyone on the team uses
Windows.

## 4. Report

```
# Validation — <project> · <date>

| # | Check | Result |
|---|---|---|
| 1 | Tests backend | PASS |
| 2 | Types backend  | FAIL |

## VERDICT: FAIL

## Failures
### 2. Types backend
<captured output, enough to diagnose>

## Problems with the list itself
<commands that don't exist or ran in the wrong cwd, if any>
```

**PASS only if every check passes.** There's no "PASS with caveats". If a check couldn't run, the
verdict is FAIL and the reason is the list, not the code.
