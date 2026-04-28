# CLAUDE.md — Practical AI IQ Quiz

> **Read this file at the start of every session before doing anything else.**
> This is the single source of truth for this project. Check CHANGELOG.md next.

---

## PROJECT INFO

| Field | Value |
|---|---|
| **Project name** | Practical AI IQ Quiz |
| **Version** | v23 (stable as of 2026-04-13) |
| **Live URL** | https://practical-ai-skills-iq.netlify.app |
| **GitHub repo** | https://github.com/smagnacca/practical-ai-iq-quiz |
| **Local Mac path** | `~/Documents/Claude/Projects/cowork-Practical AI IQ Quiz` |
| **Tech stack** | Vanilla HTML/CSS/JS — static frontend + 8 Netlify Functions (Node.js) |
| **Purpose** | Lead generation quiz that scores someone's AI readiness, delivers a personalized report, and upsells into paid courses |

---

## TECH STACK & INTEGRATIONS

| Service | Role |
|---|---|
| **Netlify** | Hosting + Serverless Functions + Forms |
| **Google Sheets** | Lead capture & data storage |
| **Resend API** | Transactional email delivery |
| **Stripe** | Payment processing for upsell |
| **Netlify Forms** | Backup form capture → email to scott.magnacca1@gmail.com |

**Env vars (set in Netlify dashboard — never hardcode):**
- `GOOGLE_SERVICE_ACCOUNT_JSON`
- `STRIPE_SECRET_KEY`
- `RESEND_API_KEY`

**Scheduled function:** Runs every 30 min to check for leads needing follow-up emails.

---

## KEY FILES

| File | What it does |
|---|---|
| `CLAUDE.md` | You are here. Read every session. |
| `CHANGELOG.md` | Full version history. Check every session. |
| `PICKUP_PROMPT.md` | Auto-updated daily with current status & action items |
| `index.html` | Quiz landing page |
| `quiz.html` | Interactive quiz UI |
| `results.html` | Personalized results/report page |
| `upsell.html` | Post-quiz upsell offer |
| `enhanced-report.html` | Premium report preview |
| `netlify/functions/` | 8 Node.js serverless functions |
| `netlify.toml` | Netlify config — root publish, scheduled function |
| `og-image.png` | Social/OG share image |
| `headshot-formal-2.png` | Scott's headshot used in report |

---

## DESIGN SYSTEM

| Token | Value |
|---|---|
| **Background** | `#0A1F15` (dark green) |
| **Primary accent** | `#C9A84C` (gold) |
| **Secondary accent** | `#EEAF00` (mango) |
| **Body font** | Inter |
| **Display font** | Merriweather |
| **Min font size** | 1rem for all interactive content |

**Animation rules:**
- Scroll reveals: IntersectionObserver only — no setTimeout batch reveals below the fold
- Typewriter effects: 1400ms stagger (reading pace), 0.7s fade/translate
- CSS animation-delay: avoid — use JS-driven timing instead

---

## FUNNEL POSITION

```
60-Second AI Quiz (top of funnel, awareness)
    ↓
→ THIS SITE: Practical AI IQ Quiz (lead capture + report)
    ↓
scottmagnacca.com (authority hub)
    ↓
4daycourse.netlify.app (paid entry course — $249)
    ↓
Storyselling Masterclass ($1,499) → AI Sales Architect ($2,499)
```

---

## GIT PUSH METHOD

**Sandbox cannot push to GitHub.** Always hand Scott this command:

```bash
cd ~/Documents/Claude/Projects/cowork-Practical\ AI\ IQ\ Quiz && rm -f .git/index.lock && git add -A && git commit -m "MESSAGE" && git push origin main
```

Netlify auto-deploys within ~90 seconds of push.

---

## SESSION START CHECKLIST

1. Read this file ✓
2. Read `CHANGELOG.md` — confirm current version
3. Read `PICKUP_PROMPT.md` — check for open action items
4. Run `git log -5` — understand recent changes
5. State: current version, what's stable, anything in progress

---

## SESSION END CHECKLIST

1. Update `CHANGELOG.md` — add entry for any changes made
2. Update `PICKUP_PROMPT.md` — refresh action items and status
3. Confirm GitHub push command given to Scott (if code changed)
4. Note any pending items for next session

---

## KNOWN ACTION ITEMS (as of 2026-04-21)

1. **Archive old session docs** — 37+ untracked files (DAILY-UPDATE-*.md, BEST-PRACTICES-*.md, etc.) should be moved to `docs/archive/` when count hits 40
2. **Commit CHANGELOG updates** — automated daily entries are unstaged
3. **v24 planning** — potential: CTA A/B test, funnel analytics, email sequence optimization

---

## ADVISORY BOARD (silent — apply on major design/copy decisions)

| Role | Perspective |
|---|---|
| Senior Website Designer | Visual hierarchy, brand consistency |
| UX Expert | Friction reduction, user flows |
| Direct Response Marketer | CTAs, conversion, urgency, offer clarity |
| Dr. Robert Cialdini | Social proof, authority, scarcity, reciprocity |
