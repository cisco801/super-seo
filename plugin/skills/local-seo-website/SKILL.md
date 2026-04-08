---
name: local-seo-website
description: |
  Website SEO optimization for local businesses — prompts 9-13 plus technical health audit.
  Covers technical audit (CWV, mobile, speed), keyword gap analysis, money page audits,
  service+city page generation with unique local utility, GSC deep analysis with internal
  link silo mapping, and review sentiment reverse-engineering.
user-invocable: true
argument-hint: "[technical-audit|keyword-gap|money-page-audit|service-pages|gsc-analysis|review-sentiment]"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO Website Optimization (Prompts 9-13)

**Command:** $ARGUMENTS

## Argument Parsing

Parse `$ARGUMENTS` for the subcommand:

- **`technical-audit`** = Technical health baseline: Core Web Vitals, mobile-friendliness, page speed, crawlability
- **`keyword-gap`** = Prompt 9: Find keywords competitors rank for that we don't
- **`money-page-audit`** = Prompt 10: Find page 2 rankings that need one push to page 1
- **`service-pages`** = Prompt 11: Generate location-specific service x city pages
- **`gsc-analysis`** = Prompt 12: Deep Google Search Console analysis
- **`review-sentiment`** = Prompt 13: Reverse-engineer customer emotional language for copy
- **(empty / no args)** = Show available subcommands and current progress for prompts 9-13

If $ARGUMENTS is empty, display a summary of available subcommands with their status from context.

---

## Prerequisite (Every Run - MANDATORY)

Before executing ANY subcommand:

### Step 1: Load Active Business

1. Read `./local-seo-context.json` to get active business slug
2. Read `~/.claude/config/local-seo/{slug}.json` for full business context
3. Extract and hold in working memory:
   - `business.name`, `business.website`, `business.phone`, `business.address`
   - `business.services.primary`, `business.services.secondary`
   - `business.service_areas` (list of cities)
   - `business.schema_type`
   - `seo_goals.target_keywords`
   - `competitors[]` (names, websites, GBP URLs)
   - `current_standings`
   - `data_sources.gsc_export_dir`

**If `./local-seo-context.json` does not exist:** Stop and tell the user to run `/local-seo-context setup` first. Do not proceed.

**If config JSON at the path does not exist:** Stop and tell the user to run `/local-seo-context setup` or `/local-seo-context switch <slug>` first.

**Success criteria:** All critical business fields loaded into working memory.

### Step 2: Ensure Output Directories

```bash
mkdir -p ./reports ./deliverables/service-pages ./deliverables/schema-markup ./data/gsc-exports
```

### Step 3: Quick Health Check (Runs Before Every Subcommand)

Before executing any subcommand, run these 3 fast checks and note results in the report header:

1. **Indexation pulse:** WebSearch `site:{business.website}` — count approximate results
   - Store as `indexed_count` for use in reports
   - If 0: prepend WARNING to any report: "This site has ZERO Google indexation. Fix this before optimizing content."

2. **Meta description pulse:** WebFetch homepage, check for `<meta name="description">`
   - Store as `has_meta_description` (true/false)
   - If false: note in report header

3. **Sitemap pulse:** WebFetch `{business.website}/sitemap.xml`
   - Store as `sitemap_url_count`
   - If missing or 0: note in report header
   - If exists: compare to `indexed_count` — flag if gap > 50%

These are fast, lightweight checks that add < 30 seconds. They ensure every report includes the site's health baseline.

**Do NOT stop execution** based on these results — just note them prominently in the report. The user may be running audits specifically to diagnose these issues.

---

## Subcommand 0: technical-audit (2026 Enhancement — Run First)

**Goal:** Establish a technical health baseline before any content optimization. Core Web Vitals, mobile-friendliness, and page speed are prerequisites — no amount of content optimization helps if the site is technically broken.

### Phase 0: Indexation & Discoverability (Check FIRST)

This is the single most important check. If Google hasn't indexed the site, nothing else matters.

1. **WebSearch** `site:{website}` — count results
   - **0 results:** CRITICAL — site is invisible to Google
     - Check: Was the site recently launched? (check whois/registration date via WebSearch)
     - Check: Is there a `noindex` meta tag on the homepage?
     - Check: Does robots.txt block Googlebot?
     - Check: Has the sitemap been submitted to Google Search Console?
   - **1-10 results but sitemap has 50+:** WARNING — major indexation gap
     - Possible causes: thin content, duplicate content, no internal linking, crawl budget
   - **Healthy indexation:** Note count, move on

2. **Sitemap completeness:**
   - WebFetch `{website}/sitemap.xml` — count URLs
   - Compare sitemap count to indexed count
   - If sitemap is missing: CRITICAL
   - If sitemap has only 1 URL but site clearly has more pages: CRITICAL
   - If gap between sitemap and indexed is > 50%: WARNING

3. **Meta description coverage:**
   - WebFetch homepage — check for `<meta name="description">`
   - WebFetch 2-3 inner pages — check for meta descriptions
   - If no pages have meta descriptions: CRITICAL
   - If some do, some don't: WARNING with list

4. **Canonical tag coverage:**
   - Check homepage for `<link rel="canonical">`
   - If missing: HIGH — risk of duplicate content

5. **Open Graph tag coverage:**
   - Check for og:title, og:description, og:image, og:url
   - If all missing: MEDIUM — hurts social sharing

**Output this section FIRST in the report, before CWV or anything else.** If indexation is at 0, make it the #1 action item regardless of everything else.

**Success criteria:** Indexation status known, sitemap assessed, meta/canonical/OG coverage checked.

### Phase 1: Page Speed & Core Web Vitals

1. **WebFetch** the business homepage and top 3 service pages
2. For each page, analyze:
   - Page load indicators (large images, render-blocking scripts, excessive redirects)
   - Mobile viewport meta tag presence
   - Image optimization (WebP format, lazy loading, proper sizing)
   - HTTPS status
   - Redirect chains

3. **WebSearch** `site:{website} pagespeed` or use PageSpeed Insights API if available:
   ```
   WebFetch https://pagespeed.web.dev/analysis?url={website_url}
   ```

4. Check Core Web Vitals indicators:
   - **LCP (Largest Contentful Paint):** Target < 2.5s — check for hero images, large assets
   - **INP (Interaction to Next Paint):** Target < 200ms — check for heavy JavaScript
   - **CLS (Cumulative Layout Shift):** Target < 0.1 — check for images without dimensions, dynamic content injection

**Success criteria:** CWV scores estimated or measured for homepage and top service pages.

### Phase 2: Mobile-Friendliness

1. **WebFetch** homepage and check:
   - `<meta name="viewport">` tag present and correct
   - Responsive CSS indicators (media queries, flexible layouts)
   - Touch target sizing (buttons/links not too small or too close together)
   - Font readability (base font size >= 16px)
   - No horizontal scrolling required

2. **WebSearch** `site:{website} mobile usability` for any known mobile issues

**Success criteria:** Mobile readiness assessed with specific issues identified.

### Phase 3: Crawlability & Indexation

1. **WebFetch** `{website}/robots.txt` — check for overly restrictive rules blocking important pages
2. **WebFetch** `{website}/sitemap.xml` — verify it exists, is valid, and lists all important pages
3. **WebSearch** `site:{website}` — count approximate number of indexed pages
4. Check for:
   - Orphaned pages (pages not in sitemap or internal link structure)
   - Duplicate content indicators (similar title tags across pages)
   - Canonical tag usage
   - Hreflang tags (if multi-language)
   - Meta robots noindex tags accidentally blocking important pages

**Success criteria:** Crawlability issues identified with specific fix instructions.

### Phase 4: Security & Trust

1. **WebFetch** homepage and verify:
   - HTTPS with valid certificate (no mixed content)
   - Security headers present (CSP, X-Frame-Options, HSTS)
   - No insecure resource loading (HTTP images on HTTPS pages)

2. Check for trust signals:
   - SSL certificate validity
   - Business address in footer
   - Phone number click-to-call
   - Privacy policy page exists
   - Terms of service page exists

**Success criteria:** Security and trust issues documented.

### Phase 5: Write Output

**Write** `./reports/00-technical-audit.md` with:
- Executive Summary (overall technical health grade: A/B/C/D/F)
- **Indexation & Discoverability** (ALWAYS first section after summary)
  - Google indexed page count vs sitemap page count
  - Meta description coverage
  - Canonical tag coverage
  - Open Graph coverage
  - If 0 indexed: giant red flag with step-by-step fix instructions
- Core Web Vitals assessment (LCP, INP, CLS with targets)
- Mobile-friendliness checklist (pass/fail for each criterion)
- Crawlability issues (robots.txt, sitemap, indexation)
- Security & Trust checklist
- Priority fix list ordered by impact:
  - CRITICAL: Issues preventing indexation or causing ranking penalties
  - HIGH: CWV failures, mobile issues
  - MEDIUM: Missing security headers, optimization opportunities
  - LOW: Nice-to-haves, minor improvements
- For each issue: what's wrong, exact fix, estimated time to fix

### Phase 6: Update Progress

Update context JSON:
```
progress["00_technical_audit"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/00-technical-audit.md"}
```

**Success criteria:** Technical baseline established with prioritized fix list.

---

## Subcommand 1: keyword-gap (Prompt 9)

**Goal:** Find keywords competitors rank for that we don't. Build a prioritized keyword gap list.

### Phase 1: Competitor Keyword Discovery

For each competitor from context (`competitors[]`):

1. **WebSearch** `site:{competitor.website} {primary_service} {main_city}` to find their indexed service pages
2. **WebSearch** `site:{competitor.website}` to inventory their overall page structure
3. **WebFetch** their homepage and top service pages to extract:
   - Title tags and H1s (these reveal their target keywords)
   - Service names and city mentions
   - Internal link anchor text

**Success criteria:** At least 10 competitor pages analyzed per competitor.

### Phase 2: Market Keyword Discovery

For each service (primary + secondary) and each service area:

1. **WebSearch** `"{service} in {city}"` and note which businesses appear
2. **WebSearch** `"best {service} in {city}"` for high-intent queries
3. **WebSearch** `"emergency {service} {city}"` for urgent-intent queries
4. **WebSearch** `"{service} {city} cost"` and `"{service} {city} price"` for price-intent
5. **WebSearch** `"{service} near {city}"` for near-me patterns
6. **WebSearch** `"{service} {city} reviews"` for review-intent queries

Track for each query: which competitors appeared, whether our site appeared, what position range.

**Success criteria:** At least 30 unique keyword patterns researched.

### Phase 3: Gap Analysis

Build a keyword gap matrix:

| Keyword | Our Site Found | Competitors Found | Intent Stage | Difficulty Est. | Action |
|---------|---------------|-------------------|--------------|----------------|--------|
| ... | yes/no | Competitor A, B | 1-4 | low/med/high | optimize/create |

**Intent Stage classification:**
- Stage 1: Awareness ("what is {service}")
- Stage 2: Research ("best {service} in {city}", "{service} vs {alternative}")
- Stage 3: Comparison ("{service} {city} reviews", "{service} {city} cost")
- Stage 4: Purchase ("emergency {service} {city}", "{service} near me", "hire {service} {city}")

**Prioritization:** Sort by: Stage 4 first, then Stage 3, then by number of competitors present, then by estimated difficulty (low first).

**Success criteria:** Gap matrix with at least 20 keyword opportunities identified and prioritized.

### Phase 4: Write Outputs

1. **Write** `./reports/09-keyword-gap.md` with:
   - Executive Summary (top 5 findings)
   - Data Gathered (search queries run, pages analyzed)
   - Full keyword gap table
   - 2026 Enhancements (note keywords triggering AI Overviews)
   - Action Items (HIGH/MEDIUM/LOW with estimated time to results)
   - Next Steps (which pages to create or optimize first)

2. **Write** `./deliverables/keyword-map.md` with:
   - Service x City keyword matrix
   - Priority keywords by intent stage
   - Content creation queue (ordered list of pages to create/optimize)

### Phase 5: Update Progress

Update context JSON:
```
progress["09_competitor_analysis"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/09-keyword-gap.md"}
```

**Success criteria:** Both report and deliverable written, progress updated.

---

## Subcommand 2: money-page-audit (Prompt 10)

**Goal:** Find pages ranking on page 2 that need one push to page 1. Build a 30-day optimization sprint.

### Phase 1: Determine Data Source

1. Check `./data/gsc-exports/` for CSV files (queries-*.csv, pages-*.csv, search-results-*.csv)
2. If CSVs exist, proceed to **Phase 2A (GSC Path)**
3. If no CSVs exist, proceed to **Phase 2B (WebSearch Path)**

### Phase 2A: GSC Data Analysis (if CSV available)

1. **Read** the most recent queries CSV from `./data/gsc-exports/`
2. Parse CSV data — expected columns: Query, Page, Clicks, Impressions, CTR, Position
3. **Find page 2 goldmine:** Keywords where:
   - Position is 11-20 (page 2)
   - Impressions >= 100 (people are searching)
   - These are the "almost there" keywords — one push gets them to page 1
4. **Find CTR problems:** Pages where:
   - Position is 1-10 (already page 1)
   - CTR is below expected for that position (title/meta needs work)
   - Expected CTR benchmarks: pos 1 = 28%, pos 2 = 15%, pos 3 = 11%, pos 4-10 = 3-8%
5. **Find cannibalization:** Keywords where multiple pages from our site rank — signal that Google is confused about which page to show

For each page 2 keyword, **WebFetch** the ranking page and check:
- Is the keyword in the title tag?
- Is the keyword in the H1?
- Is the keyword in the first 100 words?
- Content length (thin content = under 500 words)
- Number of internal links pointing to this page

**Success criteria:** At least 5 page 2 opportunities identified with specific fixes.

### Phase 2B: WebSearch Analysis (no GSC data)

1. **WebSearch** `site:{our_website}` to inventory all indexed pages
2. **WebFetch** each key page (homepage, service pages, location pages) to audit:
   - Title tag content and length
   - H1 tag content
   - Content length (word count)
   - Internal link count
   - Schema markup presence
   - Image alt text optimization
3. For each target keyword from context:
   - **WebSearch** the keyword
   - Check if our site appears and at what approximate position
   - Note who ranks above us
4. Build optimization recommendations based on on-page gaps found

**Success criteria:** All key pages audited, ranking approximations gathered for target keywords.

### Phase 3: 2026 Enhancement - AI Overviews

For each target keyword:
1. Note if WebSearch results indicate an AI Overview is present
2. For keywords with AI Overviews:
   - Recommend structured, fact-dense content with clear Q&A format
   - Recommend FAQ schema (JSON-LD) for those pages
   - Flag that AI Overviews cause 34-46% CTR drop on organic results
   - Strategy: optimize for position 0 (be the source for AI Overview) OR compensate by targeting more keywords that don't trigger AI Overviews

**Success criteria:** AI Overview impact noted for each keyword.

### Phase 4: Build 30-Day Sprint

Create a week-by-week optimization plan:

**Week 1 — Title Tags and H1 Fixes:**
- Write EXACT new title tags for each page (under 60 chars, keyword + city + brand)
- Write EXACT new H1 tags (natural keyword inclusion)
- Priority: pages closest to page 1 first

**Week 2 — Thin Content Expansion:**
- Identify pages under 500 words
- Write content expansion recommendations (what to add, target word count)
- Add local relevance signals (neighborhood mentions, local landmarks)

**Week 3 — Internal Linking Campaign:**
- Map current internal link structure
- Identify orphaned pages (no internal links pointing to them)
- Write exact anchor text and linking recommendations

**Week 4 — Meta Descriptions and CTR Optimization:**
- Write EXACT new meta descriptions (under 155 chars, keyword + CTA)
- Add FAQ schema to top pages
- Add review schema where applicable

### Phase 5: Write Output

**Write** `./reports/10-money-page-audit.md` with:
- Executive Summary
- Data Source Used (GSC or WebSearch fallback)
- Page 2 Goldmine List (table with: URL, keyword, position, impressions, issue, exact fix)
- CTR Problem Pages (if GSC data available)
- Cannibalization Issues (if found)
- AI Overview Impact Analysis
- 30-Day Optimization Sprint (week by week with exact copy for every fix)
- Action Items (HIGH/MEDIUM/LOW)
- Next Steps

### Phase 6: Update Progress

Update context JSON:
```
progress["10_link_building"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/10-money-page-audit.md"}
```

**Success criteria:** Report written with actionable 30-day sprint, progress updated.

---

## Subcommand 3: service-pages (Prompt 11)

**Goal:** Generate location-specific service pages for every service x city combination.

### Phase 1: Inventory Existing Pages

1. **WebSearch** `site:{our_website}` to find all indexed pages
2. **WebFetch** the sitemap if available (`{our_website}/sitemap.xml`)
3. Build a matrix: services (rows) x cities (columns)
4. Mark which combinations already have pages, which are missing

Display the matrix to the user:

```
Service x City Matrix
=====================
                    | Henderson | Las Vegas | North LV | Boulder City
--------------------|-----------|-----------|----------|-------------
Plumbing            |    [X]    |    [ ]    |   [ ]    |    [ ]
Water Heater Repair |    [ ]    |    [ ]    |   [ ]    |    [ ]
Drain Cleaning      |    [X]    |    [ ]    |   [ ]    |    [ ]
```

**Success criteria:** Complete matrix built with existing pages identified.

### Phase 2: Generate Missing Pages

For EACH missing service x city combination, generate a complete HTML page with:

1. **SEO Title** (under 60 chars): Natural inclusion of service + city
   - Example: "Expert Plumbing Services in Henderson, NV | Acme Plumbing"

2. **Meta Description** (under 155 chars): Service + city + compelling reason to click
   - Example: "Licensed Henderson plumber with 12+ years experience. Same-day service, upfront pricing. Call (702) 555-0123 for a free estimate."

3. **H1 Heading**: Service + city, natural language
   - Example: "Professional Plumbing Services in Henderson, Nevada"

4. **Opening Paragraph** (~100 words): Address a specific pain point common in that city. Reference local conditions, weather patterns, or housing stock that creates demand for the service.

5. **Why Choose Us Section** (~150 words): City-specific content including:
   - Local landmarks or neighborhoods served
   - Area-specific challenges (e.g., hard water in Henderson, older pipes in downtown)
   - Response time to that specific area
   - Years serving that community

6. **Service Details Section** (~200 words): What's included, the process step by step, what the customer gets. Specific to this service type.

7. **Social Proof Section**: Placeholder for city-specific reviews with structured markup
   - Include `<!-- REVIEW_PLACEHOLDER: Add 2-3 reviews from {city} customers -->`

8. **FAQ Section**: 3 questions specific to that city's customers
   - Use conversational phrasing that matches voice search queries
   - Example: "How much does emergency plumbing cost in Henderson?"

9. **CTA Section**: Phone number + "Call now for same-day {service} in {city}"

**Success criteria:** Each generated page has genuinely unique content, not template swaps.

### Phase 3: 2026 Enhancement - Anti-Cookie-Cutter

CRITICAL: Google is filtering templated location pages more aggressively in 2026.

For each page, verify:
- [ ] Content includes city-specific details that cannot apply to other cities
- [ ] At least 2 local landmarks or neighborhoods mentioned by name
- [ ] FAQ questions are unique per city (not the same 3 questions with city swapped)
- [ ] Page structure varies between cities (different section order, different lengths)
- [ ] Local area knowledge demonstrates real expertise (housing age, soil type, common issues)
- [ ] Opening paragraph addresses a genuinely different pain point per city
- [ ] **Information Gain content included** — at least ONE of these unique local data elements per page:
  - Local permit requirements or regulations for the service in that city
  - City-specific pricing context (cost of living adjustments, typical price ranges)
  - Local building code requirements relevant to the service
  - Neighborhood-specific challenges (e.g., hard water zones, soil types, flood zones)
  - Local utility provider information relevant to the service
  - Seasonal timing recommendations specific to that city's climate
- [ ] **Local utility element** — something genuinely useful that a resident would bookmark:
  - Emergency contact numbers (city utilities, emergency services)
  - Permit application links for that city
  - Local inspection requirements and timeline
  - "Before you call a {service provider}" checklist specific to that area

If generating more than 5 pages, use Agent subagents to parallelize — but give each agent different city-specific research data so the pages are genuinely unique.

### Phase 4: 2026 Enhancement - Voice Search Readiness

For each page:
1. Add FAQ schema (JSON-LD) with conversational questions
2. Use conversational H2 headings that match voice query patterns:
   - "How much does {service} cost in {city}?"
   - "What's the best {service} company near {neighborhood}?"
   - "Do I need a permit for {service} in {city}?"
3. Include "near me" optimization in meta data and content
4. Add speakable schema for key sections

### Phase 5: Schema Markup

For each generated page, create a JSON-LD file with:

```json
{
  "@context": "https://schema.org",
  "@type": "{schema_type from context}",
  "name": "{business.name}",
  "description": "{service} in {city}",
  "url": "{page URL}",
  "telephone": "{business.phone}",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "{business.address street}",
    "addressLocality": "{city}",
    "addressRegion": "{state}",
    "postalCode": "{zip}"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "{lat}",
    "longitude": "{lng}"
  },
  "areaServed": {
    "@type": "City",
    "name": "{city}"
  },
  "openingHoursSpecification": [],
  "priceRange": "$$",
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "{service}",
    "itemListElement": []
  }
}
```

Use WebSearch to find geo coordinates for each city if not in context.

### Phase 6: Internal Linking Plan

For each new page:
1. Identify 3 existing pages that should link TO this new page (with exact anchor text)
2. Identify 2 pages this new page should link TO (with exact anchor text)
3. Suggest 2 local directories for citation (city-specific directories, chamber of commerce, etc.)

### Phase 7: Integration Checks

1. Check if website is on Cloudflare Pages:
   ```bash
   cat ~/.claude/config/cf-sites.json 2>/dev/null | python3 -c "import sys,json; d=json.load(sys.stdin); [print(k,v) for k,v in d.items() if '{domain}' in str(v)]" 2>/dev/null
   ```
   If found, note the deployment path for the user.

2. After generation, recommend:
   - Run `seo-audit.py {website}` to validate new pages
   - Run `ping-search-engines.py` after deployment for IndexNow notification

### Phase 8: Write Outputs

1. **Write** `./reports/11-service-city-pages.md` with:
   - Executive Summary (pages generated, matrix coverage)
   - Service x City Matrix (before and after)
   - Anti-Cookie-Cutter Compliance Checklist
   - Internal Linking Plan
   - Deployment Instructions
   - Action Items (HIGH/MEDIUM/LOW)
   - Next Steps

2. **Write** `./deliverables/service-pages/{service-slug}-{city-slug}.html` for each page

3. **Write** `./deliverables/schema-markup/{service-slug}-{city-slug}.json` for each page

### Phase 9: Update Progress

Update context JSON:
```
progress["11_technical_seo"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/11-service-city-pages.md"}
```

**Success criteria:** All missing pages generated with unique content, schema markup created, internal linking plan documented, progress updated.

---

## Subcommand 4: gsc-analysis (Prompt 12)

**Goal:** Deep GSC analysis to find page 2 goldmine and optimization opportunities.

### Phase 1: Check Data Availability

1. Check `./data/gsc-exports/` for CSV files
2. Look for: `queries-*.csv`, `pages-*.csv`, `search-results-*.csv`
3. If files found, proceed to **Phase 2A**
4. If NO files found, proceed to **Phase 2B**

### Phase 2A: Full GSC Analysis (data available)

Read and parse the most recent CSV files.

**Analysis 1 — Page 2 Goldmine:**
- Filter: Position 11-20 AND Impressions >= 100
- Sort by impressions descending (highest opportunity first)
- For each keyword: note the ranking page URL, current position, impressions, clicks, CTR

**Analysis 2 — CTR Problems:**
- Filter: Position 1-10 AND CTR below expected benchmark
- Expected CTR: pos 1 = 28%, pos 2 = 15%, pos 3 = 11%, pos 4 = 8%, pos 5 = 6%, pos 6 = 4%, pos 7-10 = 3%
- These pages rank well but their title/description isn't compelling enough

**Analysis 3 — Keyword Cannibalization:**
- Group by query, find queries where 2+ different pages rank
- Identify the "intended" page and the "cannibal" page
- Recommend: consolidate content, add canonical, or differentiate page intent

**Analysis 4 — Trending Keywords:**
- Compare last 30 days vs previous 60 days
- Find keywords with rising impressions (growing search demand)
- Find keywords with falling position (slipping — need attention)

**Analysis 5 — Quick Win Candidates:**
- Position 4-10 with high impressions = already on page 1, push to top 3
- Position 11-13 with high impressions = closest to page 1, easiest to push over

For each opportunity, **WebFetch** the ranking page and audit:
- Title tag: Does it contain the keyword? Is it under 60 chars?
- H1: Does it match the target keyword?
- First 100 words: Is the keyword present?
- Content length: Is it thin (under 500 words)?
- Internal links: How many pages link to this one?

**Success criteria:** At least 10 optimization opportunities identified with specific fixes.

### Phase 2B: No GSC Data Available

1. Tell the user exactly what to export and where to put it:

```
GSC Export Instructions
=======================

1. Go to Google Search Console (search.google.com/search-console)
2. Select your property: {business.website}
3. Click Performance > Search Results
4. Set date range: Last 3 months
5. Click Export > Download CSV
6. Save the file to: ./data/gsc-exports/search-results-YYYY-MM-DD.csv

Also export:
- Queries tab > Export CSV > save as queries-YYYY-MM-DD.csv
- Pages tab > Export CSV > save as pages-YYYY-MM-DD.csv

Then re-run: /local-seo-website gsc-analysis
```

2. Fall back to WebSearch-based analysis (same as money-page-audit Phase 2B)
3. Note in the report that results are approximate without GSC data

### Phase 3: Build 30-Day Sprint

**Week 1 — Title Tags and H1 Fixes:**
- Write EXACT new title tag for each page (format: `{Keyword} in {City} | {Brand}` under 60 chars)
- Write EXACT new H1 for each page
- Implement changes highest-opportunity first

**Week 2 — Thin Content Expansion:**
- List every page under 500 words with its target keyword
- Write content brief for each: what to add, target word count, sections to include
- Add city-specific and service-specific details

**Week 3 — Internal Link Silo Architecture:**
- Map current internal link structure into topic silos:
  - Each service should have its own silo (e.g., all plumbing pages link to each other)
  - Each city should be cross-linked within its service silo
  - The homepage links to all main silo pages
  - Each silo page links back to the homepage and to related silos
- Identify orphaned pages (zero internal links pointing to them)
- Identify cross-silo leaks (pages linking to unrelated silos, diluting relevance)
- Build a link silo map showing the ideal internal link architecture
- Write exact anchor text and placement for each new internal link
- Target: every important page has at least 3 internal links from topically related pages
- Create a visual silo diagram in markdown format showing the hierarchy

**Week 4 — Meta Descriptions and Schema:**
- Write EXACT new meta descriptions for each page
- Add FAQ schema to top 5 pages
- Add LocalBusiness schema if missing
- Add review aggregate schema where applicable

For EVERY fix in the sprint, write the EXACT copy to use — no vague recommendations.

### Phase 4: Write Output

**Write** `./reports/12-gsc-analysis.md` with:
- Executive Summary
- Data Source (GSC export date range, or WebSearch fallback)
- Page 2 Goldmine Table (URL, keyword, position, impressions, issue, exact fix)
- CTR Problem Pages (if GSC data)
- Cannibalization Issues (if found)
- Trending Keywords (if GSC data)
- 30-Day Optimization Sprint (week by week)
- 2026 Enhancements (AI Overview impact on each keyword)
- Action Items (HIGH/MEDIUM/LOW with time-to-results estimate)
- Next Steps

### Phase 5: Update Progress

Update context JSON:
```
progress["12_photo_strategy"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/12-gsc-analysis.md"}
```

**Success criteria:** Comprehensive analysis written with exact copy for every fix, progress updated.

---

## Subcommand 5: review-sentiment (Prompt 13)

**Goal:** Reverse-engineer the emotional language customers use in reviews to build authentic website copy.

### Phase 1: Gather Competitor Reviews

For each competitor from context:

1. **WebSearch** `"{competitor.name}" reviews {city}` to find review snippets
2. **WebSearch** `site:yelp.com "{competitor.name}" {city}` for Yelp reviews
3. **WebSearch** `site:bbb.org "{competitor.name}"` for BBB reviews
4. **WebFetch** competitor GBP review pages if accessible
5. Collect at least 20 review snippets per competitor

Extract from each review:
- Emotional words used (relieved, impressed, finally, trustworthy, etc.)
- Specific outcomes mentioned (fixed in one visit, no mess, arrived on time)
- Fears/frustrations mentioned BEFORE service (worried about cost, others kept canceling)
- Recommendation language ("I would recommend..." phrases)
- Star rating context (what makes a 5-star vs 3-star)

**Success criteria:** At least 40 competitor review snippets collected and analyzed.

### Phase 2: Deep Sentiment Analysis — Competitors

Analyze all collected competitor reviews and extract:

1. **Top 20 Emotional Words** — ranked by frequency
   - Examples: relieved, impressed, finally, trustworthy, professional, honest, amazed, grateful, surprised, comfortable
   - Note which emotions appear in 5-star reviews vs 3-star reviews

2. **Top 10 Specific Outcomes** — what customers specifically praise
   - Examples: fixed in one visit, no mess left behind, arrived on time, explained everything, upfront pricing, same-day service
   - Note which outcomes get mentioned most in recommendations

3. **Top 5 Fears/Frustrations** — what customers mention about BEFORE the service
   - Examples: worried about cost, previous company no-showed, tried to fix it myself, other companies wanted to replace everything, been putting it off for months
   - These are the pain points website copy should address

4. **Money Phrases** — exact words used when recommending to others
   - Examples: "I tell everyone about...", "If you need a plumber, call these guys", "Worth every penny"
   - These phrases go directly into social proof sections

5. **5-Star vs 3-Star Language Patterns:**
   - What 5-star reviews mention that 3-star reviews don't
   - What 3-star reviews complain about (these are differentiators to highlight)

**Success criteria:** All 5 analysis categories populated with specific language examples.

### Phase 3: Our Review Analysis

1. **WebSearch** `"{business.name}" reviews {main_city}` to find our review snippets
2. **WebSearch** `site:yelp.com "{business.name}" {main_city}`
3. Collect and analyze our reviews using the same framework as Phase 2
4. **Find emotional gaps:**
   - Words competitors' reviewers use that our reviewers don't (opportunity to prompt these words)
   - Outcomes competitors highlight that we don't mention on our website
   - Fears our website doesn't address that reviews show customers have

**Success criteria:** Gap analysis between our review language and competitor review language.

### Phase 4: Rewrite Copy Using Review Language

Using the extracted emotional language, write:

1. **GBP Description** (750 chars max): Rewrite using real customer language
   - Open with the #1 emotional outcome customers mention
   - Address the #1 fear/frustration
   - Include a money phrase
   - End with CTA

2. **Homepage Headline and Subheadline:**
   - Headline: Address the #1 fear or promise the #1 outcome
   - Subheadline: Include specific proof point from reviews (e.g., "Rated 4.8 stars — 47 homeowners trust us")

3. **Review Request Script:** A message template the business can send after service
   - Naturally prompts customers to mention the emotional words that help rankings
   - Example: "We'd love to hear about your experience. What was the problem you needed help with? How did our team handle it? Would you recommend us to a neighbor?"

4. **3 Social Proof Statements** for the website:
   - Based on the most common 5-star language
   - Formatted as quotable testimonial summaries
   - Include specific outcomes, not vague praise

### Phase 5: 2026 Enhancement — E-E-A-T Signals

Ensure ALL rewritten copy includes:

1. **Experience signals:** Reference specific years in business, number of jobs completed, local projects
   - Example: "Serving Henderson since 2014" not just "experienced"

2. **Expertise markers:** Certifications, specialized training, manufacturer partnerships
   - Example: "Rheem Certified installer" not just "certified"

3. **Authoritativeness:** Specific data points from real reviews
   - Example: "4.8 stars from 47 verified customers" not just "highly rated"

4. **Trust signals:** Insurance, licensing, warranty details
   - Example: "Nevada License #12345, fully insured" not just "licensed and insured"

Review each piece of rewritten copy and verify it contains at least 2 E-E-A-T signals.

### Phase 6: Write Outputs

1. **Write** `./reports/13-review-sentiment.md` with:
   - Executive Summary
   - Data Gathered (review sources, count of snippets analyzed)
   - Competitor Sentiment Analysis (all 5 categories with examples)
   - Our Review Analysis
   - Gap Analysis (emotional language we're missing)
   - 2026 E-E-A-T Enhancement Notes
   - Action Items (HIGH/MEDIUM/LOW)
   - Next Steps

2. **Update deliverables:**
   - `./deliverables/gbp-description.md` — rewritten GBP description
   - `./deliverables/homepage-copy.md` — headline, subheadline, social proof statements
   - `./deliverables/review-request-script.md` — the review request template

### Phase 7: Update Progress

Update context JSON:
```
progress["13_qa_optimization"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/13-review-sentiment.md"}
```

**Success criteria:** Sentiment analysis complete, copy rewritten with real customer language, all deliverables written, progress updated.

---

## Progress Tracking

After EVERY subcommand completes successfully, update the business context JSON:

1. Read `~/.claude/config/local-seo/{slug}.json`
2. Update the relevant progress entry:
   ```json
   {
     "status": "completed",
     "last_run": "YYYY-MM-DD",
     "output": "./reports/0X-name.md"
   }
   ```
3. Update `updated_at` to current ISO timestamp
4. Write back to the config file

Progress key mapping:
- `technical-audit` -> `progress.00_technical_audit`
- `keyword-gap` -> `progress.09_competitor_analysis`
- `money-page-audit` -> `progress.10_link_building`
- `service-pages` -> `progress.11_technical_seo`
- `gsc-analysis` -> `progress.12_photo_strategy`
- `review-sentiment` -> `progress.13_qa_optimization`

---

## Report Format (All Subcommands)

Every report follows this structure:

1. **Executive Summary** — 3-5 bullet points of key findings
2. **Data Gathered** — what was searched/fetched/analyzed and when
3. **Analysis** — tables, gap matrices, findings with specific data
4. **2026 Enhancements** — AI Overview impact, E-E-A-T signals, voice search, anti-template measures
5. **Action Items** — prioritized list with:
   - Priority: HIGH / MEDIUM / LOW
   - Estimated time to implement
   - Estimated time to see results (e.g., "2-4 weeks for indexing")
   - Exact copy/content to use (no vague recommendations)
6. **Next Steps** — which subcommand or skill to run next

---

## Error Recovery

| Sub-command | Failure | Recovery |
|-------------|---------|----------|
| keyword-gap | WebSearch rate limited | Batch queries with pauses between searches. Process top 3 services x top 3 cities first, expand later. |
| keyword-gap | Competitor website blocks WebFetch | Fall back to WebSearch snippets only. Note in report that on-page analysis was limited. |
| money-page-audit | No GSC data in ./data/gsc-exports/ | Fall back to WebSearch-based analysis. Provide detailed GSC export instructions in report. |
| money-page-audit | GSC CSV has unexpected format | Try common column name variations (Query/query/Keyword, Page/URL, etc.). If unparseable, tell user the expected format. |
| service-pages | Too many service x city combinations (>20) | Prioritize top 3 services x top 3 cities first. Generate remaining in batches. Use Agent subagents for parallelism. |
| service-pages | Cannot determine existing pages via WebSearch | Ask user to provide sitemap URL or list of existing pages manually. |
| service-pages | WebFetch blocked by robots.txt | Use WebSearch cached snippets instead. Note limitation in report. |
| gsc-analysis | No GSC data available | Provide step-by-step export instructions. Fall back to WebSearch analysis with clear caveat about accuracy. |
| gsc-analysis | CSV too large to parse in memory | Process in chunks — read first 1000 rows, analyze, then next 1000. |
| review-sentiment | Cannot access competitor reviews | Use WebSearch snippets which often include review text. Focus on Google review snippets visible in search results. |
| review-sentiment | Very few reviews available (<10) | Expand search to neighboring cities, Yelp, BBB, Facebook, Angi/HomeAdvisor. Note small sample size in report. |
| technical-audit | Cannot access PageSpeed Insights | Fall back to manual analysis via WebFetch — check for large images, missing meta viewport, render-blocking resources |
| technical-audit | Website behind Cloudflare challenge page | Note that Cloudflare protection is active (good for security), attempt WebFetch of specific pages, report what's accessible |
| Any | Context JSON missing or corrupt | Stop and direct user to `/local-seo-context setup` or `/local-seo-context status`. |
| Any | WebSearch returns no results | Try alternative query phrasings. Remove quotes. Try broader terms. If still failing, note the gap and continue with available data. |
