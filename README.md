# Super SEO

SEO automation for Claude Code and Claude Cowork. Point it at any website -- local business, SaaS product, free tool, e-commerce, or content site -- and get a complete SEO optimization tailored to your site type. Auto-detects what kind of site you have and routes to the right audits.

## Installation

### Claude Code CLI (Terminal)

```bash
# Add the marketplace
/plugin marketplace add cisco801/super-seo

# Install the plugin
/plugin install super-seo@cisco801-super-seo
```

**Scope options:**

```bash
/plugin install super-seo@cisco801-super-seo --scope user      # All your projects (default)
/plugin install super-seo@cisco801-super-seo --scope project   # Current project, shared with team
/plugin install super-seo@cisco801-super-seo --scope local     # Current project, gitignored
```

### Claude Code Desktop App (Mac/Windows)

Two options:

**Option A — GUI:** Click the `+` button next to the prompt box > "Add plugin" > add marketplace `cisco801/super-seo` > install `super-seo`

**Option B — Terminal:** Same commands as CLI above — type them in the Desktop app's terminal.

### Claude Code Web App (claude.ai/code)

Same commands as CLI — type them in the web terminal.

### Claude Cowork

Cowork uses a separate plugin system. Custom GitHub plugins require organization admin setup:

**For organization admins:**
1. Fork or mirror this repo as a **private** repo (Cowork requires private repos)
2. Go to Organization Settings > Plugins > "Add from GitHub"
3. Enter the private repo path
4. Enable automatic syncing

**For individual Cowork users:** Custom plugins from GitHub are not available without org admin setup. Ask your admin to add the repo, or use Claude Code CLI/Desktop/Web instead.

### Quick Test (No Install)

```bash
# Clone and run directly (session-only, no permanent install):
git clone https://github.com/cisco801/super-seo.git
claude --plugin-dir ./super-seo/plugin
```

## Quick Start

### 2. Point at Any Website

```
/seo https://acmeplumbing.com
```

The `/seo` command auto-discovers business details from the website:
- Business name, address, phone
- Google Business Profile URL
- Services and service areas
- Schema.org type (Plumber, Dentist, etc.)

Then asks for what it can't auto-detect (keywords, competitors, biggest SEO problem).

### 3. Quick Audit or Full Pipeline

```
/seo --audit https://acmeplumbing.com   # Quick audit tailored to your site type
/seo --health https://acmeplumbing.com  # Fast 5-point health check
/seo --full https://acmeplumbing.com    # Start full 12-week pipeline
```

Or run individual audits directly:

```
/local-seo-website technical-audit     # Core Web Vitals, mobile, page speed
/local-seo-gbp category-audit         # GBP category gaps vs competitors
/local-seo-gbp video-strategy         # Short-form video plan
/local-seo-authority citation-audit    # NAP consistency across 15+ directories
/local-seo-authority spam-audit        # Detect fake competitor listings
/local-seo-content entity-optimization # Schema + Knowledge Graph building
```

## Site Type Intelligence

The plugin auto-detects your site type and adapts:

| Site Type | How Detected | Audits Run | Audits Skipped |
|-----------|-------------|-----------|----------------|
| **Local Service** | GBP listing, LocalBusiness schema, local phone + address | All 24 audit types | None |
| **SaaS Product** | SoftwareApplication schema, pricing + features pages | Technical, keywords, content, entity, backlinks | GBP, citations, reviews, map pack |
| **Free Tool** | WebApplication with price "0", single-page tool | Technical, per-variant pages, content gaps | GBP, citations, reviews |
| **Professional Service** | ProfessionalService schema, consulting/agency | Technical, GBP (if exists), entity, content | Citations (optional) |
| **E-commerce** | Product schema, shopping cart | Technical, keywords, product schema | GBP, local citations |
| **Content Site** | Primarily blog/articles | Technical, keywords, content gaps, backlinks | GBP, citations, reviews |

Every site gets a **Universal Quick Health Check** first:

```
Quick Health Check: Abundera QR
================================
Indexation:    0 pages indexed / 1 in sitemap  -- CRITICAL
Meta Desc:     MISSING                          -- CRITICAL
Sitemap:       1 URL (should be 20+)            -- CRITICAL
Canonical:     MISSING                          -- HIGH
Schema:        WebApplication + FAQPage         -- OK

Overall: 1 of 5 checks passed
```

### 4. Check Progress Anytime

```
/local-seo-pipeline status    # Week-by-week progress table
/local-seo-pipeline resume    # Pick up where you left off (even across sessions)
```

## What It Does

### 6 Skills, 24 Audit Types, 12 Weeks

| Skill | Sub-commands | What It Produces |
|-------|-------------|-----------------|
| **local-seo-context** | setup, switch, update, status | Business profile JSON, progress tracker |
| **local-seo-gbp** | category-audit, attributes-audit, competitor-reviews, review-strategy, posts-strategy, services-optimization, description-optimization, photo-audit, video-strategy, direct-booking | GBP optimization reports, review templates, posts calendar, video scripts |
| **local-seo-website** | technical-audit, keyword-gap, money-page-audit, service-pages, gsc-analysis, review-sentiment | Technical health grade, keyword maps, location pages with schema, 30-day sprints |
| **local-seo-authority** | backlink-audit, citation-audit, search-intent, spam-audit | Link targets, outreach emails, citation tracker, spam report log |
| **local-seo-content** | gap-analysis, entity-optimization, competitor-posts, monthly-report | Content calendar, JSON-LD schema, monthly performance reports |
| **local-seo-pipeline** | start, resume, status, week N, reset | 12-week orchestration with progress tracking |

### Output Structure

Every audit produces actionable deliverables in your project directory:

```
reports/                          # Analysis reports (one per audit)
  00-technical-audit.md
  01-category-audit.md
  ...
  20-monthly-report.md

deliverables/                     # Ready-to-use assets
  review-response-templates.md    # 12 templates by star rating
  gbp-posts-calendar.md          # 8-week posting calendar
  gbp-video-scripts.md           # Video scripts + production guide
  gbp-description-drafts.md      # 3 A/B test versions
  photo-shot-list.md             # 8-week photo plan with geotagging
  keyword-map.md                  # Prioritized keyword opportunities
  backlink-targets.md             # 90-day link acquisition plan
  outreach-emails.md              # 15 ready-to-send emails
  local-sponsorship-leads.md      # Community engagement targets
  citation-tracker.md             # NAP consistency matrix
  spam-report-log.md              # Fake competitor tracking
  intent-map.md                   # Keywords by buyer journey stage
  content-calendar.md             # 20 content briefs
  service-pages/*.html            # Location pages with unique content
  schema-markup/*.json            # JSON-LD per page
```

## 12-Week Execution Plan

| Week | Phase | Focus |
|------|-------|-------|
| 1 | Foundation | Technical audit + GBP categories/attributes |
| 2 | GBP | Reviews, response templates, video + posts strategy |
| 3 | GBP Complete | Services, description, photos, direct booking |
| 4 | Website | Keyword gap analysis + money page audit |
| 5 | Location Pages | Service x city page generation with unique local utility |
| 6 | Deep Analysis | GSC analysis with link silos + review sentiment |
| 7 | Authority | Backlinks, citations, competitor spam detection |
| 8 | Intent + Content | Search intent mapping + content gap analysis |
| 9 | Entity | Schema markup + Knowledge Graph optimization |
| 10 | Competitive Intel | Competitor post forensics + Information Gain strategy |
| 11 | First Report | Monthly performance report with baseline metrics |
| 12 | Re-Audit | Measure progress, plan next quarter |

## 2026 Enhancements

Built on the latest algorithm research (March 2026 core update):

- **AI Overviews / GEO** -- structured content for the 48% of queries showing AI summaries
- **GBP completeness scoring** -- now an explicit ranking input
- **Review recency weighting** -- velocity matters more than count
- **Video & Social Proof** -- GBP video updates are primary ranking signals
- **Anti-cookie-cutter** -- genuinely unique city pages with Information Gain content
- **Voice search readiness** -- FAQ schema + conversational content (50%+ local via voice)
- **Citation freshness** -- quarterly update cycle as ranking signal
- **Competitor spam fighting** -- identify and report fake GBP listings
- **Direct booking integration** -- Reserve with Google recommendations
- **Specific schema subtypes** -- `Plumber`, `Dentist`, etc. not generic `LocalBusiness`

## Multi-Client Support

Manage multiple businesses from one installation:

```
/local-seo-context setup                  # Set up first client
/local-seo-context setup                  # Set up second client
/local-seo-context switch acme-plumbing   # Switch between clients
/local-seo-context status                 # View progress for active client
```

Each client gets its own config at `~/.claude/config/local-seo/{slug}.json`.

## No Paid Tools Required

The original system assumed SEMrush, Ahrefs, and browser automation. This system uses:

- **WebSearch + WebFetch** for all competitor research and GBP analysis
- **GSC CSV exports** for Search Console data (optional -- falls back to WebSearch)
- **No paid tool subscriptions** needed

## Ethical SEO Only

White-hat practices enforced at the system level:

- No review gating or fake reviews
- No PBNs or link buying
- No content spinning -- every page gets genuinely unique content
- Spam reporting limited to objectively fake profiles with documented evidence
- All AI-generated content reviewed for E-E-A-T compliance

## Plugin Structure

```
plugin/
  .claude-plugin/
    plugin.json                   # Plugin metadata (name, version, keywords)
  commands/
    seo.md                        # /seo quick-start command
  skills/
    local-seo-context/ -> SKILL.md   # Business profile loader
    local-seo-gbp/ -> SKILL.md      # GBP optimization (10 audits)
    local-seo-website/ -> SKILL.md   # Website SEO (6 sub-commands)
    local-seo-authority/ -> SKILL.md # Authority building (4 sub-commands)
    local-seo-content/ -> SKILL.md   # Content strategy (4 sub-commands)
    local-seo-pipeline/ -> SKILL.md  # 12-week orchestrator
  README.md                       # Plugin installation docs
```

## License

MIT
