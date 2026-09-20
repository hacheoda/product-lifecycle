# Product Lifecycle — entry point for agents

The first thing any agent reads when opening this repository, in any tool. The real content
lives elsewhere, on purpose; keep reading only long enough to know where.

## What this is

A library of **Agent Skills** (open standard, `SKILL.md` format) that makes up a complete product
development method, from the first signal to judging the result. It is not an application with a
build or test suite. It is infrastructure for agents: 17 skills, each a directory under `skills/`.

**Read in this order:**
1. [README.md](README.md) — the map: what each skill does and why.
2. [ROUTER.md](ROUTER.md) — the one authoritative pipeline diagram and the automation/product
   branching rule.

## Activating the skills in your tool

The `SKILL.md` format is an open standard and works unmodified in any compatible tool. What
changes is only where the skill directory needs to live:

| Tool | Skills directory |
|---|---|
| Claude Code | `~/.claude/skills/<name>` |
| Google Antigravity | see [Antigravity's own docs](https://antigravity.google/docs/skills/); the exact path is theirs to change, not ours to duplicate |
| Cursor, Codex, or another Agent Skills-compatible tool | consult that tool's own documentation |

Symlink, never copy, in any tool. The activation loop is in [README.md](README.md); swap the
destination directory for your tool's own.

## Why this file exists, and stays this short

The pipeline's content (what, when, why) already lives in [README.md](README.md) and
[ROUTER.md](ROUTER.md). Rewriting that logic here would create a second copy bound to drift from
the first, exactly the bug [templates/new-project.md](templates/new-project.md) documents. This
file exists only because Claude Code, Google Antigravity, Cursor, and Codex all auto-load
`AGENTS.md` (or `CLAUDE.md`) on session start; none of them auto-load the README.

A `CLAUDE.md` sits next to this file for the same reason `AGENTS.md` exists at all: Claude Code
only started falling back to `AGENTS.md` on 2026-09-18, so an older build reads nothing here
without it. That `CLAUDE.md` must never grow past a one-line import of this file. Real content
added there would win over this file in Claude Code and go stale exactly the way this section
warns against.
