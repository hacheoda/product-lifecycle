# New project bootstrap

Checklist of what to copy and create when a project enters the pipeline. The skills are already
active via symlink in `~/.claude/skills/`; no skill needs to be copied.

## 1. In the project's repo

```bash
mkdir -p .claude/references
cp ~/Documents/product-lifecycle/references/conventions.md .claude/references/
cp ~/Documents/product-lifecycle/templates/validation.md   .claude/references/
```

Then **fill in `validation.md`** with the real commands. Without this, `piv-validate` stops and
asks instead of running, which is the correct behavior but costs a round trip.

## 2. Where the artifacts live

In the project's own repo, not in `product-lifecycle`:

```
docs/
├── spec-draft.md        ← pm-decision Phase 3
├── validation-plan.md   ← pm-decision Phase 4
├── architecture.md      ← plan-architecture  (path B)
├── tickets/             ← piv-slice-epic
└── outcome-review.md    ← pm-outcome-review, after ship
```

For path A (VLAEG), the structure is the protocol's own: `vlaeg.md`, `task_plan.md`,
`findings.md`, `progress.md`, `architecture/`, `tools/`.

**A project that already existed before this pipeline, with a document in VLAEG's format
(`vlaeg.md` / `project.md` as a "Project Constitution"), but that the router classifies as B.**
This happens with a personal project that started solo, with no process, and only later enters
the pipeline. It's a real inconsistency (the document promises automation, the router says
product), not a cosmetic one, but don't force the migration the moment you notice it. The content
(schema, business rules, invariants) stays valid regardless of the title. Restructuring mid-plan,
before the final shape of Phase 3/4 is known, risks doing the work twice. Log it as an explicit
open item, resolved **once planning finishes, before development starts**: whatever's already in
the old document stays where it is until then, only what gets written from the decision onward
goes into `docs/`.

**When you do resolve it, the content file is rarely the most serious bug.** It's common for the
project's own `CLAUDE.md`/`AGENTS.md` to declare, on its first line, "this project follows the
V.L.A.E.G. protocol" or the equivalent, and that's the statement governing the agent's behavior
every session, not a content doc's title. Fixing only `project.md`/`vlaeg.md` and leaving that
declaration as is resolves the cosmetic half and ignores the operating half. Also check whether a
frozen local copy of the protocol exists (a `vlaeg_protocol.md` or equivalent, referenced only by
that line in `CLAUDE.md`). If the global skill already covers the same ground, it's a candidate
for deletion, not for preservation "just in case": a frozen copy diverges from the live skill
silently, and nobody notices until someone follows it by mistake. *(Real finding, not
hypothetical, see `RESUME.md`, Session 2, financial-planning project, 2026-08-22.)*

## 3. Order

1. `pm-decision`, until you have `spec-draft.md` and a validation plan.
2. Classify with [ROUTER.md](../ROUTER.md), at the end of Phase 4 (exception per epic: see "When
   Phase 4 waits on architecture" in `ROUTER.md`).
3. Follow path A, B, or C.
4. Project's `CLAUDE.md`/`AGENTS.md`: generate it once the architecture is decided, not before.
5. Project's `RESUME.md`: before the first session ends.

## 4. Debt to collect

Phase 4's validation plan has a date. Schedule `pm-outcome-review` for after it. A validation
plan nobody comes back to check is decoration.
