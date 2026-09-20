# Phase 3 — A: Architecture (The 3-Layer Build)

**Goal:** separate responsibilities so an inherently probabilistic LLM produces deterministic, reliable business logic.

## Build order, if a pm-decision spec-draft.md exists
Use its epics as the SOP backlog instead of inventing scope here: build in RICE-score order (highest first), down to the cut line — items below it wait for a future pass. This keeps what gets built traceable to a decision that was already made, instead of re-litigating priority mid-build.

## The three layers
- **Layer 1 — Architecture (`architecture/`)**
  Technical SOPs in Markdown. Defines objectives, inputs, tool logic, and edge cases.
  **Golden Rule:** if the logic changes, update the SOP *before* updating the code.
- **Layer 2 — Navigation (decision making)**
  The reasoning layer. Routes data between SOPs and Tools. Don't perform complex tasks directly — call execution tools in the correct order.
- **Layer 3 — Tools (`tools/`)**
  Deterministic, atomic, testable scripts (e.g. Python). Secrets live in `.env`. Use `.tmp/` for all intermediate file operations.

## Self-correction (the repair loop)
When a tool fails or an error occurs:
1. **Analyze** — read the stack trace and error message. Don't guess.
2. **Fix** — adjust the script in `tools/`.
3. **Test** — verify the fix works.
4. **Update architecture** — record the new learning in the relevant `architecture/*.md` file (e.g. "API requires a specific header", "rate limit is 5 calls/sec") so the error never repeats.

## Bookkeeping
- After any significant task: update `progress.md` with what happened and any errors; store discoveries in `findings.md`.
- Only update `vlaeg.md` when a schema changes, a rule is added, or the architecture is modified.
- `vlaeg.md` is the law; the other three files are working memory.

## Done when
Each capability has an SOP in `architecture/`, a corresponding tool in `tools/`, and passes its own test.
