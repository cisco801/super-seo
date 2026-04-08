---
name: local-seo-content
description: |
  Content strategy, entity optimization, competitor post monitoring, and monthly reporting
  for local SEO. Covers prompts 17-20: content gap analysis, Knowledge Graph entity building,
  competitor GBP post reverse-engineering, and performance reporting.
user-invocable: true
argument-hint: "[gap-analysis | entity-optimization | competitor-posts | monthly-report]"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO: Content Strategy & Reporting

**Request:** $ARGUMENTS

4 sub-commands covering prompts 17-20 of the local SEO system: content gap analysis, entity optimization, competitor post intelligence, and monthly performance reporting.

## Argument Parsing

Parse `$ARGUMENTS` for:
- **`gap-analysis`** = run prompt 17 (content gap analysis)
- **`entity-optimization`** = run prompt 18 (Knowledge Graph entity building)
- **`competitor-posts`** = run prompt 19 (competitor GBP post reverse-engineering)
- **`monthly-report`** = run prompt 20 (monthly performance report)
- **No arguments** = show available sub-commands and current progress for prompts 17-20

## Phase 0: Prerequisite (Every Run)

1. Read `./local-seo-context.json` to get the active business slug
2. Read `~/.claude/config/local-seo/{slug}.json` to load full business context (business name, city, service areas, target keywords, competitor names, services offered, website URL, schema type, current standings)
3. If either file is missing or the slug is empty, stop and tell the user: "No business context found. Run `/local-seo-context setup` first."
4. Create `./reports/` and `./deliverables/` directories if they do not exist

**Success criteria:** Business context loaded with at minimum: business name, city, website URL, top 3 target keywords, competitor names, list of services offered.

---

## Sub-Command 1: `gap-analysis` (Prompt 17)

**Goal:** Find every piece of content competitors have that we are missing.

### Phase 1 - Competitor Content Inventory

For each competitor from context:
- WebSearch `site:{competitor_website}` to get an overview of indexed pages
- WebSearch `site:{competitor_website} blog` to find blog content
- WebSearch `site:{competitor_website} {primary_service}` to find service pages
- WebSearch `site:{competitor_website} {city}` to find location pages

Categorize all discovered competitor content into:
- **Service pages** (individual service descriptions)
- **Location pages** (city/neighborhood-specific pages)
- **Blog posts** (educational and informational content)
- **FAQ pages** (question-and-answer content)
- **Guides** (long-form how-to or resource content)

**Success criteria:** Content inventory completed for at least 2 competitors with pages categorized by type.

### Phase 2 - Gap Identification

- Cross-reference competitor content inventory against the business website content (WebSearch `site:{business_website}`)
- Filter to keywords with local intent in the 50-500 monthly search volume sweet spot
- Focus on question-format keywords: how, why, what, when, is, can, does
- Focus on keywords related to problems the business solves

**Success criteria:** At least 15 content gaps identified with local intent keywords.

### Phase 3 - Categorize Gaps

Sort every gap into one of three intent stages:

1. **Problem-awareness content** - Someone has a problem but does not know the solution yet (e.g., "why is my AC not cooling", "water heater making noise"). These answer objections before the phone call.
2. **Solution-comparison content** - Someone evaluating options (e.g., "repair vs replace water heater", "tankless vs traditional water heater"). These position the business as a knowledgeable authority.
3. **Local service content** - Someone ready to hire (e.g., "[service] [city] cost", "emergency [service] near me"). These capture bottom-of-funnel searchers.

**Success criteria:** Every gap keyword assigned to an intent stage.

### Phase 4 - Content Briefs

For the top 20 gap keywords (prioritized by intent stage: problem-awareness first), write:

- **Suggested page title** (SEO optimized, under 60 characters)
- **URL slug** (lowercase, hyphenated)
- **200-word content brief** containing:
  - Target keyword and 3-5 secondary keywords
  - Questions the content must answer
  - Recommended word count (500-1500 depending on topic depth)
  - Internal links to existing pages
  - CTA (call, schedule, get quote)
- **Priority rating:** HIGH / MEDIUM / LOW based on intent stage and estimated volume

**Success criteria:** 20 content briefs written with complete targeting data.

### Phase 5 - 2026 Enhancement: AI Overviews / GEO

For each content piece in the top 20:
- Note whether the target keyword triggers AI Overviews in Google search results
- Structure content recommendations for AI citation: clear factual statements, structured data, FAQ schema
- Include specific data points and statistics (AI Overviews prefer fact-dense content over opinion)
- Recommend `FAQPage` schema markup for question-format content

**Success criteria:** AI Overview assessment noted for each of the top 20 content briefs.

### Output

Write `./reports/17-content-gap.md` with standard report format.

Write `./deliverables/content-calendar.md` with:
- 12-week content calendar mapping briefs to publication dates (1-2 pieces per week)
- Content organized by intent stage (problem-awareness first, then solution-comparison, then local service)
- Each entry includes: week number, title, URL slug, target keyword, word count, priority

---

## Sub-Command 2: `entity-optimization` (Prompt 18)

**Goal:** Build and strengthen the business entity in Google's Knowledge Graph.

This is the most advanced prompt in the system. Most local SEOs do not know this lever exists.

### Phase 1 - Knowledge Panel Check

- WebSearch `"{business_name}" "{city}"` -- does a Knowledge Panel appear in results?
- WebSearch `"{owner_name}" "{business_name}"` -- any entity recognition for the owner? (use owner name from context if available, otherwise skip)
- Document what appears: Knowledge Panel, Local Pack, organic listing, or nothing

**Success criteria:** Knowledge Panel presence documented (yes/no/partial).

### Phase 2 - Wikidata Check

- WebSearch `site:wikidata.org "{business_name}"` -- does an entity exist?
- If not found, note as an opportunity to create a Wikidata entry
- If found, document the Wikidata QID and check for completeness

**Success criteria:** Wikidata entity status documented.

### Phase 3 - Schema Markup Audit

- WebFetch the business website homepage
- Search the HTML for existing JSON-LD structured data (`<script type="application/ld+json">`)
- Parse and catalog what schema types are present
- Identify what is present vs. missing against the full LocalBusiness schema specification

**Success criteria:** Current schema markup cataloged with gap list.

### Phase 4 - Build Complete Schema

Write a full JSON-LD `LocalBusiness` schema using the SPECIFIC subtype from context (e.g., `Plumber`, `Dentist`, `HVACBusiness` -- NEVER use generic `LocalBusiness` when a specific subtype exists):

```json
{
  "@context": "https://schema.org",
  "@type": "[specific subtype from context schema_type]",
  "name": "[business name]",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "[street]",
    "addressLocality": "[city]",
    "addressRegion": "[state]",
    "postalCode": "[zip]",
    "addressCountry": "US"
  },
  "telephone": "[phone]",
  "url": "[website]",
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "[lat]",
    "longitude": "[lng]"
  },
  "openingHours": "[hours]",
  "priceRange": "[range]",
  "areaServed": [
    { "@type": "City", "name": "[service area 1]" },
    { "@type": "City", "name": "[service area 2]" }
  ],
  "sameAs": ["[social profiles and directory URLs]"],
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "[rating]",
    "reviewCount": "[count]"
  },
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Services",
    "itemListElement": [
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "[service name]"
        }
      }
    ]
  }
}
```

Populate every field possible from context data. For fields not in context (hours, geo coordinates, social profiles), add placeholder comments noting what the business needs to provide.

**Success criteria:** Complete JSON-LD schema generated with all available context data populated.

### Phase 5 - Entity Strengthening Plan

Build an action plan to strengthen entity signals across the web:

- **Directory presence:** List of authoritative sites to create or claim presence on:
  - LinkedIn company page
  - Crunchbase (if applicable)
  - Industry-specific associations and directories
  - Local chamber of commerce
  - Better Business Bureau
  - Yelp, Angi, HomeAdvisor (or industry equivalents)
- **Wikipedia eligibility:** Assess whether the business meets Wikipedia notability criteria. If eligible, document the path. If not, note what milestones would qualify.
- **Wikidata entry:** If no Wikidata entity exists, provide step-by-step creation instructions
- **Brand mention strategy:** Recommended anchor text variations and contexts for brand mentions across the web
- **Knowledge Panel triggering:** Specific steps to trigger or claim a Google Knowledge Panel
- **NAP consistency:** Checklist of all entity touchpoints where Name, Address, Phone must match exactly

**Success criteria:** At least 10 specific entity strengthening actions documented with priority ratings.

### Phase 6 - 2026 Enhancement: AI Mode / Gemini Entity Verification

- Schema now feeds Google's AI Mode (Gemini) for entity verification and citation decisions
- Businesses with strong entity signals rank higher in AI Overviews
- Stronger entity = more resilient to algorithm updates
- Include `sustainabilityPolicy` and eco-label schema properties if applicable to the business
- Recommend structured data testing via Google Rich Results Test

**Success criteria:** AI Mode entity considerations documented.

### Output

Write `./reports/18-entity-optimization.md` with standard report format.

Write `./deliverables/schema-markup/homepage.json` with the complete JSON-LD schema (ensure the `./deliverables/schema-markup/` directory exists).

---

## Sub-Command 3: `competitor-posts` (Prompt 19)

**Goal:** Reverse-engineer competitor GBP posting patterns to build a data-driven posting strategy.

### Phase 1 - Forensic Analysis

For each competitor from context, WebSearch their GBP posts and extract:
- **Posting frequency** (posts per week / per month)
- **Post types used** (offer, update, event, product)
- **Topics and services mentioned** in posts
- **Geographic mentions** (neighborhoods, cities, service areas)
- **Pricing or offer mentions** (discounts, free estimates, seasonal deals)
- **Seasonal patterns** (holiday posts, weather-related, back-to-school, etc.)
- **Gaps in posting schedule** (weeks or months with no posts)

**Success criteria:** Posting data extracted for at least 2 competitors.

### Phase 2 - Pattern Analysis

Across all competitors, identify:
- Which days of the week competitors post most frequently
- Which post types they favor (offers vs. updates vs. events)
- Seasonal topic patterns (what gets posted in which months)
- Content themes that appear repeatedly
- Gaps we can exploit (topics no competitor covers, days no competitor posts)

**Success criteria:** At least 5 actionable patterns or gaps identified.

### Phase 3 - Build Data-Driven Strategy

Based on competitor data (NOT generic advice), build:

- **Optimal posting schedule:** Days and frequency calibrated to this specific market
- **Post type mix:** Distribution of offer/update/event/product posts based on what works in this market
- **Topic rotation:** Covering all services from context + all service areas, weighted by competitor gaps
- **4 weeks of complete post copy:** For each post include:
  - Post type (offer / update / event / product)
  - Full copy (100-150 words)
  - Target keyword inclusion
  - CTA (call, visit, book online)
  - Image description (what photo to use)
  - Scheduled day and time

**Success criteria:** 4 weeks of posts written (minimum 8 posts, maximum 16), each mentioning at least 1 service and 1 service area.

### Output

Write `./reports/19-competitor-posts.md` with:
- Executive summary of competitor posting patterns
- Competitor-by-competitor forensic analysis
- Pattern analysis with data tables
- Complete 4-week posting strategy with full copy
- Recommended posting schedule going forward

---

## Sub-Command 4: `monthly-report` (Prompt 20)

**Goal:** Build a monthly SEO performance report tracking only what matters. Readable in 5 minutes.

### Phase 1 - Data Collection

**GSC Data (if available):**
- Check `./data/gsc-exports/` for CSV files
- If found, parse the most recent CSV for:
  - Total organic clicks + change vs. prior month
  - Total impressions + change
  - Average CTR + change
  - Average position + change
  - Top 10 keywords by clicks
  - Top 10 keywords that improved position
  - Top 10 keywords that dropped position
  - Pages gaining vs. losing clicks

**WebSearch-based metrics:**
- WebSearch `site:{business_website}` to estimate current indexation count
- WebSearch `"{business_name}" "{city}"` to check current search presence

**Previous reports:**
- Read previous reports in `./reports/` for comparison data points
- Read context JSON for baseline metrics from `current_standings`

**Success criteria:** At least one data source provides usable metrics. If no GSC data exists, proceed with WebSearch-only metrics and include GSC setup instructions.

### Phase 2 - GBP Metrics (WebSearch-Based Estimates)

- WebSearch the business GBP listing for current review count and rating
- Compare against the review count in context JSON (last known value)
- Calculate review velocity trend (new reviews since last report)
- Check for recent GBP posts (estimate post frequency since last report)

**Success criteria:** Current review count and rating captured.

### Phase 3 - Report Generation

Write the monthly report in this exact format (designed to be read in 5 minutes):

#### Section 1: 3 Wins This Month
What improved. Specific numbers wherever possible. Celebrate momentum.

#### Section 2: 3 Problems to Address
What needs work. Be direct and honest (per `work_preferences.honesty` in context). Include severity rating for each problem.

#### Section 3: Single Most Important Action
The ONE thing to focus on next month. Not three things. One thing. With a clear explanation of why this one action will move the needle most.

#### Section 4: Calls/Revenue Trend
Did GBP calls, direction requests, or website clicks go up or down? Use available data or note as "data needed" with instructions on how to track.

#### Section 5: Progress Against 12-Week Plan
- Which prompts (1-20) have been completed
- What was completed this month
- What is next in the sequence
- Overall percentage complete

#### Section 6: Comparison vs. Baseline
How current metrics compare to when we started (from context `current_standings`):
- Reviews: then vs. now
- Estimated traffic: then vs. now
- Map pack status: then vs. now
- Keywords ranking: then vs. now

### Phase 4 - 2026 Enhancement

- Track AI Overview appearances for target keywords (WebSearch each target keyword and note if AI Overview appears)
- Track local pack position vs. organic position separately (local pack ads increased 733% in 2025-2026)
- Note any recent algorithm update impact (reference March 2026 core update if relevant)
- Flag keywords where the business appears in AI Overviews as a citation source

**Success criteria:** AI Overview tracking for at least top 5 target keywords.

### Phase 5 - Update Context Standings

After generating the report, update `current_standings` in the context JSON with the latest metrics captured during this report run.

**Success criteria:** Context JSON updated with fresh metric values.

### Output

Write `./reports/20-monthly-report.md` with the 6-section format above.

If no GSC data was available, append a section: **"How to Set Up GSC Exports"** with step-by-step instructions (reference the README in `./data/gsc-exports/` if it exists).

---

## Progress Tracking

After each sub-command completes, update the business context JSON at `~/.claude/config/local-seo/{slug}.json`:

```json
{
  "progress": {
    "17_content_gap": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/17-content-gap.md"},
    "18_entity_optimization": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/18-entity-optimization.md"},
    "19_competitor_posts": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/19-competitor-posts.md"},
    "20_monthly_report": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/20-monthly-report.md"}
  }
}
```

Use today's date in `YYYY-MM-DD` format for `last_run`.

---

## Standard Report Format (All Sub-Commands)

Every report file must include these sections:

1. **Executive Summary** - 2-3 sentences summarizing findings and top recommendation
2. **Data Gathered** - What was found, sources used, data quality notes
3. **Analysis** - Comparison tables, gaps identified, quantitative findings
4. **2026 Enhancements** - What is new or different for 2026 algorithm updates
5. **Action Items** - Prioritized list, each with:
   - Impact rating: HIGH / MEDIUM / LOW
   - Estimated time to see results (days/weeks/months)
   - Effort level: Quick Win / Moderate / Significant
6. **Next Steps** - Which prompt to run next in the sequence

---

## Error Recovery

| Sub-command | Failure | Recovery |
|---|---|---|
| Any | No context file found | Tell user to run `/local-seo-context setup` first |
| Any | WebSearch returns limited data | Note data gaps in the report, proceed with available info, add manual verification action item |
| gap-analysis | Too many gaps found (50+) | Prioritize top 20 by intent stage and estimated volume, note remaining gaps in an appendix |
| gap-analysis | No competitor website indexed | Use GBP listing and WebSearch snippets as proxy for content inventory |
| entity-optimization | Cannot access website for schema audit | Build schema entirely from context data, note that on-site verification is needed |
| entity-optimization | No Knowledge Panel exists | Document this as the primary opportunity, prioritize Wikidata creation and directory presence |
| competitor-posts | No competitor posts visible | Note as competitive advantage (competitors not posting), build strategy from industry best practices and seasonal patterns |
| competitor-posts | Only 1 competitor has posts | Analyze that competitor deeply, supplement with WebSearch for industry posting benchmarks |
| monthly-report | No GSC data available | Use WebSearch-only metrics, provide detailed GSC export instructions, set up tracking baselines |
| monthly-report | No previous reports for comparison | Use context `current_standings` as baseline, note this is the first report |
