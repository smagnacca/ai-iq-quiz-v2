# Best Practices Log — 2026-04-20 (Automated Daily Run)

## Context
Scheduled daily update ran without Scott present. Production (v23 + SEO + OG) has been stable 7 days with zero code changes. This run was pure verification + documentation. No new anti-patterns encountered.

---

## New Lessons Logged Today

### 1. A missed scheduled day does not mean production drift
**Situation:** No `DAILY-UPDATE-2026-04-19-AUTOMATED.md` exists. Either the scheduled task did not fire on Sunday or it produced no artifact. Today's MCP verification confirms production is identical to 04-18 (same deploy ID, same commit SHA, same function inventory).
**Mistake to avoid:** Panic-auditing code or re-pushing the last-known-good commit when a day is skipped.
**What works:** Verify current Netlify deploy state via MCP. If the commit SHA matches last-known-good and `state=ready`, there is no drift. Document the gap in today's daily-update, move on.
**How to apply:** Whenever a prior day's daily-update file is missing, add one line to today's Status section noting the gap, and confirm current state is unchanged from the last documented day.

### 2. Continuity across 4 automated runs (04-12, 04-13, 04-14, 04-18, 04-20) confirms the pattern
**What's proven:** The scheduled-task workflow — verify via Netlify MCP, write daily-update artifact, append to CHANGELOG, overwrite PICKUP_PROMPT, add a best-practices file when there's something new — has now executed autonomously 5 times without error and without generating false alarms.
**Token cost per run:** ~2-3% of daily limit when the path is verification-only (no file reads > 30 lines, 2 MCP calls, ~4 writes). This is the target — any future automated run that consumes > 5% should be investigated for a regression in the workflow.
**How to apply:** Treat this workflow as the stable reference. Only change it if a failure mode appears.

---

## Previously-Established Patterns — Confirmed Still Working Today

| Pattern | Status | Notes |
|---|---|---|
| Netlify auto-deploy from `main` within 90 sec | ✅ | Not exercised today (no push) |
| Scheduled `check-followups` cron every 30 min | ✅ | Still active per deploy data |
| Secret scan baseline (0 findings) | ✅ | 1081 files clean |
| No tracked changes = no push = no Terminal one-liner | ✅ | Correctly skipped |
| Sandbox cannot push to GitHub (documented 04-10) | ✅ | Not attempted today |
| Netlify MCP > WebFetch for this project (domain egress-blocked) | ✅ | MCP worked on second attempt after transient server error |
| Batched `ToolSearch` tool-schema loads | ✅ | Loaded 2 Netlify readers in one call |
| Addendum-mode for same-day repeat runs | n/a | Today's first run; no prior 04-20 artifact existed |

---

## Strategies to Apply Next Session

1. **If the first Netlify MCP call returns a server error, retry once before considering alternatives.** First attempt today failed transiently; second call succeeded immediately with the same payload.
2. **Verify via Netlify MCP first.** This project's custom domain is egress-blocked — don't waste tokens retrying WebFetch.
3. **Scheduled runs are verification-first.** Don't invent work. Report, document, stop.
4. **Address untracked-file cleanup at 40+ threshold.** We're still at 31 — propose archive next session if it grows.
5. **When Scott returns, offer compound actions.** Example: "Clean up 31 untracked docs + verify v23 still green" as a single one-liner, rather than two separate tasks.

---

## Token Efficiency Summary — Today's Run
- **Target:** <3% of daily limit for scheduled verification run
- **Actual:** On track (MCP tools fetched in one ToolSearch call; no full-file reads; targeted head reads of CHANGELOG and prior artifacts; 4 small writes + 1 edit)
- **Avoided traps:** Did not retry-loop WebFetch; did not invent code changes; did not rewrite the whole CHANGELOG (edit-in-place); did not create a BEST-PRACTICES file with duplicate content from 04-18 (only new lessons + diff from prior table)
