# Cole's skills that are NOT part of this workflow

Storage only. **Nothing in this folder is active** — none of it is symlinked into
`~/.claude/skills/`, so none of it triggers.

## Why keep them at all

Two reasons, both proven in practice:

1. **Harvesting.** `plan-create-prd` was cut from the install because its triggers collide with
   `pm-decision`. Reading it later turned up four concepts worth borrowing, which now live inside
   `pm-decision` (see its "Borrowed concepts" section). Keeping the source local means the next
   harvest does not need a re-clone.
2. **Reversibility.** If one of these turns out to be needed, it is one symlink away.

## What is here (19 skills)

| Skill | Why it is not in the workflow |
|---|---|
| `plan-create-prd` | Trigger collision with `pm-decision`. Concepts harvested instead |
| `plan-create-stories` | Writes backlogs to Jira/GitHub; no tracker in use |
| `prime-backend`, `prime-frontend` | Narrower variants of `prime-codebase`, which is installed |
| `piv-investigate-issue`, `piv-implement-issue` | GitHub-issue-driven flow; not how work arrives here |
| `piv-review-pr` | Reviews PRs on GitHub; `piv-review-changes` covers the local gate |
| `piv-run-full-loop` | Chains the whole loop unattended. The loop is run step by step on purpose |
| `worktree-*` | *(installed — not in this folder)* |
| `rules-create-global` | A customisable `/init`. Not needed yet |
| `ablate-ai-layer` | Tests whether rules still earn their place. Useful once the rules file has grown |
| `opportunity-scan` | Finds what to encode next from real usage. Worth revisiting after a few real runs |
| `system-execution-report`, `system-evolution-review` | Process retros. `pm-outcome-review` covers the product side; these cover the method side and may be worth adding later |
| `second-brain-audit` | Finds stale facts in notes |
| `hooks-create` | Authors hooks. Fold in when a hook is actually needed |
| `build-dark-factory` | Fully autonomous shipping. Deliberately out of scope |
| `agent-browser`, `ast-grep` | Tooling skills, not workflow steps |
| `setup-ai-tutor` | Specific to Cole's course sample project |

## License

MIT, from [coleam00/skills](https://github.com/coleam00/skills). See
`../../skills/LICENSE-cole-medin-MIT`.
