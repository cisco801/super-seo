---
description: Quick-start SEO optimization for any website — auto-detects site type and routes to relevant audits
argument-hint: "[website URL or business name] [--audit] [--full] [--health]"
---

# /seo -- SEO Quick Start

Point at any website and start optimizing. Auto-detects site type and routes to the right audits.

## Invocation

```
/seo https://acmeplumbing.com
/seo "Acme Plumbing Henderson NV"
/seo --audit https://acmeplumbing.com
/seo --full https://acmeplumbing.com
/seo --health https://acmeplumbing.com
```

## Argument Parsing

Parse `$ARGUMENTS` for:
- **URL** = a website URL (starts with http or contains `.com/.net/.org/etc.`) -- target website to optimize
- **Quoted text** = business name + location if no URL given
- **`--audit`** = run a quick technical + GBP audit only (no full pipeline)
- **`--full`** = start the full 12-week pipeline immediately after setup
- **`--health`** = run universal quick health check only (indexation, meta, sitemap, schema)

If no arguments, ask what business/website to optimize.

## Workflow

### Step 1: Check for Existing Context

1. Check if `./local-seo-context.json` exists in the current directory
2. If it exists, read the active business context and ask: "Found existing context for {business name}. Continue with this business, or set up a new one?"
3. If user wants to continue, skip to Step 3

### Step 2: Auto-Setup Business Context

If no context exists or user wants a new business:

1. **WebSearch** the provided URL or business name to discover:
   - Business name, address, phone number
   - Google Business Profile URL
   - Primary service category
   - Service areas mentioned on the website
2. **WebFetch** the website homepage to extract:
   - Title tag (reveals primary keyword targeting)
   - Services listed
   - Service areas/cities mentioned
   - Phone number and address from footer/contact page
   - Existing schema markup
3. **Detect Site Type** — after all auto-discovery is complete, classify the site:

   Site Type Detection Rules:
   - **LOCAL_SERVICE**: Has GBP listing, OR schema @type is a LocalBusiness subtype (Plumber, Dentist, Attorney, Restaurant, etc.), OR has physical address + service areas + local phone
   - **SAAS_PRODUCT**: Schema @type is SoftwareApplication or WebApplication, OR has pricing page + features page + API docs, OR has "sign up" / "free trial" / "pricing" CTAs
   - **FREE_TOOL**: Schema @type is WebApplication with price "0", OR is a single-page tool/generator/calculator, OR has no pricing (completely free)
   - **ECOMMERCE**: Has product pages, shopping cart, product schema
   - **CONTENT_SITE**: Primarily blog/articles, no products or services for sale
   - **PROFESSIONAL_SERVICE**: Schema @type is ProfessionalService, consulting, agency, freelancer
   - **UNKNOWN**: Can't determine — ask user

4. **Present findings** to the user:
   ```
   Found: Acme Plumbing
   Address: 123 Main St, Henderson, NV 89011
   Phone: (702) 555-0123
   Website: https://acmeplumbing.com
   GBP: https://google.com/maps/place/...
   Services detected: Residential Plumbing, Water Heater Repair, Drain Cleaning
   Areas detected: Henderson, Las Vegas, North Las Vegas
   Schema type: Plumber
   Site Type: Local Service (detected from Plumber schema + GBP listing + service areas)

   Is this correct? What should I adjust?
   ```
5. **Ask for missing data** that couldn't be auto-detected:
   - Target keywords (top 3-5)
   - Competitors (top 1-3)
   - Biggest SEO problem
   - Review count and rating (if not found)
6. **Save context** using the local-seo-context setup flow
   - Write config to `~/.claude/config/local-seo/{slug}.json` (include `siteType` field)
   - Write pointer to `./local-seo-context.json`
   - Create `./reports/`, `./deliverables/`, `./data/gsc-exports/` directories

### Step 3: Universal Quick Health Check (runs ALWAYS, every mode)

Before any deep audit, run these 5 quick checks. These apply to ALL site types:

1. **Indexation check:** WebSearch `site:{website}` — count results
   - 0 indexed: CRITICAL — "Your site is invisible to Google. Nothing else matters until this is fixed."
   - 1-10 indexed but sitemap has 50+: WARNING — "Major indexation gap"
   - Healthy: note count

2. **Meta description check:** WebFetch homepage — is there a `<meta name="description">` tag?
   - Missing: CRITICAL — "No meta description. Google has nothing to show in search results."

3. **Sitemap check:** WebFetch `/sitemap.xml` — does it exist? How many URLs?
   - Missing: CRITICAL
   - Exists but only 1 URL when site has multiple pages: WARNING
   - Compare sitemap count vs site: search count

4. **Canonical tag check:** Is `<link rel="canonical">` present on homepage?
   - Missing: HIGH — risk of duplicate content issues

5. **Schema check:** Is there any JSON-LD structured data?
   - Missing: MEDIUM — missing rich result opportunities
   - Present: note @type

Output the health check as a quick scorecard:
```
Quick Health Check: {site name}
================================
Indexation:    [X pages indexed / Y in sitemap] — {CRITICAL/WARNING/OK}
Meta Desc:     {MISSING/PRESENT} — {CRITICAL/OK}
Sitemap:       {MISSING/X URLs} — {CRITICAL/WARNING/OK}
Canonical:     {MISSING/PRESENT} — {HIGH/OK}
Schema:        {MISSING/type found} — {MEDIUM/OK}

Overall: {X of 5 checks passed}
```

If ANY check is CRITICAL, flag it prominently and recommend fixing before deep audits.

### Step 4: Choose Audit Mode

Based on flags AND site type:

**If `--health` (health check only):**
Output the health check from Step 3 and stop.

**If `--audit` (quick audit mode):**

Route based on detected site type:

**For LOCAL_SERVICE:**
1. `/local-seo-website technical-audit`
2. `/local-seo-gbp category-audit`
3. `/local-seo-authority citation-audit`

**For SAAS_PRODUCT:**
1. `/local-seo-website technical-audit`
2. `/local-seo-website keyword-gap` (competitor keyword analysis)
3. `/local-seo-content entity-optimization` (schema + directory submissions)
+ Recommend: comparison pages, Product Hunt launch, G2/Capterra listings

**For FREE_TOOL:**
1. `/local-seo-website technical-audit`
2. `/local-seo-website keyword-gap` (per-variant landing page opportunities)
3. `/local-seo-content gap-analysis` (missing landing pages per feature)
+ Recommend: one page per tool variant, listicle outreach, tool directories

**For PROFESSIONAL_SERVICE:**
1. `/local-seo-website technical-audit`
2. `/local-seo-gbp category-audit` (if GBP exists)
3. `/local-seo-content entity-optimization`
+ Recommend: case studies, comparison content, authority building

**For CONTENT_SITE:**
1. `/local-seo-website technical-audit`
2. `/local-seo-website keyword-gap`
3. `/local-seo-content gap-analysis`

**For ECOMMERCE or UNKNOWN:**
1. `/local-seo-website technical-audit`
2. `/local-seo-website keyword-gap`
+ Ask user which additional audits are relevant

After audit completes, output combined summary with site-type-specific recommendations.

**If `--full` (full pipeline mode):**
Run `/local-seo-pipeline start` — the pipeline itself will check site type and skip irrelevant weeks.

**If no flags (default):**
Show health check results + available options tailored to site type:
```
Site Type: {detected type}

Recommended audits for {type}:
  /seo --audit              Quick 3-part audit tailored to your site type
  /seo --health             Health check only (indexation, meta, sitemap)
  /local-seo-pipeline start Full optimization pipeline

{type-specific recommendations}
```

### Step 5: Next Steps (Site-Type-Aware)

After any mode completes, tailor next steps to the detected site type:

**For LOCAL_SERVICE:**
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Run `/local-seo-gbp category-audit` to optimize your Google Business Profile
- Run `/local-seo-authority citation-audit` to fix NAP inconsistencies
- Run `/local-seo-pipeline start` for the full 12-week local SEO plan
- Run `/local-seo-pipeline status` to see overall progress

**For SAAS_PRODUCT:**
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Run `/local-seo-website keyword-gap` to find competitor keyword opportunities
- Run `/local-seo-content entity-optimization` for schema and directory submissions
- Create comparison pages (vs competitors) for bottom-of-funnel traffic
- Submit to Product Hunt, G2, Capterra, and relevant SaaS directories
- Run `/local-seo-pipeline start` for structured optimization (pipeline will skip irrelevant local-only weeks)

**For FREE_TOOL:**
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Run `/local-seo-website keyword-gap` to find landing page opportunities per tool variant
- Run `/local-seo-content gap-analysis` for missing feature landing pages
- Create one page per tool variant/use-case for long-tail traffic
- Submit to tool directories and pitch for listicle inclusion
- Run `/local-seo-pipeline start` for structured optimization

**For PROFESSIONAL_SERVICE:**
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Run `/local-seo-content entity-optimization` for Knowledge Graph and authority building
- Create case study pages to demonstrate expertise
- Run `/local-seo-gbp category-audit` if you have a Google Business Profile
- Run `/local-seo-pipeline start` for structured optimization

**For CONTENT_SITE, ECOMMERCE, or UNKNOWN:**
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Run `/local-seo-website keyword-gap` to find content opportunities
- Run `/local-seo-content gap-analysis` for missing topic coverage
- Run `/local-seo-pipeline status` to see overall progress
- Run `/local-seo-pipeline resume` to continue where you left off
