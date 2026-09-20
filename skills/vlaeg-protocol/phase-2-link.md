# Phase 2 — L: Link (Connectivity)

**Goal:** every external connection is verified before logic is built on top of it.

## Steps
1. **Verification** — test all API connections and `.env` credentials.
2. **Handshake** — build minimal scripts in `tools/` that confirm each external service responds correctly.

## Rule
Do not proceed to full logic (Phase 3) if the Link is broken. A broken connection blocks everything downstream — fix it here, not later.

## Done when
Every integration named in Phase 1 has a working handshake script and returns a real response.
