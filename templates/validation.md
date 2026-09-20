# Validation

Copy to `<project>/.claude/references/validation.md` and fill it in with this project's REAL
commands. Most reliable source: the CI workflow, because that's what already has to pass for
real.

| # | Check | cwd | Command | Expected |
|---|---|---|---|---|
| 1 | Tests | . | <command> | all pass |
| 2 | Types | . | <command> | zero errors |
| 3 | Lint | . | <command> | zero violations |
| 4 | Build | . | <command> | build completes |

## Notes
<services that need to be up · environment variables · slow checks>

---
**Watch the `cwd`.** If the config lives in `backend/pyproject.toml`, the command is
`cd backend && uv run pytest`, not `uv run pytest` from the root. A command that finds no tests
exits with code 0 and looks like a PASS.
