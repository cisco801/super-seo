---
name: local-seo-pipeline
description: |
  Master 12-week orchestrator for the local SEO system. Sequences all 5 local-seo skills
  across a structured execution plan, tracks progress per week, and can resume from any
  point. Start, pause, resume, check status, or jump to any week.
user-invocable: true
argument-hint: "[start | resume | status | week <N> | reset]"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO: 12-Week Pipeline Orchestrator

**Command:** $ARGUMENTS

Master orchestrator that sequences all 5 local-seo skills (`local-seo-context`, `local-seo-gbp`, `local-seo-website`, `local-seo-authority`, `local-seo-content`) across a 12-week execution plan.

## Argument Parsing

Parse `$ARGUMENTS` for the subcommand:

- **`start`** = begin the 12-week plan from week 1
- **`resume`** = pick up where we left off (default if pipeline already started)
- **`status`** = show progress table
- **`week <N>`** = run a specific week (1-12) regardless of sequence
- **`reset`** = reset all progress (with confirmation)
- **(empty / no args)** = if pipeline exists, show status; otherwise prompt to start

---

## Phase 0: Prerequisite (Every Run)

1. Read `./local-seo-context.json` to get the active business slug
2. Read `~/.claude/config/local-seo/{slug}.json` to load full business context
3. If no context file exists and the command is `start`, run `/local-seo-context setup` as the first step of week 1
4. If no context file exists and the command is NOT `start`, stop and tell the user: "No business context found. Run `/local-seo-pipeline start` or `/local-seo-context setup` first."
5. Create `./reports/` and `./deliverables/` directories if they do not exist

**Success criteria:** Business context loaded with business name, city, services, competitors, and target keywords.

---

## 12-Week Schedule

## Site Type Routing

Before starting any week, check `business.site_type` from the context JSON. The pipeline adapts its schedule based on the detected site type:

### For `local_service` (default): Run ALL 12 weeks as defined below.

### For `saas_product`:
Skip weeks focused on local-only audits. Modified schedule:
- Week 1: Foundation + Technical (same)
- Week 2: SKIP GBP reviews/posts → run keyword-gap + money-page-audit instead
- Week 3: SKIP GBP services/description/photos → run service-pages (feature landing pages) instead
- Week 4: Website keywords (same)
- Week 5: Feature/comparison page generation (adapt service-pages for SaaS comparison pages)
- Week 6: GSC analysis + review sentiment (same — review sentiment applies to G2/Capterra reviews)
- Week 7: Backlink audit (same) + SKIP citation-audit and spam-audit → run entity-optimization instead
- Week 8: Search intent + content gap (same)
- Week 9: Entity optimization (same — focus on Product Hunt, G2, Capterra, SaaS directories)
- Week 10: Competitor content analysis (adapt competitor-posts to competitor content strategy)
- Week 11: Monthly report (same)
- Week 12: Re-audit (same)

### For `free_tool`:
- Week 1: Foundation + Technical (same)
- Week 2: SKIP GBP → run keyword-gap focused on per-feature landing pages
- Week 3: SKIP GBP → generate landing pages per tool variant (e.g., /wifi-qr-code, /vcard-qr-code)
- Week 4-6: Content creation sprint (one landing page per feature/variant)
- Week 7: Backlink audit + tool directory submissions
- Week 8: Search intent + content gap (same)
- Week 9: Entity optimization (schema per page, tool directories)
- Week 10: Competitor analysis (what pages do competitors have that we don't?)
- Week 11: Monthly report (same)
- Week 12: Re-audit (same)

### For `professional_service`:
Run the standard 12-week schedule but:
- Week 2-3: Run GBP audits only if a GBP listing exists. If not, skip to keyword/content audits.
- Week 7: Run backlink audit and entity optimization. Skip citation-audit unless business serves local area.
- All other weeks: same

### For `content_site`:
- Week 1: Technical audit (same)
- Week 2-6: SKIP all GBP → focus entirely on keyword gap, content gap, and page optimization
- Week 7-8: Backlink audit + search intent (same)
- Week 9-10: Entity optimization + competitor analysis (same)
- Week 11-12: Report + re-audit (same)

### For `ecommerce` or `unknown`:
Run technical audit first, then ask the user which audit areas are most relevant.

**The pipeline status display should show which weeks are SKIPPED vs ACTIVE based on site type:**
```
Week | Phase        | Status       | Notes
-----|-------------|-------------|--------
  2  | Reviews     | ⊘ Skipped   | Not applicable for saas_product
  3  | GBP Final   | ⊘ Skipped   | Not applicable for saas_product
```

### Week 1: Foundation + Technical Baseline

**Skills:**
- `/local-seo-context setup` (if context does not already exist)
- `/local-seo-website technical-audit`
- `/local-seo-gbp category-audit`
- `/local-seo-gbp attributes-audit`

**Focus:** Profile setup, technical health baseline, category gaps, missing attributes

**Expected outcome:** Technical baseline established, GBP categories fixed, missing attributes identified

**Why first:** Technical issues block everything else. Categories and attributes are the fastest ranking wins — changes can appear within days.

**Deliverables:**
- `reports/00-technical-audit.md`
- `reports/01-category-audit.md`
- `reports/02-attributes-audit.md`

---

### Week 2: Reviews + Posts

**Skills:**
- `/local-seo-gbp competitor-reviews`
- `/local-seo-gbp review-strategy`
- `/local-seo-gbp posts-strategy`
- `/local-seo-gbp video-strategy`

**Focus:** Review velocity analysis, response templates, posting + video calendar

**Expected outcome:** Review target set, 8-week posts + video calendar built, response templates ready

**Why now:** Reviews and posts are ongoing — starting early compounds over the full 12 weeks

**Deliverables:**
- `reports/03-competitor-reviews.md`
- `reports/04-review-strategy.md`
- `reports/05-posts-strategy.md`
- `deliverables/review-response-templates.md`
- `deliverables/gbp-posts-calendar.md`
- `deliverables/gbp-video-scripts.md`

---

### Week 3: GBP Complete

**Skills:**
- `/local-seo-gbp services-optimization`
- `/local-seo-gbp description-optimization`
- `/local-seo-gbp photo-audit`
- `/local-seo-gbp direct-booking`

**Focus:** Services descriptions, GBP description A/B versions, photo strategy, direct booking setup

**Expected outcome:** Full GBP optimization complete, booking integration recommended

**Why now:** Completes the GBP foundation before shifting to website

**Deliverables:**
- `reports/06-services-optimization.md`
- `reports/07-description-optimization.md`
- `reports/08-photo-audit.md`
- `deliverables/gbp-description-drafts.md`
- `deliverables/photo-shot-list.md`
- `reports/10-direct-booking.md`

---

### Week 4: Website Keywords

**Skills:**
- `/local-seo-website keyword-gap`
- `/local-seo-website money-page-audit`

**Focus:** Keyword gaps vs competitors, page 2 goldmine identification

**Expected outcome:** Priority keyword list, page optimization sprint plan

**Why now:** Need keyword data before building new pages

**Deliverables:**
- `reports/09-keyword-gap.md`
- `reports/10-money-page-audit.md`
- `deliverables/keyword-map.md`

---

### Week 5: Location Pages

**Skills:**
- `/local-seo-website service-pages`

**Focus:** Generate all missing service x city combination pages

**Expected outcome:** All location pages generated with unique content + schema

**Why dedicated week:** This is the largest single deliverable — may generate 20-80 pages

**Deliverables:**
- `reports/11-service-pages.md`
- `deliverables/service-pages/*`
- `deliverables/schema-markup/*`

**Post-completion:** Suggest running `seo-audit.py` if site is on Cloudflare Pages. Suggest `ping-search-engines.py` for IndexNow after deployment.

---

### Week 6: Deep Analysis

**Skills:**
- `/local-seo-website gsc-analysis`
- `/local-seo-website review-sentiment`

**Focus:** GSC deep dive for page 2 opportunities with internal link silo architecture, customer language mapping

**Expected outcome:** Page 2 goldmine found, internal link silo structure mapped, website copy rewritten in customer voice

**Note:** GSC analysis requires user to export CSV data first. If CSV is missing, prompt the user with these instructions:

> Go to Google Search Console > Performance > Export (top right) > Download CSV.
> Save it as `./data/gsc-export.csv`.

If user cannot provide GSC data, offer to run a WebSearch-based fallback analysis.

**Deliverables:**
- `reports/12-gsc-analysis.md`
- `reports/13-review-sentiment.md`

---

### Week 7: Authority Foundation

**Skills:**
- `/local-seo-authority backlink-audit`
- `/local-seo-authority citation-audit`
- `/local-seo-authority spam-audit`

**Focus:** Competitor backlink analysis, NAP consistency fixes, competitor spam detection

**Expected outcome:** 90-day link building plan, citation fixes queued, competitor spam reported

**Why now:** Authority work takes months to compound — starting mid-plan lets it build

**Deliverables:**
- `reports/14-backlink-audit.md`
- `reports/15-citation-audit.md`
- `deliverables/backlink-targets.md`
- `deliverables/citation-tracker.md`
- `deliverables/outreach-emails.md`
- `deliverables/local-sponsorship-leads.md`
- `deliverables/spam-report-log.md`

---

### Week 8: Intent + Gaps

**Skills:**
- `/local-seo-authority search-intent`
- `/local-seo-content gap-analysis`

**Focus:** Buyer journey keyword mapping, content gap identification

**Expected outcome:** All keywords mapped to funnel stages, 20 content briefs written

**Deliverables:**
- `reports/16-search-intent.md`
- `reports/17-gap-analysis.md`
- `deliverables/intent-map.md`
- `deliverables/content-calendar.md`

---

### Week 9: Entity Building

**Skills:**
- `/local-seo-content entity-optimization`

**Focus:** Schema markup, Knowledge Graph, entity strengthening

**Expected outcome:** Complete JSON-LD markup, entity building action plan

**Why now:** Entity work is the most advanced lever — builds on all prior data

**Deliverables:**
- `reports/18-entity-optimization.md`
- `deliverables/schema-markup/homepage.json`

---

### Week 10: Competitive Intel

**Skills:**
- `/local-seo-content competitor-posts`

**Focus:** Forensic analysis of competitor GBP posting patterns, Information Gain content strategy

**Expected outcome:** Data-driven posting strategy with 4 weeks of ready copy, unique local utility assets planned

**Deliverables:**
- `reports/19-competitor-posts.md`

---

### Week 11: First Report

**Skills:**
- `/local-seo-content monthly-report`

**Focus:** Measure what moved, capture baseline metrics

**Expected outcome:** First monthly report showing wins, problems, and next actions

**Why now:** ~8 weeks of optimization should show measurable results

**Deliverables:**
- `reports/20-monthly-report.md`

---

### Week 12: Re-Audit + Plan

**Skills:**
- `/local-seo-gbp category-audit` (re-run)
- `/local-seo-authority citation-audit` (re-run)
- `/local-seo-content monthly-report` (final)

**Focus:** Measure progress, identify what improved, plan next quarter

**Expected outcome:** Before/after comparison, next 90-day plan

**Deliverables:**
- `reports/pipeline-status.md` (final summary with before/after comparison)

---

## Pipeline Execution Logic

### Subcommand: `start`

1. Verify or create business context (run `/local-seo-context setup` if missing)
2. Initialize pipeline state in the business context JSON file. Add a `pipeline` key:

```json
{
  "pipeline": {
    "started": "YYYY-MM-DD",
    "current_week": 1,
    "site_type": "{business.site_type}",
    "completed_prompts": [],
    "schedule": {
      "week_01": { "status": "in_progress", "started_at": "YYYY-MM-DD", "completed_at": null, "skills_run": [], "notes": "" },
      "week_02": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_03": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_04": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_05": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_06": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_07": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_08": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_09": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_10": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_11": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" },
      "week_12": { "status": "pending", "started_at": null, "completed_at": null, "skills_run": [], "notes": "" }
    }
  }
}
```

3. Run all Week 1 skills sequentially
4. After completion, mark `week_01` status as `"complete"`, set `completed_at`, and advance `current_week` to 2
5. Output the Week 1 pipeline report (see template below)

---

### Subcommand: `resume`

1. Load business context JSON
2. Read `pipeline.current_week` to find where we are
3. Read the current week's `skills_run` array to see which skills already completed
4. Compare against the full skill list for that week — run only the remaining skills
5. If the current week is fully done (all skills in `skills_run`), advance to the next week and start it
6. If all 12 weeks are complete, report that the pipeline is finished and offer to start a new cycle or run monthly reports

---

### Subcommand: `status`

Output a formatted progress table:

```
Super SEO Pipeline Status: [Business Name]
==========================================
Started: YYYY-MM-DD | Current Week: N of 12
Site Type: {site_type} | Routing: {standard/modified}

Week | Phase        | Status      | Completed  | Skills
-----|-------------|-------------|------------|--------
  1  | Foundation  | ? Complete  | YYYY-MM-DD | technical-audit, category-audit, attributes-audit
  2  | Reviews     | ? Complete  | YYYY-MM-DD | competitor-reviews, review-strategy, posts-strategy, video-strategy
  3  | GBP Final   | > Active    | —          | services-optimization, description-optimization, photo-audit, direct-booking
  4  | Keywords    | o Pending   | —          | keyword-gap, money-page-audit
  5  | Pages       | o Pending   | —          | service-pages
  6  | Analysis    | o Pending   | —          | gsc-analysis, review-sentiment
  7  | Authority   | o Pending   | —          | backlink-audit, citation-audit, spam-audit
  8  | Intent      | o Pending   | —          | search-intent, gap-analysis
  9  | Entity      | o Pending   | —          | entity-optimization
 10  | Intel       | o Pending   | —          | competitor-posts
 11  | Report      | o Pending   | —          | monthly-report
 12  | Re-Audit    | o Pending   | —          | category-audit, citation-audit, monthly-report

Progress: X/20 prompts complete (X%)
Reports generated: ./reports/01-*.md through ./reports/XX-*.md
Next action: Run /local-seo-[skill] [sub-command]
```

Use checkmark, arrow, and circle symbols for status indicators.

---

### Subcommand: `week <N>`

1. Parse N from arguments (validate 1-12)
2. Run all skills for week N regardless of pipeline sequence
3. Update that week's status in the pipeline state
4. Do NOT advance `current_week` (since this is an out-of-order run)
5. Output the pipeline report for that week

Useful for:
- Re-running a week after implementing recommendations
- Skipping ahead if some weeks are not relevant
- Running weekly reports (week 11) on an ongoing basis

---

### Subcommand: `reset`

1. Ask the user for explicit confirmation: "This will reset all pipeline progress tracking. Reports and deliverables will NOT be deleted. Type 'yes' to confirm."
2. If confirmed, reset the `pipeline` object in the context JSON:
   - Set `current_week` back to 1
   - Set all week statuses to `"pending"`
   - Clear all `started_at`, `completed_at`, and `skills_run` fields
   - Clear `completed_prompts` array
3. Do NOT delete any files in `./reports/` or `./deliverables/` — they are still useful

---

## Decision Rules

| Situation | Action |
|-----------|--------|
| A skill fails mid-execution | Log the failure in the week's `notes` field, add the skill name to `skills_run` with a `"failed"` suffix, suggest manual fix, continue to the next skill in the week |
| GSC data is needed but missing (weeks 4, 6, 11) | Prompt user with exact export instructions (GSC > Performance > Export CSV), offer to run WebSearch-based fallback analysis instead |
| User has not implemented prior week's recommendations | Note this in the week report but proceed — recommendations compound even if delayed |
| Pipeline is already complete and user runs `start` | Warn that a pipeline already exists, offer `reset` + `start` or continuing with monthly reports |
| Multiple independent skills in one week | Use Agent subagents to run them in parallel where the sub-commands have no data dependencies on each other |
| Each week session length | Expect 1-2 sessions per week to complete all skills |

---

## Pipeline Report Template (Output After Each Week)

After completing each week, output this report and also save it to `./reports/pipeline-week-NN.md`:

```
Week [N] Complete: [Phase Name]
================================

Business: [Business Name]
Week: [N] of 12 — [Phase Name]

Skills Run:
  [checkmark] [skill sub-command]: [brief result summary]
  [checkmark] [skill sub-command]: [brief result summary]
  [X] [skill sub-command]: [failure reason] — [recovery suggestion]

Reports Generated:
  - ./reports/XX-name.md
  - ./deliverables/XX-name.md

Key Findings:
  1. [Most important finding from this week]
  2. [Second most important finding]
  3. [Third finding]

Immediate Actions (do this week):
  1. [Action with HIGH impact — be specific]
  2. [Action with HIGH impact — be specific]

Next Week Preview:
  Week [N+1]: [Phase Name]
  Skills: [comma-separated list]
  Prep needed: [any data exports, access, or actions needed before next week]

Overall Progress: [X]/20 prompts complete ([X]%)
```

---

## Operational Resilience

### Audit Trail Logging

Every skill execution should be noted in the pipeline progress. For detailed logging:

1. **Before each skill runs:** Note the start time and skill name in the week's `notes` field
2. **After each skill completes:** Add completion status and key metrics to `skills_run` array
3. **On failure:** Log the error message, what was attempted, and suggested recovery

### Config Backups

Before updating `~/.claude/config/local-seo/{slug}.json`, the pipeline should:
1. Read the current state
2. If the file has been modified since last read, create a timestamped backup:
   ```bash
   mkdir -p ./backups/config
   cp ~/.claude/config/local-seo/{slug}.json ./backups/config/{slug}-{YYYY-MM-DD-HHMMSS}.json
   ```
3. Then write the updated config

This ensures recovery from corrupt writes or accidental overwrites.

### State Recovery

If the pipeline JSON becomes corrupt:
1. Check `./backups/config/` for the most recent backup
2. Read completed report files in `./reports/` to reconstruct progress state
3. Update the config from the backup + file system evidence

---

## Ethical SEO Guidelines (White-Hat Only)

These rules apply to ALL skills in the pipeline. Violation of these rules undermines long-term ranking stability.

### Review Ethics
- **NO review gating:** Never implement systems that filter negative reviews before they reach GBP. This violates Google TOS and can result in listing suspension.
- **NO fake reviews:** Never suggest buying reviews, trading reviews, or incentivizing reviews with discounts/gifts (violates FTC guidelines).
- **YES review solicitation:** Asking happy customers to leave honest reviews is legitimate and encouraged.

### Link Building Ethics
- **NO PBNs (Private Blog Networks):** Never suggest creating or participating in link networks.
- **NO link buying:** Never suggest purchasing links or participating in link exchange schemes.
- **YES local sponsorships:** Supporting local organizations for genuine community involvement (with a link) is legitimate.
- **YES earned media:** Getting mentioned in local news, industry publications through real value is legitimate.

### Content Ethics
- **NO content spinning:** Never generate near-duplicate content across pages with minor word substitutions.
- **YES unique local content:** Every service+city page must contain genuinely unique information.
- **AI disclosure:** All AI-generated content must be reviewed by a human for local accuracy before publishing. Include E-E-A-T signals (real experience, expertise, authoritativeness, trust).

### Spam Reporting Ethics
- **ONLY report objectively fake profiles:** Keyword-stuffed business names, fake addresses (virtual offices, UPS stores), lead-gen spam listings.
- **NEVER report legitimate competitors:** Having more reviews, better rankings, or more aggressive (but legal) marketing is not spam.
- **Document evidence:** Every spam report must include specific evidence of the policy violation.

---

## Error Recovery Table

| Scenario | Recovery |
|----------|----------|
| No context file found | Run `/local-seo-context setup` as part of week 1 |
| Skill execution fails | Log failure in pipeline notes, suggest fix, continue to next skill in the week |
| GSC CSV data missing | Prompt user with exact export path instructions, offer WebSearch-based fallback |
| Partial week completion | `resume` detects completed skills via `skills_run` array and runs only remaining ones |
| User wants to skip a week | Allow via `week N` — skipped weeks stay `"pending"` in status, note skipped prompts |
| Pipeline already completed (all 12 weeks done) | Offer to start a new 12-week cycle (`reset` + `start`) or run ongoing monthly reports (`week 11`) |
| Context JSON is corrupt or unparseable | Back up the file, recreate it via `/local-seo-context setup`, preserve any salvageable data |
| Network/WebSearch failures | Retry once, then log failure and continue — most skills can partially complete without web data |

---

## Integration Notes

- After week 5 (location pages), suggest running `seo-audit.py` on the deployed site if it is hosted on Cloudflare Pages
- After any week that generates new content for deployment, suggest `ping-search-engines.py` for IndexNow notification
- Week 11+ monthly reports can be scheduled as a recurring task via `/schedule` for ongoing monitoring
- All pipeline state lives in the business context JSON at `~/.claude/config/local-seo/{slug}.json` under the `pipeline` key — this is the single source of truth for progress tracking
