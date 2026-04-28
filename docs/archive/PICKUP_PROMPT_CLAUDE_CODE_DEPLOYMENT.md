# PICKUP PROMPT: Complete v23 Deployment + Automated System Setup
**For: Claude Code Agent | Project: practical-ai-iq-quiz**  
**Date: 2026-04-09 | Status: v23 code complete, commit staged, ready to push**

---

## OBJECTIVE
Deploy v23 of the Practical AI IQ Quiz (already code-complete in Cowork) to GitHub and Netlify, then set up automated deployment system across all 10 Scott projects.

---

## CRITICAL CONTEXT

### Current State
- **v23 code location:** `/Users/scottmagnacca/Projects/practical-ai-iq-quiz/` (local Mac repo)
- **Cowork location:** Synced to local repo
- **Git status:** 
  - `index.html` = staged with v23 changes ✅
  - `CHANGELOG.md` = modified, NOT staged ❌
  - `netlify/functions/send-immediate-email.js` = untracked ❌
  - Last remote commit: `49ee427` (follow-up emails fix)
- **GitHub Actions:** Workflow file `.github/workflows/deploy.yml` exists and triggers on push
- **Netlify:** Site ID = `practical-ai-skills-iq`, auth token stored in env

### v23 Improvements (Already Implemented)
1. **Immediate confirmation email** — `send-immediate-email.js` (new function)
2. **0% score ring fix** — Minimum 5% display + "Keep Learning" message
3. **Cold-start retry** — Automatic retry on submit-lead HTTP 500
4. **Mobile orientation handling** — Canvas resize on orientationchange
5. **Timer hidden on results** — Quiz countdown timer hidden when results render

### All 10 Projects (Deployment System Scope)
```
1. practical-ai-iq-quiz          → ~/Projects/practical-ai-iq-quiz
2. ai-advantage-summit           → ~/Projects/ai-advantage-summit
3. email-outreach-machine        → ~/Projects/email-outreach-machine
4. video-editing                 → ~/Projects/video-editing
5. website-design-funnel         → ~/Projects/website-design-funnel
6. cowork-4daycourse             → (Cowork-only, no local repo)
7. ceo-sales-quiz                → (Cowork-only, no local repo)
8. salesforlife                  → (Cowork-only, no local repo)
9. babson-exec-quiz              → (Cowork-only, no local repo)
10. scottmagnacca-site           → ~/Projects/scottmagnacca-site
```

### Token Access
- **GitHub token:** `[REDACTED_GITHUB_TOKEN]`
- **Netlify token:** `nfk_xxx...` (stored in env or can be retrieved from user)

---

## TASK BREAKDOWN

### PHASE 1: Deploy v23 (10 min)
**Goal:** Push v23 to GitHub → GitHub Actions triggers → Netlify deploys

**Steps:**
1. Navigate to `/Users/scottmagnacca/Projects/practical-ai-iq-quiz`
2. Stage remaining files:
   ```bash
   git add CHANGELOG.md netlify/functions/send-immediate-email.js
   ```
3. Create commit with v23 message:
   ```bash
   git commit -m "v23: 5 improvements—confirmation email, 0% ring, cold-start retry, mobile orientation, timer cleanup

- Add send-immediate-email.js: zero-delay personalized welcome email
- Enforce 5% minimum display for zero-score ring
- Implement automatic retry for submit-lead on cold-start
- Enhance canvas resize: orientationchange + debounce
- Hide quiz countdown timer when results render

Co-Authored-By: Claude Haiku 4.5 <noreply@anthropic.com>"
   ```
4. Push to GitHub:
   ```bash
   git push origin main
   ```
5. **Verify:**
   - GitHub commit appears at https://github.com/smagnacca/practical-ai-iq-quiz/commits/main
   - GitHub Actions workflow runs (check Actions tab)
   - Netlify deployment starts (check Netlify dashboard)
   - Site live at https://practical-ai-skills-iq.netlify.app (test after ~2 min)

---

### PHASE 2: Verify GitHub Actions Workflow (5 min)
**Goal:** Confirm `.github/workflows/deploy.yml` exists and triggers on push

**Steps:**
1. Check if workflow file exists:
   ```bash
   ls -l /Users/scottmagnacca/Projects/practical-ai-iq-quiz/.github/workflows/deploy.yml
   ```
2. If it doesn't exist, create it from template:
   ```yaml
   name: Deploy to Netlify
   on:
     push:
       branches: [main]
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Deploy to Netlify
           uses: nwtgck/actions-netlify@v2.0
           with:
             publish-dir: '.'
             production-branch: main
             github-token: ${{ secrets.GITHUB_TOKEN }}
             deploy-message: "Deploy from GitHub Actions"
             enable-pull-request-comment: true
           env:
             NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
             NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
   ```
3. Add GitHub Secrets to repo (if not already present):
   - `NETLIFY_AUTH_TOKEN` = (Netlify personal access token)
   - `NETLIFY_SITE_ID` = `practical-ai-skills-iq`
4. Verify workflow ran:
   ```bash
   # Check GitHub Actions API or UI for successful run
   ```

---

### PHASE 3: Set Up Automated Deployment System (15 min)
**Goal:** Create configuration to automate deployments across all 10 projects

**Steps:**
1. **Create master deployment config** at `~/.claude/tokens/DEPLOYMENT-SYSTEM.json`:
   ```json
   {
     "system_version": "1.0",
     "created": "2026-04-09T16:00:00Z",
     "projects": [
       {
         "id": "practical-ai-iq-quiz",
         "name": "Practical AI IQ Quiz",
         "cowork_folder": "cowork-Practical AI IQ Quiz",
         "local_repo": "~/Projects/practical-ai-iq-quiz",
         "github_repo": "smagnacca/practical-ai-iq-quiz",
         "netlify_site_id": "practical-ai-skills-iq",
         "has_workflow": true,
         "status": "deployed"
       },
       {
         "id": "ai-advantage-summit",
         "name": "AI Advantage Summit",
         "cowork_folder": "cowork-ai-advantage-summit",
         "local_repo": "~/Projects/ai-advantage-summit",
         "github_repo": "smagnacca/ai-advantage-summit",
         "netlify_site_id": "ai-advantage-summit",
         "has_workflow": false,
         "status": "needs_setup"
       },
       {
         "id": "email-outreach-machine",
         "name": "Email Outreach Machine",
         "cowork_folder": "cowork-email-outreach-machine",
         "local_repo": "~/Projects/email-outreach-machine",
         "github_repo": "smagnacca/email-outreach-machine",
         "netlify_site_id": "email-outreach-machine",
         "has_workflow": false,
         "status": "needs_setup"
       },
       {
         "id": "video-editing",
         "name": "Video Editing",
         "cowork_folder": "cowork-video-editing",
         "local_repo": "~/Projects/video-editing",
         "github_repo": "smagnacca/video-editing",
         "netlify_site_id": "video-editing",
         "has_workflow": false,
         "status": "needs_setup"
       },
       {
         "id": "website-design-funnel",
         "name": "Website Design Funnel",
         "cowork_folder": "cowork-website-design-funnel",
         "local_repo": "~/Projects/website-design-funnel",
         "github_repo": "smagnacca/website-design-funnel",
         "netlify_site_id": "website-design-funnel",
         "has_workflow": false,
         "status": "needs_setup"
       },
       {
         "id": "cowork-4daycourse",
         "name": "4-Day AI Course",
         "cowork_folder": "cowork-4daycourse",
         "local_repo": null,
         "github_repo": null,
         "netlify_site_id": null,
         "has_workflow": false,
         "status": "cowork_only"
       },
       {
         "id": "ceo-sales-quiz",
         "name": "CEO Sales Quiz",
         "cowork_folder": "cowork-ceo-sales-quiz",
         "local_repo": null,
         "github_repo": null,
         "netlify_site_id": null,
         "has_workflow": false,
         "status": "cowork_only"
       },
       {
         "id": "salesforlife",
         "name": "Sales for Life",
         "cowork_folder": "cowork-salesforlife",
         "local_repo": null,
         "github_repo": null,
         "netlify_site_id": null,
         "has_workflow": false,
         "status": "cowork_only"
       },
       {
         "id": "babson-exec-quiz",
         "name": "Babson Exec Quiz",
         "cowork_folder": "cowork-babson-exec-quiz",
         "local_repo": null,
         "github_repo": null,
         "netlify_site_id": null,
         "has_workflow": false,
         "status": "cowork_only"
       },
       {
         "id": "scottmagnacca-site",
         "name": "Scott Magnacca Site",
         "cowork_folder": null,
         "local_repo": "~/Projects/scottmagnacca-site",
         "github_repo": "smagnacca/scottmagnacca-site",
         "netlify_site_id": "scottmagnacca-site",
         "has_workflow": false,
         "status": "needs_setup"
       }
     ],
     "deployment_tokens": {
       "github_token": "[REDACTED_GITHUB_TOKEN]",
       "netlify_token_path": "~/.claude/tokens/.netlify_token"
     }
   }
   ```

2. **Create GitHub Actions workflow template** at `~/.claude/templates/deploy.yml` (to be copied to each repo):
   - Location: `[REPO]/.github/workflows/deploy.yml`
   - Triggers on push to main
   - Uses `nwtgck/actions-netlify@v2.0`
   - Reads `NETLIFY_AUTH_TOKEN` and `NETLIFY_SITE_ID` from GitHub secrets
   - Creates commit comment with deployment status

3. **For each repo with `status: "needs_setup"`:**
   - Verify `.github/workflows/deploy.yml` exists
   - If missing, copy from template
   - Add GitHub Secrets: `NETLIFY_AUTH_TOKEN` and `NETLIFY_SITE_ID`
   - Test by pushing a small change (e.g., update timestamp in README)

4. **Create deployment log** at `~/.claude/tokens/deployment-log.json`:
   ```json
   {
     "system": "Automated Deployment",
     "last_check": "2026-04-09T16:00:00Z",
     "deployments": [
       {
         "project": "practical-ai-iq-quiz",
         "version": "v23",
         "commit": "TBD_AFTER_PUSH",
         "deployed_at": "TBD_AFTER_GITHUB_ACTIONS",
         "status": "pending"
       }
     ]
   }
   ```

---

## EXPECTED OUTCOME

### After Phase 1 (v23 Deploy):
- ✅ v23 commit pushed to GitHub
- ✅ GitHub Actions workflow triggers automatically
- ✅ Netlify deployment starts within 30 seconds
- ✅ Site live at https://practical-ai-skills-iq.netlify.app within 2 minutes
- ✅ Commit appears on GitHub with "Deployed to Netlify" comment

### After Phase 2 (Workflow Verify):
- ✅ `.github/workflows/deploy.yml` confirmed in practical-ai-iq-quiz
- ✅ GitHub Secrets verified in repo settings
- ✅ Workflow runs on future pushes automatically

### After Phase 3 (System Setup):
- ✅ `DEPLOYMENT-SYSTEM.json` created with all 10 projects mapped
- ✅ GitHub Actions workflow template ready to deploy to other 5 repos
- ✅ Deployment log started for tracking future deployments
- ✅ System ready for automated multi-project deployments

---

## KEY CONSTRAINTS & SAFETY CHECKS

1. **Always use `git add` before `git commit`** — don't assume staged files
2. **Never force push** — all pushes are clean merges to main
3. **Verify Netlify deployment** — check dashboard before declaring success
4. **GitHub Secrets must be added via web UI** — cannot be added via CLI in this setup
5. **Token security** — GitHub token embedded in git URLs, Netlify token in env
6. **Non-blocking operations** — email functions are fire-and-forget

---

## ROLLBACK PROCEDURE (If Needed)

If v23 deployment fails:
1. Revert commit: `git reset --soft HEAD~1`
2. Fix issue in code
3. Re-commit with same message
4. Push again
5. GitHub Actions will re-run automatically

---

## SUCCESS CRITERIA

- [ ] v23 deployed to production and live
- [ ] GitHub Actions workflow confirmed working
- [ ] All 10 projects mapped in DEPLOYMENT-SYSTEM.json
- [ ] 5 "needs_setup" repos have workflow files + secrets ready
- [ ] Deployment log started for future tracking
- [ ] System is fully automated (zero manual steps for future deployments)

---

## CONTACT / QUESTIONS

Scott's preferences:
- Minimal terminal output — only errors and success confirmations
- Visual verification preferred (screenshots of Netlify dashboard, GitHub Actions)
- One command per task where possible
- Document any deviations from plan with reasons

**This prompt is complete and ready for Claude Code to execute.**
