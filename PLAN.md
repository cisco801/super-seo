# Super SEO: Local SEO Automation System

## Overview

A 6-skill Claude Code system that automates local SEO auditing, optimization, and strategy for service-area businesses. Based on a proven 20-prompt framework refined over 14 years of local SEO management, enhanced with 2026 algorithm updates and AI Overviews optimization.

**Skills:**

| Skill | Prompts | Purpose |
|-------|---------|---------|
| `local-seo-context` | — | Business profile loader + progress tracker |
| `local-seo-gbp` | 1-8 | GBP optimization + Video/Social strategy |
| `local-seo-website` | 9-13 | Technical health + Website SEO + page generation |
| `local-seo-authority` | 14-16 | Backlinks, citations, spam fighting, search intent |
| `local-seo-content` | 17-20 | Content strategy, entity, reporting |
| `local-seo-pipeline` | All | 12-week master orchestrator |

## Architecture

### Data Flow

```
local-seo-context
    │
    ├── writes: ~/.claude/config/local-seo/{slug}.json
    │           ./local-seo-context.json (active pointer)
    │
    ▼
local-seo-gbp  ◄── reads context JSON
    │
    ├── writes: ./reports/01-08-*.md
    │           ./deliverables/review-templates.md, posts-calendar.md, etc.
    ├── updates: progress in context JSON
    │
    ▼
local-seo-website  ◄── reads context JSON + GBP reports
    │
    ├── reads: ./data/gsc-exports/*.csv (user-provided)
    ├── writes: ./reports/09-13-*.md
    │           ./deliverables/service-pages/*.html
    │           ./deliverables/schema-markup/*.json
    ├── integrates: seo-audit.py, ping-search-engines.py
    ├── updates: progress in context JSON
    │
    ▼
local-seo-authority  ◄── reads context JSON + keyword data
    │
    ├── writes: ./reports/14-16-*.md
    │           ./deliverables/backlink-targets.md, citation-tracker.md
    ├── updates: progress in context JSON
    │
    ▼
local-seo-content  ◄── reads ALL prior reports + context JSON
    │
    ├── writes: ./reports/17-20-*.md
    │           ./deliverables/content-calendar.md, schema-templates/
    ├── updates: progress in context JSON
    │
    ▼
local-seo-pipeline  ◄── reads/writes context JSON progress
    │                    orchestrates all above in 12-week sequence
    └── writes: ./reports/pipeline-status.md
```

### Business Context Storage

Each client gets a JSON config at `~/.claude/config/local-seo/{slug}.json`:

```json
{
  "business": {
    "name": "Acme Plumbing",
    "slug": "acme-plumbing",
    "address": "123 Main St, Las Vegas, NV 89102",
    "phone": "(702) 555-1234",
    "website": "https://acmeplumbing.com",
    "gbp_url": "https://maps.google.com/...",
    "years_in_business": 12,
    "team_size": "small team",
    "services": {
      "primary": "Residential Plumbing",
      "secondary": ["Drain Cleaning", "Water Heater Repair", "Pipe Repair", "Sewer Line"]
    },
    "service_areas": ["Las Vegas", "Henderson", "North Las Vegas", "Summerlin", "Spring Valley"],
    "target_customer": "Homeowners with plumbing emergencies or maintenance needs",
    "average_job_value": 450,
    "schema_type": "Plumber"
  },
  "seo_goals": {
    "target_keywords": ["plumber las vegas", "emergency plumber near me", "drain cleaning las vegas"],
    "currently_ranking": ["las vegas plumber", "plumbing repair 89102"],
    "should_rank_for": ["24 hour plumber henderson", "water heater installation las vegas"],
    "biggest_problem": "Not showing in map pack for emergency searches"
  },
  "current_standings": {
    "google_reviews": { "total": 87, "rating": 4.6, "monthly_new": 5 },
    "gbp_monthly_views": 2400,
    "monthly_website_traffic": 800,
    "map_pack_status": "Ranking for 'plumber 89102', not ranking for 'emergency plumber las vegas'"
  },
  "competitors": [
    {
      "name": "Bob's Plumbing",
      "gbp_url": "https://maps.google.com/...",
      "website": "https://bobsplumbing.com",
      "why_beating": "200+ reviews, posting on GBP weekly"
    }
  ],
  "previous_seo_work": "Had an agency for 6 months, they did basic citations. No ongoing work.",
  "data_sources": {
    "gsc_export_dir": "./data/gsc-exports/",
    "project_dir": null
  },
  "progress": {},
  "pipeline": {
    "started": null,
    "current_week": 0,
    "schedule": {}
  }
}
```

The active project pointer lives at `./local-seo-context.json`:

```json
{
  "active_business": "acme-plumbing",
  "config_path": "~/.claude/config/local-seo/acme-plumbing.json"
}
```

### Directory Layout (Per Client Project)

```
{project-dir}/
├── local-seo-context.json          # Active business pointer
├── data/
│   └── gsc-exports/                # User drops GSC CSV exports here
├── reports/
│   ├── 01-category-audit.md
│   ├── 02-attributes-audit.md
│   ├── 03-competitor-reviews.md
│   ├── 04-review-strategy.md
│   ├── 05-posts-strategy.md
│   ├── 06-services-optimization.md
│   ├── 07-description-optimization.md
│   ├── 08-photo-audit.md
│   ├── 09-keyword-gap.md
│   ├── 10-money-page-audit.md
│   ├── 11-service-city-pages.md
│   ├── 12-gsc-analysis.md
│   ├── 13-review-sentiment.md
│   ├── 14-backlink-audit.md
│   ├── 15-citation-audit.md
│   ├── 16-search-intent-map.md
│   ├── 17-content-gap.md
│   ├── 18-entity-optimization.md
│   ├── 19-competitor-posts.md
│   ├── 20-monthly-report.md
│   └── pipeline-status.md
└── deliverables/
    ├── review-response-templates.md
    ├── gbp-posts-calendar.md
    ├── gbp-video-scripts.md         # Scripts for short-form video updates
    ├── gbp-description-drafts.md
    ├── photo-shot-list.md
    ├── keyword-map.md
    ├── backlink-targets.md
    ├── local-sponsorship-leads.md  # Community engagement targets
    ├── citation-tracker.md
    ├── spam-report-log.md           # Tracking reported fake competitors
    ├── intent-map.md
    ├── content-calendar.md
    ├── outreach-emails.md
    ├── service-pages/
    │   ├── plumber-las-vegas.html
    │   └── drain-cleaning-henderson.html
    └── schema-markup/
        ├── homepage.json
        └── service-plumber-las-vegas.json
```

## 2026 Enhancements (Beyond Original 20 Prompts)

These are integrated into the relevant skills based on March 2026 algorithm updates and industry research:

| Enhancement | Integrated Into | Impact |
|-------------|----------------|--------|
| **AI Overviews / GEO optimization** | website, content | Structured, fact-dense content with schema for AI citation (48% of queries now show AI Overviews) |
| **GBP completeness scoring** | gbp | Google now explicitly uses profile completeness as ranking input |
| **Citation freshness monitoring** | authority | Google tracks when citations are updated; quarterly updates show measurable benefit |
| **Review recency + response rate** | gbp | March 2026 core update weights these more heavily than raw review counts |
| **Video & Social Proof** | gbp | GBP short-form video updates (behind-the-scenes, video testimonials) are now primary ranking signals |
| **Information Gain / Local Utility** | website, content | Unique local data (permit guides, local pricing calculators) prevents "cookie-cutter" penalties |
| **Zero-Party Data / Direct Booking** | gbp, pipeline | Integration with "Reserve with Google" and direct messaging is a high-weight ranking factor |
| **Voice search readiness** | website, content | 50%+ local searches via voice; FAQ schema + conversational content |
| **Cookie-cutter page detection** | website | Google filtering templated location pages more aggressively |
| **Specific LocalBusiness schema subtypes** | content | Use `Plumber`, `Dentist`, etc. — never generic `LocalBusiness` |
| **AI content E-E-A-T signals** | website, content | Local expertise markers required to avoid AI content penalties |
| **GBP Vision AI** | gbp | Google's AI categorizes services from uploaded photos — image quality now ranking factor |
| **Local pack ad displacement** | gbp, pipeline | Ads increased 733% in local packs — organic optimization more critical |

## 12-Week Execution Plan

| Week | Phase | Skills/Prompts | Focus | Expected Outcome |
|------|-------|---------------|-------|-----------------|
| 1 | Foundation | Context + Website | Technical audit, Mobile/CWV check, Category audit | Technical baseline established, GBP categories fixed |
| 2 | GBP | GBP 3-5 | Reviews analysis, response strategy, Video strategy | Review velocity target set, Video/Post calendar built |
| 3 | GBP | GBP 6-8 | Services, description, photos, Direct Booking | Full GBP optimization complete, booking integrated |
| 4 | Website | Website 9-10 | Keyword gap + money page audit | Priority keyword list, page optimization sprint plan |
| 5 | Website | Website 11 | Service + city page generation (Unique local utility) | High-utility location pages generated |
| 6 | Website | Website 12-13 | GSC deep dive + internal link silos | Page 2 goldmine found, internal link structure fixed |
| 7 | Authority | Authority 14-15 | Backlinks, citations, Spam reporting | Citation fixes queued, competitor spam reported |
| 8 | Authority + Content | Authority 16 + Content 17 | Search intent + content gaps | Buyer journey mapped, content briefs written |
| 9 | Content | Content 18 | Entity / schema optimization (Specific subtypes) | JSON-LD markup ready, knowledge graph signals built |
| 10 | Content | Content 19 | Competitor posts + Information Gain strategy | Posting strategy built, unique local assets planned |
| 11 | Reporting | Content 20 | First monthly report | Baseline metrics captured |
| 12 | Re-audit | All key audits | Re-run categories, citations, GSC | Progress measured, next quarter planned |

## Operational Resilience & Ethics

### Logging & Backups
- **Audit Trail:** All skill executions log to `./logs/{skill}-{date}.log`.
- **Config Backups:** Every update to `~/.claude/config/local-seo/{slug}.json` triggers a timestamped backup in `./backups/config/`.
- **State Recovery:** The `local-seo-pipeline` can resume from the last successful week by reading the `progress` object in the context JSON.

### Ethical (White-Hat) SEO
- **No Review Gating:** System will not implement "review gating" (filtering negative reviews before they hit GBP), which violates Google TOS.
- **Natural Link Building:** Focus on local relevance and real-world relationships, avoiding PBNs or link farms.
- **AI Disclosure:** All AI-generated content is reviewed for E-E-A-T signals and human-in-the-loop validation to ensure local accuracy.
- **Spam Reporting:** Only report objectively fake profiles (e.g., keyword-stuffed names, residential addresses used as storefronts) to maintain ecosystem integrity.

## Tool Adaptations

The original system assumes browser automation ("Open Chrome and go to..."). Our Claude Code adaptation uses:

| Original Approach | Our Approach |
|-------------------|-------------|
| Open Chrome, navigate to GBP | WebSearch + WebFetch GBP URLs |
| Log into SEMrush/Ahrefs | WebSearch for keyword/backlink research (no paid tool dependency) |
| Log into GSC | User exports CSV, skill reads from `./data/gsc-exports/` |
| Manual spreadsheet creation | Markdown tables + structured reports |
| Copy-paste to GBP | Ready-to-paste deliverables in `./deliverables/` |

## Integration with Existing Scripts

| Script | Used By | Purpose |
|--------|---------|---------|
| `seo-audit.py` | local-seo-website | Validate generated pages have robots.txt, sitemap, headers |
| `ping-search-engines.py` | local-seo-website | IndexNow notifications after page deployment |
| `cloudflare.py` | local-seo-website | Deploy to CF Pages if applicable |
| `cf-sites.json` | local-seo-context | Check if business website is a known CF Pages site |

## Plugin Architecture

The system is packaged as a Claude Code plugin at `./plugin/`:

```
plugin/
  .claude-plugin/
    plugin.json               # Plugin metadata (name, version, description, keywords)
  commands/
    seo.md                    # /seo quick-start command -- point at any website
  skills/
    local-seo-context/        # -> symlink to ~/projects/claude-code/skills/
    local-seo-gbp/
    local-seo-website/
    local-seo-authority/
    local-seo-content/
    local-seo-pipeline/
  README.md                   # Plugin-specific docs
```

**Installation:**
```bash
# Development/testing (session-only):
claude --plugin-dir ./plugin

# Install for current project:
claude plugin install local-seo@super-seo --scope project

# Install for all projects:
claude plugin install local-seo@super-seo --scope user
```

**Quick start command:**
```
/seo https://any-business-website.com          # Auto-discover and setup
/seo --audit https://any-business-website.com  # Quick 3-part audit
/seo --full https://any-business-website.com   # Start full 12-week pipeline
```

The `/seo` command auto-discovers business details from the website (name, address, phone, services, areas, GBP URL, schema type) via WebSearch + WebFetch, then offers quick audit or full pipeline.

## Source

Based on Sarvesh Shrivastava's "20 SEO Prompts" system (Alventra Marketing), enhanced with:
- 2026 Google algorithm updates (March 2026 core update)
- AI Overviews / Generative Engine Optimization research
- GBP completeness scoring as explicit ranking input
- Voice search optimization (50%+ local searches)
- Citation freshness as ranking signal
- Cookie-cutter page detection avoidance
