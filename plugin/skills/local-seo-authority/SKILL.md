---
name: local-seo-authority
description: |
  Authority building for local SEO — backlink audit, citation audit, search intent
  mapping, and competitor spam audit (prompts 14-16 + spam fighting). Finds link opportunities,
  fixes NAP inconsistencies, maps keywords to buyer journey stages, and identifies fake
  competitor profiles for reporting.
user-invocable: true
argument-hint: "[backlink-audit|citation-audit|search-intent|spam-audit]"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO Authority Building (Prompts 14-16)

**Command:** $ARGUMENTS

Covers three authority-building sub-commands: backlink audit and link acquisition planning, citation NAP consistency audit, and search intent mapping across buyer journey stages.

## Argument Parsing

Parse `$ARGUMENTS` for the sub-command:

- **`backlink-audit`** = Prompt 14 — competitor backlink research and 90-day link acquisition plan
- **`citation-audit`** = Prompt 15 — NAP consistency audit across directories
- **`search-intent`** = Prompt 16 — keyword-to-buyer-journey mapping
- **`spam-audit`** = Identify and report fake competitor GBP listings

If `$ARGUMENTS` is empty or unrecognized, list the three sub-commands and ask which to run.

---

## Prerequisite: Load Business Context

Before any sub-command executes:

1. Read `./local-seo-context.json` in the current project directory
2. Extract `config_path` and read the full config from `~/.claude/config/local-seo/{slug}.json`
3. Parse the config and hold it in memory for all subsequent phases

If `./local-seo-context.json` does not exist or the config file is missing, stop and tell the user:

> No business context found. Run `/local-seo-context setup` first to create one.

**Success criteria:** Config loaded, business name, address, phone, website, services, service areas, competitors, and SEO goals all accessible.

### Site Type Gate (citation-audit only)

For the `citation-audit` sub-command, check `business.site_type`:
- If `local_service`: proceed normally (full NAP consistency audit)
- If `professional_service`: proceed but note that citations are less critical for this type
- If `saas_product`, `free_tool`, `content_site`: Stop and suggest alternatives:
  > "Citation audits check NAP consistency across local directories — this is primarily for local service businesses. For {site_type} sites, try `/local-seo-website keyword-gap` or `/local-seo-content entity-optimization` instead."
  > Use `--force` to run anyway.
- `backlink-audit` and `search-intent` work for ALL site types — no gate needed
- `spam-audit` only applies to `local_service` — gate same as citation-audit

---

## Sub-Command: backlink-audit (Prompt 14)

**Goal:** Discover where competitors earn backlinks and build a 90-day link acquisition plan with ready-to-send outreach emails.

### Phase 1: Competitor Backlink Discovery

For each competitor in `config.competitors`:

1. **Mention search:** WebSearch `"{competitor name}" -site:{competitor domain}` to find external mentions and links
2. **Link operator:** WebSearch `link:{competitor domain}` (limited but sometimes returns results)
3. **Contextual search:** WebSearch `"{competitor name}" "{city}" news|blog|sponsor|partner|association`
4. **Directory search:** WebSearch `"{competitor name}" directory|listing|profile`

Also run general discovery searches:

5. **Local directories:** WebSearch `"{industry}" directory "{city}"` and `"{industry}" "{state}" business listing`
6. **Chambers of commerce:** WebSearch `chamber of commerce "{city}" member directory`
7. **Industry associations:** WebSearch `"{industry}" association "{state}" member`
8. **Local news mentions:** WebSearch `"{industry}" "{city}" news|feature|spotlight`
9. **Guest post opportunities:** WebSearch `"write for us" "{industry}" "{state}"` and `"{industry}" guest post "{city}"`
10. **Local sponsorship opportunities:** WebSearch `"{city}" sponsor|sponsorship youth|little league|school|charity|event`
11. **Community involvement:** WebSearch `"{city}" volunteer|donate|community event {industry}` for community engagement opportunities

**Success criteria:** At least 15 potential link sources identified across all searches.

### Phase 2: Categorize and Analyze

For each link opportunity found, record:

- **Domain** — the linking site
- **Type** — one of: directory, news, blog, association, sponsor, PR mention, chamber, government, education, partner
- **How competitor earned it** — listing, sponsorship, guest post, news coverage, membership, etc.
- **Which competitors have it** — mark which of the 3 competitors appear on this domain
- **Realistic chance** — HIGH (just submit/claim), MEDIUM (outreach needed), LOW (requires relationship/PR)
- **Outreach strategy** — 1-2 sentence approach

Prioritize by:
- Domains linking to ALL 3 competitors = highest priority (proven link source)
- Domains linking to 2/3 competitors = medium priority
- Domains linking to 1 competitor = standard priority

**Success criteria:** All opportunities categorized with type, chance rating, and outreach strategy.

### Phase 3: Build 90-Day Link Acquisition Plan

Organize into three monthly phases:

**Month 1 — 5 Easiest Wins (directories, citations, local associations):**
- Select the 5 highest-chance opportunities (directories, chambers, industry listings)
- For each: domain, type, specific action steps, expected timeline
- Write a complete outreach email for each (ready to send, personalized to the business)

**Month 2 — 5 Medium Opportunities (local news, sponsors, guest posts):**
- Select 5 opportunities requiring outreach or content creation
- For each: domain, type, specific action steps, expected timeline
- Write a complete outreach email for each

**Month 3 — 5 Authority Targets (industry publications, .gov, .edu):**
- Select 5 high-authority opportunities that require more effort
- For each: domain, type, specific action steps, expected timeline
- Write a complete outreach email for each

**Success criteria:** 15 total targets across 3 months, each with action steps and a ready-to-send outreach email.

### Phase 3.5: Local Sponsorship Leads

From the sponsorship opportunities discovered in Phase 1, build a targeted list:

**For each sponsorship opportunity:**
- Organization name and contact info
- Type: youth sports team, school event, charity run, community festival, chamber event
- Sponsorship level options (if discoverable) and estimated cost
- Link value: will the sponsorship include a backlink? (website listing, event page, program ad)
- Community visibility: how many local people will see the sponsorship
- Recommended approach: email, phone, in-person at next meeting

**Priority ranking:**
1. Opportunities that include a dofollow backlink from a .org or .edu domain
2. Opportunities with high local visibility (events with 500+ attendees)
3. Ongoing sponsorships (annual events, season-long team sponsorships)

Write `./deliverables/local-sponsorship-leads.md` with the full list.

### Phase 4: Write Reports

Create output directories if needed:
```bash
mkdir -p ./reports ./deliverables
```

Write three files:

1. **`./reports/14-backlink-audit.md`** — Full report following the standard report format (see Report Format section)
2. **`./deliverables/backlink-targets.md`** — Clean target list organized by month with domain, type, priority, action steps
3. **`./deliverables/outreach-emails.md`** — All 15 outreach emails organized by month, each with subject line, body, and follow-up timing
4. **`./deliverables/local-sponsorship-leads.md`** — Local sponsorship opportunities with contact info, cost estimates, and link value

**Success criteria:** All three files written with complete content.

### Phase 5: Update Progress

Update context JSON:
```
progress["14_backlink_audit"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/14-backlink-audit.md"}
```

Write updated config back to `~/.claude/config/local-seo/{slug}.json` with refreshed `updated_at`.

---

## Sub-Command: citation-audit (Prompt 15)

**Goal:** Find and fix NAP (Name, Address, Phone) inconsistencies across all major directories.

### Phase 1: Check Presence on Core Platforms

WebSearch for the business name on each of these platforms to determine if a listing exists:

**General directories (check all):**
- Google Business Profile
- Yelp
- Bing Places
- Apple Maps
- Facebook
- BBB (Better Business Bureau)
- Angi
- HomeAdvisor
- Thumbtack
- Houzz
- Yellow Pages
- Manta
- Foursquare
- Superpages
- Citysearch

**Industry-specific directories** (select based on `config.business.services.primary`):
- Medical: Healthgrades, Zocdoc, Vitals, WebMD
- Legal: Avvo, FindLaw, Justia, Lawyers.com
- Dental: Healthgrades, Zocdoc, 1-800-Dentist
- Home services: HomeAdvisor, Angi, Porch, Houzz
- Restaurant: TripAdvisor, OpenTable, Zomato
- Auto: CarFax, RepairPal, AutoMD
- Real estate: Zillow, Realtor.com, Trulia
- Financial: NerdWallet, Bankrate

For each platform, WebSearch: `site:{platform domain} "{business name}" "{city}"`

**Success criteria:** Presence checked on all 15 core platforms plus relevant industry directories.

### Phase 2: Verify NAP Consistency

For each platform where a listing was found, use WebFetch or WebSearch to verify:

- **Business name** — exact match to `config.business.name`? Note any variations (abbreviations, old names, misspellings)
- **Address** — exact match to `config.business.address`? Note missing suite/unit numbers, old addresses, formatting differences
- **Phone number** — exact match to `config.business.phone`? Note old numbers, tracking numbers, alternate formats
- **Website URL** — correct? Note http vs https, www vs non-www, old domains
- **Duplicate listings** — are there multiple listings for the same business on this platform?
- **Rating/review count** — record for reference

Create a consistency matrix:

```
Platform         | Name | Address | Phone | URL  | Dupes | Rating | Reviews
-----------------|------|---------|-------|------|-------|--------|--------
Google Business  | OK   | OK      | OK    | OK   | No    | 4.6    | 47
Yelp             | DIFF | OK      | MISS  | OLD  | Yes   | 4.0    | 12
Bing Places      | MISS | MISS    | MISS  | MISS | -     | -      | -
...
```

Legend: OK = matches, DIFF = different, MISS = not found/not listed, OLD = outdated

**Success criteria:** Consistency matrix populated for all found listings.

### Phase 3: 2026 Enhancement — Citation Freshness

Citation freshness is now a ranking factor. Google tracks when citations are last updated.

For each listing found:
- Note whether the listing appears recently updated or stale (look for last-modified dates, recent activity indicators, last review date as a proxy)
- Flag listings that appear to have been untouched for 6+ months
- Recommend a quarterly update cycle for all citations
- Create a citation maintenance calendar with specific dates:
  - Q1: January — update all directory listings (hours, photos, description)
  - Q2: April — update all directory listings + add seasonal content
  - Q3: July — update all directory listings + refresh photos
  - Q4: October — update all directory listings + holiday hours

**Success criteria:** Freshness assessed for each listing, maintenance calendar created.

### Phase 4: Gap Analysis and Fix Plan

**Inconsistency damage assessment:**
- Flag ALL inconsistencies (these actively suppress local rankings)
- Rank by severity:
  - CRITICAL: Wrong phone number, wrong address (sends customers to wrong place)
  - HIGH: Business name variations (confuses Google's entity matching)
  - MEDIUM: Missing website URL, old URL (loses referral traffic)
  - LOW: Formatting differences (e.g., "St" vs "Street")

**Priority fix list** — ordered by impact:
For each inconsistency, provide:
- Platform name and URL to the listing management page
- What is wrong (current value vs correct value)
- Step-by-step fix instructions specific to that platform
- Estimated time to fix

**Gap list** — high-value directories where NO listing exists:
For each missing directory, provide:
- Platform name and signup/claim URL
- Why it matters for this business type
- Step-by-step creation instructions
- Estimated time to create

**Monthly citation maintenance checklist:**
- [ ] Check all listings for accuracy (quarterly)
- [ ] Respond to new reviews on all platforms (weekly)
- [ ] Update photos on all profiles (quarterly)
- [ ] Check for and merge duplicate listings (monthly)
- [ ] Update business hours for holidays (as needed)
- [ ] Refresh business descriptions (quarterly)

**Success criteria:** All inconsistencies flagged with fix instructions, all gaps identified with creation instructions.

### Phase 5: Write Reports

Write two files:

1. **`./reports/15-citation-audit.md`** — Full report following the standard report format, including the consistency matrix, freshness assessment, and gap analysis
2. **`./deliverables/citation-tracker.md`** — Actionable tracker with:
   - Master NAP reference (the correct values)
   - Platform-by-platform status (OK / needs fix / needs creation)
   - Fix instructions for each issue
   - Maintenance calendar
   - Monthly checklist

**Success criteria:** Both files written with complete content.

### Phase 6: Update Progress

Update context JSON:
```
progress["15_citation_audit"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/15-citation-audit.md"}
```

Write updated config back to `~/.claude/config/local-seo/{slug}.json` with refreshed `updated_at`.

---

## Sub-Command: search-intent (Prompt 16)

**Goal:** Map all relevant keywords to buyer journey stages for content prioritization.

### Phase 1: Keyword Discovery

Using `config.business.services` and `config.business.service_areas`, run WebSearch queries to discover keywords at each buyer journey stage.

**Stage 1 — Problem-Unaware (symptom-based queries):**
WebSearch for common symptoms related to the business's services:
- `"{service} symptoms" common problems`
- `"why is my" {related object} {symptom}` (e.g., "why is my AC making noise")
- `"signs you need" {service}`
- People Also Ask harvesting: WebSearch `{common problem} {city}` and note PAA questions

**Stage 2 — Problem-Aware (research queries):**
- `"how to fix" {common problem}`
- `"how to choose" {service provider type}`
- `"{service} cost" {city}` / `"how much does {service} cost"`
- `"{service} vs {alternative}"` comparison queries
- `"do I need" {service}`

**Stage 3 — Solution-Aware (comparison queries):**
- `"best {service provider}" {city}`
- `"{service provider} reviews" {city}`
- `"{competitor name} vs" {city}`
- `"{service} companies" {city} comparison`
- `"questions to ask" {service provider}`

**Stage 4 — Ready-to-Hire (buyer queries):**
- `"{service}" {city}` and `"{service} near me"`
- `"emergency {service}" {city}`
- `"{service}" {city} "free estimate"`
- `"{service provider} near me open now"`
- `"hire {service provider}" {city}`

Run these across the top 3 services and top 3 service areas from the config.

**Success criteria:** At least 40 keywords discovered across all 4 stages.

### Phase 2: Categorize and Analyze

For each stage, compile:

- **Total keywords found**
- **Estimated search volume range** — based on WebSearch result counts and autocomplete prevalence: HIGH (shows in autocomplete, many results), MEDIUM (some results), LOW (few results)
- **Difficulty assessment** — based on who ranks on page 1: HIGH (national brands, major sites), MEDIUM (mix of local and national), LOW (mostly local businesses)
- **Top 10 keywords** — ranked by estimated volume and relevance

Create a summary table:

```
Stage              | Keywords | Avg Volume | Avg Difficulty | Content Type
-------------------|----------|------------|----------------|------------------
1. Problem-Unaware | 15       | Medium     | Low            | Blog / awareness
2. Problem-Aware   | 20       | Medium     | Medium         | How-to / guides
3. Solution-Aware  | 12       | Medium     | Medium         | Comparison pages
4. Ready-to-Hire   | 18       | High       | High           | Service pages
```

**Success criteria:** All keywords categorized with volume and difficulty estimates.

### Phase 3: Strategy by Stage

Map each stage to the appropriate content and page strategy:

**Stage 4 (Ready-to-Hire) — PRIORITIZE FIRST:**
- These are money keywords. Map to existing service pages and GBP optimization.
- For each keyword: which page should rank, what changes are needed, on-page optimization specifics.

**Stage 3 (Solution-Aware):**
- Map to comparison pages, FAQ pages, and "why choose us" content.
- For each keyword: page type needed, key content elements, internal linking strategy.

**Stage 2 (Problem-Aware):**
- Map to educational blog content that funnels readers to service pages.
- For each keyword: blog post title, target word count, CTA to service page, internal links.

**Stage 1 (Problem-Unaware):**
- Map to problem-identification content that builds early trust and brand awareness.
- For each keyword: content format (blog, video script, social post), awareness-building angle.

**Success criteria:** Every keyword mapped to a specific content type and page strategy.

### Phase 4: 2026 Enhancement — Voice Search Mapping

Over 50% of local searches now happen via voice. Flag keywords that match voice search patterns:

**Voice search indicators:**
- Question format ("how do I...", "what is the best...", "where can I find...")
- Conversational length (4-7 words)
- Natural language phrasing (not keyword-stuffed)
- Location-implicit ("near me", "around here", "close by")

For each voice-friendly keyword:
- Mark it with a voice search flag
- Recommend FAQ schema markup for the corresponding page
- Recommend Position Zero (featured snippet) targeting strategy
- Suggest the exact question-and-answer format to use

Create a voice search priority list: top 10 voice-friendly keywords with the highest conversion potential.

**Success criteria:** Voice-friendly keywords flagged, FAQ schema and Position Zero recommendations provided.

### Phase 5: Priority Recommendations

Identify the **top 5 Stage 4 keywords** to rank for within 90 days.

For each, provide an exact action plan:
- **Target keyword**
- **Current ranking** (if known from config, otherwise "not ranking")
- **Target page** — which existing page to optimize (or recommend creating)
- **On-page changes** — title tag, H1, meta description, content additions, internal links
- **Off-page actions** — GBP optimization, citation consistency, review keywords
- **Timeline** — week-by-week milestones over 90 days
- **Expected outcome** — realistic ranking improvement estimate

**Success criteria:** 5 keywords with complete 90-day action plans.

### Phase 6: Write Reports

Write two files:

1. **`./reports/16-search-intent-map.md`** — Full report following the standard report format, including all keyword data, stage analysis, voice search findings, and priority recommendations
2. **`./deliverables/intent-map.md`** — Clean deliverable with:
   - Keywords organized by stage (table format)
   - Content mapping (keyword -> page -> content type)
   - Voice search priority list
   - 90-day action plan for top 5 keywords

**Success criteria:** Both files written with complete content.

### Phase 7: Update Progress

Update context JSON:
```
progress["16_search_intent"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/16-search-intent-map.md"}
```

Write updated config back to `~/.claude/config/local-seo/{slug}.json` with refreshed `updated_at`.

---

## Sub-Command: spam-audit

**Goal:** Identify fake or policy-violating competitor GBP listings and build a report for Google spam submissions. This is ethical competitive defense — only report objectively fake profiles.

### Phase 1: Competitor Listing Analysis

For each competitor in `config.competitors`, plus any businesses found in the map pack for target keywords:

1. **WebSearch** `"{keyword}" {city}` for each target keyword and note all businesses in the map pack
2. **WebSearch** each business name to check for legitimacy signals
3. For each listing, check for these spam indicators:

**Red flags (strong spam signals):**
- Business name contains keyword stuffing (e.g., "Best Plumber Las Vegas - 24/7 Emergency Plumbing Service" instead of "Smith Plumbing")
- Address is a residential home being used as a storefront listing (check via WebSearch/Google Maps)
- Address is a virtual office, UPS Store, or mailbox service (violates Google TOS for service-area businesses)
- Multiple listings for the same business at different addresses
- Business has reviews but no website, no social presence, and no real-world footprint
- Reviews appear fake (all posted within short timeframe, generic language, reviewer profiles with no other reviews)

**Yellow flags (investigate further):**
- Business name includes city name unnecessarily
- Hours show 24/7 but industry typically doesn't operate 24/7
- Photos are stock images or stolen from other businesses
- Service area claims a radius larger than reasonable

**Success criteria:** All map pack competitors analyzed for spam indicators.

### Phase 2: Evidence Documentation

For each suspected fake listing:
- Screenshot URLs (WebSearch result showing the listing)
- Specific policy violation (reference Google Business Profile guidelines)
- Evidence: what specifically is wrong (keyword-stuffed name, fake address, etc.)
- Severity: HIGH (clearly fake/violating) / MEDIUM (likely violating) / LOW (borderline)

Only include listings with HIGH or MEDIUM severity — do NOT report legitimate competitors just because they rank above you.

### Phase 3: Report Templates

For each HIGH/MEDIUM severity listing, create a Google spam report:
- Business name
- GBP URL (or search query to find it)
- Specific violation type (keyword-stuffed name, fake address, fake reviews, etc.)
- Evidence summary (2-3 sentences)
- Google's reporting URL: `https://support.google.com/business/contact/business_redressal_form`

### Phase 4: Ethical Guidelines

Include in the report:
- **DO report:** Keyword-stuffed names, fake addresses, lead gen spam, review manipulation
- **DO NOT report:** Legitimate competitors who just rank higher, businesses using legal marketing tactics, competitors with more reviews than you
- **Remember:** Filing false spam reports can result in YOUR account being flagged

### Phase 5: Write Reports

Write two files:

1. **`./reports/spam-audit.md`** — Full analysis with evidence and recommendations
2. **`./deliverables/spam-report-log.md`** — Tracker with:
   - Each suspected listing
   - Evidence and violation type
   - Report status (not yet reported / reported on DATE / resolved)
   - Follow-up dates

### Phase 6: Update Progress

Update context JSON — note this is a bonus audit, not one of the numbered 20:
```
progress["spam_audit"] = {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/spam-audit.md"}
```

---

## Report Format

All reports in `./reports/` follow this structure:

1. **Executive Summary** — 3-5 sentences covering what was found and the single most important takeaway
2. **Data Gathered** — what sources were checked, how many data points collected, any limitations
3. **Analysis** — detailed findings with comparison tables where applicable
4. **2026 Enhancements** — modern ranking factors and recommendations specific to the current search landscape
5. **Action Items** — prioritized list, each tagged:
   - Priority: **HIGH** / **MEDIUM** / **LOW**
   - Estimated time to complete
   - Estimated time to see results
6. **Next Steps** — which local-seo skill to run next and why

---

## Progress Tracking

After each sub-command completes successfully, update the business context JSON:

1. Read the config from `~/.claude/config/local-seo/{slug}.json`
2. Update the relevant progress key:
   - `backlink-audit` updates `progress["14_backlink_audit"]`
   - `citation-audit` updates `progress["15_citation_audit"]`
   - `search-intent` updates `progress["16_search_intent"]`
3. Set `status` to `"completed"`, `last_run` to today's date (YYYY-MM-DD), `output` to the report path
4. Update `updated_at` to current ISO 8601 timestamp in UTC
5. Write the config back

---

## Error Recovery

| Sub-command | Failure | Recovery |
|-------------|---------|----------|
| backlink-audit | Limited WebSearch data for links | Focus on directory/association discovery instead of direct link queries. Suggest user run a free backlink checker (Ahrefs free tool, Ubersuggest) and paste results into `./data/` |
| backlink-audit | Competitor has no web presence | Skip that competitor, note in report, focus on remaining competitors and general industry link sources |
| backlink-audit | Fewer than 15 opportunities found | Lower the target to what was found, supplement with general local link building best practices |
| citation-audit | Can't verify NAP on a platform | Mark platform as "unverified" in the tracker, provide direct URL for manual check, continue with remaining platforms |
| citation-audit | Platform blocks WebFetch | Note in report, provide the search URL so user can check manually, do not retry excessively |
| citation-audit | Business has no existing citations | Treat entire audit as a gap analysis — every platform becomes a "needs creation" entry |
| search-intent | Too many keywords discovered | Focus on top 3 services x top 3 cities from config, cap at 20 keywords per stage |
| search-intent | Very niche industry with few searches | Broaden to state-level and adjacent service terms, note low volume in report, recommend long-tail strategy |
| search-intent | WebSearch rate limiting | Space out searches, prioritize Stage 4 (money keywords) first, fill remaining stages with best available data |
| spam-audit | No obvious spam found in local market | Good news — report clean market, note as positive finding. Focus efforts elsewhere. |
| spam-audit | Uncertain whether a listing is fake | Do NOT report. Only report clear policy violations with evidence. Note uncertain cases for monitoring. |
| Any | Context JSON missing or corrupt | Stop and tell user to run `/local-seo-context setup` or `/local-seo-context status` to diagnose |
| Any | Output directory not writable | Run `mkdir -p ./reports ./deliverables` and retry |

## Important Notes

- This skill reads from the business context but never modifies business data fields — it only updates progress tracking.
- All WebSearch queries should include the city name from `config.business.address` for local relevance.
- Outreach emails in the backlink audit should be personalized using the business name, owner details, and specific value propositions from the config.
- The citation audit canonical NAP values always come from the config — those are the "correct" values to compare against.
- Search intent keywords should map back to `config.seo_goals.target_keywords` where possible, extending beyond them for full coverage.
- Timestamps use ISO 8601 format in UTC.
