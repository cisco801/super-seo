---
description: Quick-start local SEO optimization for any service-area business website
argument-hint: "[website URL or business name] [--audit] [--full]"
---

# /seo -- Local SEO Quick Start

Point at any service-area business website and start optimizing.

## Invocation

```
/seo https://acmeplumbing.com
/seo "Acme Plumbing Henderson NV"
/seo --audit https://acmeplumbing.com
/seo --full https://acmeplumbing.com
```

## Argument Parsing

Parse `$ARGUMENTS` for:
- **URL** = a website URL (starts with http or contains `.com/.net/.org/etc.`) -- target website to optimize
- **Quoted text** = business name + location if no URL given
- **`--audit`** = run a quick technical + GBP audit only (no full pipeline)
- **`--full`** = start the full 12-week pipeline immediately after setup

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
3. **Present findings** to the user:
   ```
   Found: Acme Plumbing
   Address: 123 Main St, Henderson, NV 89011
   Phone: (702) 555-0123
   Website: https://acmeplumbing.com
   GBP: https://google.com/maps/place/...
   Services detected: Residential Plumbing, Water Heater Repair, Drain Cleaning
   Areas detected: Henderson, Las Vegas, North Las Vegas
   Schema type: Plumber

   Is this correct? What should I adjust?
   ```
4. **Ask for missing data** that couldn't be auto-detected:
   - Target keywords (top 3-5)
   - Competitors (top 1-3)
   - Biggest SEO problem
   - Review count and rating (if not found)
5. **Save context** using the local-seo-context setup flow
   - Write config to `~/.claude/config/local-seo/{slug}.json`
   - Write pointer to `./local-seo-context.json`
   - Create `./reports/`, `./deliverables/`, `./data/gsc-exports/` directories

### Step 3: Choose Mode

Based on flags:

**If `--audit` (quick audit mode):**
Run these three audits and produce a summary:
1. `/local-seo-website technical-audit` -- Core Web Vitals, mobile, crawlability
2. `/local-seo-gbp category-audit` -- GBP category gaps vs competitors
3. `/local-seo-authority citation-audit` -- NAP consistency across directories

After all three complete, output a combined quick audit summary:
```
Quick SEO Audit: {Business Name}
================================

Technical Health: [A/B/C/D/F]
  - [top 3 technical findings]

GBP Categories: [X missing categories found]
  - [top 3 category recommendations]

Citation Consistency: [X inconsistencies found]
  - [top 3 citation fixes needed]

Top 3 Quick Wins:
  1. [highest impact action]
  2. [second highest]
  3. [third highest]

Run /local-seo-pipeline start for the full 12-week optimization plan.
```

**If `--full` (full pipeline mode):**
Run `/local-seo-pipeline start` to begin the 12-week plan.

**If no flags (default):**
Show what's available and recommend next steps:
```
Local SEO Ready: {Business Name}
=================================

Context loaded. Here's what you can do:

Quick audit:     /seo --audit
Full pipeline:   /local-seo-pipeline start
Specific audits: /local-seo-gbp category-audit
                 /local-seo-website technical-audit
                 /local-seo-authority citation-audit
Check progress:  /local-seo-pipeline status

Recommended: Start with /seo --audit for a quick health check,
then /local-seo-pipeline start for the full 12-week plan.
```

### Step 4: Next Steps

After any mode completes, always remind the user:
- Reports are in `./reports/`
- Deliverables are in `./deliverables/`
- Progress is tracked automatically
- Run `/local-seo-pipeline status` to see overall progress
- Run `/local-seo-pipeline resume` to continue where they left off
