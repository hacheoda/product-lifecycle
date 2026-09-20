# Phase 5 — G: Gateway (Deployment / Trigger)

**Goal:** the system runs on its own, in production, without hand-holding.

## Steps
1. **Cloud transfer** — move finalized logic from local testing to the production environment.
2. **Automation** — configure execution triggers (cron jobs, webhooks, listeners).
3. **Documentation** — finalize the maintenance log in `vlaeg.md` for long-term stability.

## Deliverables vs. intermediates
- **Local (`.tmp/`)** — collected data, logs, temp files. Ephemeral, safe to delete.
- **Global (cloud)** — the actual Payload: Google Sheets, databases, or UI updates. A project is only "Done" once the payload lives in its final destination in the cloud.

## Done when
The trigger is live, the payload lands in its final destination unattended, and `vlaeg.md`'s maintenance log is up to date. If a `pm-decision` validation plan exists for this project, "done" also means the automation satisfies the success criteria it defined — deploying isn't the finish line, meeting the reason the project was approved is.
