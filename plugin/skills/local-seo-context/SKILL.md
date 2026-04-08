---
name: local-seo-context
description: |
  Load or create business context for local SEO work. This is the foundation skill —
  all other local-seo skills read from the context file this creates. Supports setup,
  switch, update, and status commands.
user-invocable: true
argument-hint: "[setup|switch <slug>|update|status] (default: load active context)"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO Business Context

**Command:** $ARGUMENTS

Foundation skill for local SEO pipeline. Creates and manages business context files that all other local-seo skills depend on.

## Argument Parsing

Parse `$ARGUMENTS` for the subcommand:

- **`setup`** or **`setup <JSON>`** = interactive (or bulk) setup for a new business
- **`switch <slug>`** = switch active business to specified slug
- **`update`** = update fields in the active business context
- **`status`** = show progress table across all 20 audit prompts
- **(empty / no args)** = load and display active business context summary

If $ARGUMENTS is empty or missing, default to **load mode**.

---

## Subcommand: Load (default, no args)

### Phase 1: Find Active Context

1. Read `./local-seo-context.json` in the current project directory
2. Extract the `config_path` field pointing to the full config JSON
3. Read the full config from `~/.claude/config/local-seo/{slug}.json`

**Success criteria:** Config file exists and parses as valid JSON.

### Phase 2: Display Summary

Display a formatted summary:

```
Local SEO Context: {business.name}
====================================

Business:    {name} | {phone} | {website}
Address:     {address}
GBP:         {gbp_url}
Services:    {primary} + {N secondary}
Areas:       {comma-separated service areas}
Schema:      {schema_type}

SEO Goals:
  Target KWs:   {comma-separated}
  Biggest Gap:   {biggest_problem}

Current Standings:
  Reviews:  {total} ({rating} avg, {monthly_new}/mo)
  GBP:      {gbp_monthly_views} views/mo
  Traffic:  {monthly_website_traffic}/mo
  Map Pack: {map_pack_status}

Competitors: {count}
Progress:    {completed}/{total} prompts done

Last updated: {updated_at}
```

**Success criteria:** Summary displayed with no missing critical fields.

---

## Subcommand: setup

Setup accepts either a **URL**, a **business name**, or **no arguments** (interactive mode).

- `setup https://acmeplumbing.com` — auto-extract everything from the website first, then ask only for what's missing
- `setup "Acme Plumbing Henderson NV"` — search for the business, then auto-extract from website
- `setup` — fully interactive (ask for everything)

### Phase 0: Auto-Extract from Website (if URL or business name provided)

If a URL or business name is provided, extract as much as possible automatically before asking the user anything.

**Step 0a — Resolve website URL:**
- If URL provided: use it directly
- If business name provided: WebSearch `"{business name}"` to find the website URL

**Step 0b — WebFetch the website homepage.** Extract:
- **Business name** from `<title>` tag, `<meta property="og:site_name">`, or header/logo text
- **Phone number** from `<a href="tel:">` tags, footer, contact page
- **Address** from structured data (JSON-LD), footer, contact page, `<meta>` tags
- **Services** from navigation menu items, service page links, H2 headings
- **Service areas** from footer ("Serving Henderson, Las Vegas, North Las Vegas"), area pages, or service area schema

**Step 0c — WebFetch the contact/about page** (try `/contact`, `/about`, `/about-us`). Extract:
- Address confirmation
- Additional phone numbers
- Years in business ("serving since 2014", "20+ years experience")
- Team size clues ("our team of 8 technicians")

**Step 0d — Check for existing schema markup.** Parse any JSON-LD on the homepage:
- `@type` tells us the schema type directly
- `areaServed` tells us service areas
- `hasOfferCatalog` tells us services
- `address`, `telephone`, `name` are direct extractions

**Step 0e — WebSearch for GBP listing:** `"{business name}" "{city}" site:google.com/maps`
- Extract GBP URL, review count, rating, categories

**Step 0f — WebSearch for competitors:** `"{primary service}" "{city}"` and note the top 3 businesses that aren't ours. For each:
- Name, website URL
- WebSearch for their GBP URL
- Note approximate review count from search snippets

**Step 0g — Auto-generate target keywords:** Based on extracted services and cities:
- `"{primary service} {main city}"` (e.g., "plumber henderson nv")
- `"emergency {primary service} {main city}"`
- `"best {primary service} {main city}"`
- `"{secondary service} {main city}"` for each secondary service
- `"{primary service} near me"`

**Step 0h — Present all auto-discovered data to the user:**
```
Auto-discovered from website:
==============================
Business:     Acme Plumbing
Address:      123 Main St, Henderson, NV 89011
Phone:        (702) 555-0123
Website:      https://acmeplumbing.com
GBP:          https://google.com/maps/place/...
Reviews:      47 reviews, 4.6 stars
Schema type:  Plumber

Services found:    Residential Plumbing, Water Heater Repair, Drain Cleaning, Sewer Line
Areas found:       Henderson, Las Vegas, North Las Vegas, Boulder City

Competitors found:
  1. Best Plumbing Co (152 reviews, 4.8 stars) - bestplumbingco.com
  2. Quick Fix Plumbing (89 reviews, 4.5 stars) - quickfixplumbing.com
  3. Pro Plumbers NV (67 reviews, 4.3 stars) - proplumbersnv.com

Auto-generated target keywords:
  1. plumber henderson nv
  2. emergency plumber henderson
  3. water heater repair henderson
  4. drain cleaning henderson
  5. plumber near me henderson

Is this correct? What should I adjust?
```

The user only needs to confirm or correct — they don't need to type anything that was auto-discovered.

**Success criteria:** At least business name, website, and city extracted automatically.

### Phase 1: Gather Remaining Information

After auto-extraction, ask ONLY for fields that couldn't be detected:
- Anything marked as "not found" from Phase 0
- **Target customer description** (can't be extracted from website)
- **Average job value** (can't be extracted)
- **Biggest SEO problem** (can't be extracted — ask: "What's your #1 SEO frustration right now?")
- **Competitor context** (if auto-detected competitors look wrong, let user correct)

If running in fully interactive mode (no URL provided), collect all fields:
- Business name, address, phone, website URL
- GBP URL (optional; will search)
- Years in business, team size
- Primary and secondary services
- Service areas (up to 8)
- Target customer, average job value

**Success criteria:** All critical fields populated (auto-extracted + user-provided).

### Phase 2: SEO Goals

If target keywords were auto-generated in Phase 0, present them and ask:
- "Are these the right keywords, or would you change any?"
- "What keywords do you currently rank for?" (can be empty)
- "What keywords should you rank for but don't?" (can be empty)

If no auto-generation was possible, collect:
- Top 5 target keywords (at least 3)
- Keywords currently ranking for
- Keywords should rank for but don't

**Success criteria:** At least 3 target keywords confirmed.

### Phase 3: Current Standings

If review data was found from GBP search in Phase 0, present it:
- "Found 47 reviews at 4.6 stars — is that current?"

Ask only for what wasn't found:
- Monthly new reviews (approximate)
- GBP monthly views (if known)
- Monthly website traffic (if known)
- Map pack status
- Previous SEO work done

**Success criteria:** At minimum, review count and rating confirmed or collected.

### Phase 4: Competitors

If competitors were auto-discovered in Phase 0, present them:
- "Found these competitors in the map pack — correct?"

Ask only for corrections or additions. User can provide 0-3 competitors.

**Success criteria:** Competitor data confirmed (even if empty array).

### Phase 5: Validation and Auto-Discovery

Run any remaining discovery steps not completed in Phase 0:

1. **Verify GBP exists:** If GBP URL was not provided, use WebSearch to search for `"{business name}" "{city}" site:google.com/maps` and extract the GBP URL. If found, confirm with user.

2. **Find competitor GBP URLs:** For any competitor missing a GBP URL, use WebSearch to search `"{competitor name}" "{city}" site:google.com/maps`. Populate if found.

3. **Determine schema.org type:** Based on the primary service, select the most appropriate schema.org LocalBusiness subtype. Common mappings:
   - Plumbing -> Plumber
   - HVAC -> HVACBusiness
   - Electrical -> Electrician
   - Dental -> Dentist
   - Legal -> Attorney
   - Roofing -> RoofingContractor
   - Auto repair -> AutoRepair
   - Restaurant -> Restaurant
   - Hair/Beauty -> BeautySalon
   - General contractor -> GeneralContractor
   - Other -> LocalBusiness (with note to refine)

   Confirm the selected type with the user.

4. **Check Cloudflare Pages integration:** Search `~/.claude/config/cf-sites.json` (or similar) for the business website domain. If found, note the project name for future deployments.

5. **Initialize progress tracker:** Create entries for all 20 prompts with status `pending`.

**Success criteria:** GBP URL resolved (or marked unknown), schema type selected, progress tracker initialized.

### Phase 6: Save

1. **Generate slug** from business name: lowercase, replace spaces/special chars with hyphens, strip trailing hyphens (e.g., "Acme Plumbing & Heating" -> `acme-plumbing-heating`)

2. **Ensure config directory exists:**
   ```bash
   mkdir -p ~/.claude/config/local-seo
   ```

3. **Write full config** to `~/.claude/config/local-seo/{slug}.json` with this schema:

```json
{
  "business": {
    "name": "Acme Plumbing",
    "slug": "acme-plumbing",
    "address": "123 Main St, Henderson, NV 89011",
    "phone": "(702) 555-0123",
    "website": "https://acmeplumbing.com",
    "gbp_url": "https://www.google.com/maps/place/...",
    "years_in_business": 12,
    "team_size": 8,
    "services": {
      "primary": "Residential Plumbing",
      "secondary": ["Water Heater Repair", "Drain Cleaning", "Sewer Line"]
    },
    "service_areas": ["Henderson", "Las Vegas", "North Las Vegas", "Boulder City"],
    "target_customer": "Homeowners 35-65 in Henderson NV",
    "average_job_value": 450,
    "schema_type": "Plumber"
  },
  "seo_goals": {
    "target_keywords": ["plumber henderson nv", "emergency plumber henderson", "water heater repair henderson", "drain cleaning henderson", "plumber near me henderson"],
    "currently_ranking": ["acme plumbing henderson"],
    "should_rank_for": ["best plumber henderson", "24 hour plumber las vegas"],
    "biggest_problem": "Not showing in map pack for primary keywords"
  },
  "current_standings": {
    "google_reviews": {
      "total": 47,
      "rating": 4.6,
      "monthly_new": 3
    },
    "gbp_monthly_views": 1200,
    "monthly_website_traffic": 800,
    "map_pack_status": "sometimes"
  },
  "competitors": [
    {
      "name": "Best Plumbing Co",
      "gbp_url": "https://www.google.com/maps/place/...",
      "website": "https://bestplumbingco.com",
      "why_beating": "150+ reviews, been around 20 years, ranks #1 for all Henderson plumbing terms"
    }
  ],
  "previous_seo_work": "Had an SEO agency for 6 months in 2024, they did some blog posts but no local optimization",
  "work_preferences": {
    "priority": "quick_wins",
    "output_format": "spreadsheet_when_comparing",
    "honesty": true
  },
  "data_sources": {
    "gsc_export_dir": "./data/gsc-exports/",
    "project_dir": null
  },
  "progress": {
    "01_category_audit": { "status": "pending", "last_run": null, "output": null },
    "02_gbp_audit": { "status": "pending", "last_run": null, "output": null },
    "03_citation_audit": { "status": "pending", "last_run": null, "output": null },
    "04_review_strategy": { "status": "pending", "last_run": null, "output": null },
    "05_website_local_seo": { "status": "pending", "last_run": null, "output": null },
    "06_service_area_pages": { "status": "pending", "last_run": null, "output": null },
    "07_schema_markup": { "status": "pending", "last_run": null, "output": null },
    "08_content_strategy": { "status": "pending", "last_run": null, "output": null },
    "09_competitor_analysis": { "status": "pending", "last_run": null, "output": null },
    "10_link_building": { "status": "pending", "last_run": null, "output": null },
    "11_technical_seo": { "status": "pending", "last_run": null, "output": null },
    "12_photo_strategy": { "status": "pending", "last_run": null, "output": null },
    "13_qa_optimization": { "status": "pending", "last_run": null, "output": null },
    "14_post_strategy": { "status": "pending", "last_run": null, "output": null },
    "15_tracking_setup": { "status": "pending", "last_run": null, "output": null },
    "16_social_profiles": { "status": "pending", "last_run": null, "output": null },
    "17_reputation_management": { "status": "pending", "last_run": null, "output": null },
    "18_conversion_optimization": { "status": "pending", "last_run": null, "output": null },
    "19_quarterly_audit": { "status": "pending", "last_run": null, "output": null },
    "20_monthly_report": { "status": "pending", "last_run": null, "output": null }
  },
  "pipeline": {
    "started": null,
    "current_week": 0,
    "schedule": {}
  },
  "created_at": "2026-04-08T12:00:00Z",
  "updated_at": "2026-04-08T12:00:00Z"
}
```

4. **Write active pointer** to `./local-seo-context.json`:

```json
{
  "active_slug": "acme-plumbing",
  "config_path": "~/.claude/config/local-seo/acme-plumbing.json"
}
```

5. **Create project directories:**
   ```bash
   mkdir -p ./reports ./deliverables ./data/gsc-exports
   ```

6. **Create GSC export README** at `./data/gsc-exports/README.md`:

```markdown
# Google Search Console Export Instructions

Export these reports from Google Search Console and save CSV files here.

## Required Exports

### 1. Performance > Search Results (Last 16 months)
- Go to GSC > Performance > Search results
- Set date range: Last 16 months
- Click Export > CSV
- Save as: `search-results-YYYY-MM-DD.csv`

### 2. Performance > Search Results by Page
- Same report, but click "Pages" tab
- Export CSV
- Save as: `pages-YYYY-MM-DD.csv`

### 3. Performance > Search Results by Query
- Same report, but click "Queries" tab
- Export CSV
- Save as: `queries-YYYY-MM-DD.csv`

### 4. Performance > Discover (if available)
- Go to GSC > Performance > Discover
- Export CSV
- Save as: `discover-YYYY-MM-DD.csv`

### 5. Links > External Links (Top linked pages)
- Go to GSC > Links > External links > Top linked pages
- Export CSV
- Save as: `external-links-YYYY-MM-DD.csv`

### 6. Links > Internal Links
- Go to GSC > Links > Internal links > Top linked pages
- Export CSV
- Save as: `internal-links-YYYY-MM-DD.csv`

## Tips
- Use the EXACT filenames above so scripts can auto-detect them
- Re-export monthly for trend tracking
- The date in the filename is when you exported, not the date range
```

7. **Confirm to user** with a summary of what was created and next steps.

**Success criteria:** Config file written, pointer file written, directories created, GSC README created.

---

## Subcommand: switch

### Phase 1: Validate Target

1. Check that `~/.claude/config/local-seo/{slug}.json` exists for the provided slug
2. If not found, list available configs:
   ```bash
   ls ~/.claude/config/local-seo/*.json
   ```
3. If slug is ambiguous, show options and ask user to clarify

**Success criteria:** Target config file exists.

### Phase 2: Update Pointer

1. Write `./local-seo-context.json` with the new slug:
   ```json
   {
     "active_slug": "{slug}",
     "config_path": "~/.claude/config/local-seo/{slug}.json"
   }
   ```
2. Read and display the business name from the config as confirmation

**Success criteria:** Pointer updated, business name displayed.

---

## Subcommand: update

### Phase 1: Load Current Context

1. Read `./local-seo-context.json` to find active config
2. Read the full config JSON
3. Display current values summary

**Success criteria:** Config loaded successfully.

### Phase 2: Collect Changes

Ask the user what they want to update. Accept:
- Specific field paths (e.g., "business.phone", "seo_goals.target_keywords")
- Section names (e.g., "competitors", "current_standings")
- Free-form descriptions (e.g., "add a new service area", "update review count")

Interpret the request and show the current value -> proposed new value for confirmation.

**Success criteria:** User confirms changes.

### Phase 3: Save

1. Apply changes to the config JSON
2. Update `updated_at` to current ISO timestamp
3. Write back to `~/.claude/config/local-seo/{slug}.json`
4. Display what changed

**Success criteria:** Config updated, timestamp refreshed.

---

## Subcommand: status

### Phase 1: Load Context

1. Read `./local-seo-context.json` to find active config
2. Read the full config JSON

**Success criteria:** Config loaded.

### Phase 2: Display Progress Table

Display a formatted table of all 20 prompts:

```
Local SEO Progress: {business.name}
======================================

 #  | Prompt                    | Status      | Last Run   | Output
----|---------------------------|-------------|------------|---------------------------
 01 | Category Audit            | completed   | 2026-04-01 | reports/01-category-audit.md
 02 | GBP Audit                 | in_progress | 2026-04-03 | reports/02-gbp-audit.md
 03 | Citation Audit            | pending     | -          | -
 04 | Review Strategy           | pending     | -          | -
 05 | Website Local SEO         | pending     | -          | -
 06 | Service Area Pages        | pending     | -          | -
 07 | Schema Markup             | pending     | -          | -
 08 | Content Strategy          | pending     | -          | -
 09 | Competitor Analysis       | pending     | -          | -
 10 | Link Building             | pending     | -          | -
 11 | Technical SEO             | pending     | -          | -
 12 | Photo Strategy            | pending     | -          | -
 13 | Q&A Optimization          | pending     | -          | -
 14 | Post Strategy             | pending     | -          | -
 15 | Tracking Setup            | pending     | -          | -
 16 | Social Profiles           | pending     | -          | -
 17 | Reputation Management     | pending     | -          | -
 18 | Conversion Optimization   | pending     | -          | -
 19 | Quarterly Audit           | pending     | -          | -
 20 | Monthly Report            | pending     | -          | -
----|---------------------------|-------------|------------|---------------------------
Progress: 1/20 completed (5%)
```

Also show:
- Pipeline start date (if set)
- Current week number
- Next recommended prompt to run

**Success criteria:** Table displayed with accurate status for all 20 prompts.

---

## Error Recovery

| Situation | Recovery |
|-----------|----------|
| `local-seo-context.json` not found | Prompt user to run `setup` first |
| Config file missing at path | Check `~/.claude/config/local-seo/` for available configs, suggest `switch` |
| GBP URL not found via search | Set to `null`, note it needs manual entry, continue setup |
| WebSearch fails | Skip auto-discovery step, note fields that need manual population |
| Invalid JSON in config | Show parse error, attempt to read raw file and identify issue |
| Slug collision on setup | Append number suffix (e.g., `acme-plumbing-2`) |
| No `cf-sites.json` found | Skip Cloudflare check, set `project_dir` to null |
| User provides partial info | Accept what's given, set missing optional fields to null, note gaps |

## Important Notes

- This skill NEVER runs other local-seo skills directly. It only manages context.
- All 20 progress keys must match exactly: `01_category_audit` through `20_monthly_report`.
- The `work_preferences.honesty` flag means: always give frank assessments, don't sugarcoat rankings or competitive position.
- The `work_preferences.priority` of `quick_wins` means: when recommending next steps, prioritize changes with the highest impact-to-effort ratio.
- When loading context (default command), always read from the full config file, not just the pointer.
- Timestamps use ISO 8601 format in UTC.
