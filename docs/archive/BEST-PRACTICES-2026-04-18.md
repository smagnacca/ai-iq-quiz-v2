# Best Practices Log — 2026-04-18 (Automated Daily Run)

## Context
Scheduled daily update ran without Scott present. Production (v23 + SEO + OG) has been stable 5 days with zero code changes. This run was pure verification + documentation.

---

## New Lessons Logged Today

### 1. Use Netlify MCP, not WebFetch, for deploy verification on this project
**Mistake to avoid:** Calling `WebFetch("https://practicalaiiq.com")` wastes tokens — the custom domain is egress-blocked by the sandbox proxy.
**What works:** `mcp__netlify-project-services-reader.get-projects` (search by name) → `mcp__netlify-deploy-services-reader.get-deploy-for-site` with site ID + deploy ID returns authoritative state, commit SHA, function inventory, secret-scan results, and cron schedule in a single call.
**Token savings:** ~400 tokens per verification vs. retry attempts.

### 2. Only act on what the scheduled task asks for
**Mistake to avoid:** Inventing code changes to "justify" a scheduled run. The task instructions say: after each major change or revision, push + save conversation + changelog + best-practices. No changes → no push. Do not fabricate deliverables.
**What works:** When there are no tracked-file diffs, produce the verification report, update CHANGELOG as a no-op entry, and update pickup prompt — nothing else.

### 3. Untracked-file accumulation is now a cleanup candidate
**Current count:** 31 untracked files in the Cowork folder — all historical daily updates, pickup prompts, and status reports from 04-07 through 04-14.
**Risk:** When it exceeds ~50, `git status` becomes noisy and new changes are harder to spot.
**Proposal for next maintenance session:** Move historical documentation to `docs/archive/YYYY-MM/` and commit as a single housekeeping PR. Keeps the root tidy without losing history.

### 4. Streamlined MCP tool schema loading
**Pattern that worked today:** Called `ToolSearch` exactly twice — once for WebFetch (to attempt domain probe), once for the two Netlify reader schemas together. Batching avoids N×round-trip overhead.
**Rule for next time:** When you know you'll need multiple MCP tools, request all their schemas in ONE `ToolSearch` call using `select:tool1,tool2,tool3`.

### 5. Same-day repeat scheduled runs → addendum, not duplicate
**Situation encountered:** The scheduled daily-update task fired twice on 2026-04-18 (01:00 EDT morning run, 15:02 EDT afternoon run). The second run had nothing new to report — zero commits, zero tracked-file diffs in the 14-hour window.
**Mistake to avoid:** Generating a full second set of daily-update + best-practices + pickup-prompt artifacts. That bloats the Cowork folder and duplicates information.
**What works:** On the second/third same-day run, append a short "ADDENDUM — Second Automated Pass" section to the existing daily-update file, add one CHANGELOG line documenting the re-verification, and only touch `PICKUP_PROMPT.md` if its timestamp needs refresh. No new artifact files.
**Why it matters:** Untracked-file count is already at 31. A second full artifact set would push it to 34+ for no new information. Keeps `git status` signal high.
**How to apply:** At the start of any automated daily run, check whether `DAILY-UPDATE-YYYY-MM-DD-AUTOMATED.md` already exists. If yes → addendum mode. If no → full artifact mode.

---

## Previously-Established Patterns — Confirmed Still Working Today

| Pattern | Status | Notes |
|---|---|---|
| Netlify auto-deploy from `main` within 90 sec | ✅ | Last deploy took 10 sec |
| Scheduled `check-followups` cron every 30 min | ✅ | Still active per deploy data |
| Secret scan baseline (0 findings) | ✅ | 1081 files clean |
| No tracked changes = no push = no Terminal one-liner needed | ✅ | Correctly skipped |
| Sandbox cannot push to GitHub (documented 04-10) | ✅ | Not attempted today |

---

## Strategies to Apply Next Session

1. **Batch tool-schema loads.** One `ToolSearch` call with `select:A,B,C` beats three sequential calls.
2. **Verify via Netlify MCP first, WebFetch second.** This project's custom domain is egress-blocked — don't waste tokens rediscovering this.
3. **Scheduled runs are verification-first.** Don't invent work. Report, document, stop.
4. **Address untracked-file cleanup at 40+ threshold.** We're at 31 — propose archive next session if it grows.
5. **When Scott returns, offer compound actions.** Example: "Clean up 31 untracked docs + verify v23 still green" as a single one-liner, rather than two separate tasks.

---

## Token Efficiency Summary — Today's Run
- **Target:** <3% of daily limit for scheduled verification run
- **Actual:** On track (no file reads > 30 lines, no full-file reads, no loops, 2 MCP calls for ground truth, 4 small writes)
- **Avoided traps:** Did not call WebFetch retry loop on blocked domain; did not invent code changes; did not read full 58KB CHANGELOG (only head); did not run cross-project scan
