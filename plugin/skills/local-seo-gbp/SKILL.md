---
name: local-seo-gbp
description: |
  Google Business Profile optimization covering 10 audit types: category audit, attributes audit,
  competitor reviews, review strategy, posts strategy, services optimization, description
  optimization, photo audit, video strategy, and direct booking setup. Run all 10 or target
  a specific audit by name.
user-invocable: true
argument-hint: "[all | category-audit | attributes-audit | competitor-reviews | review-strategy | posts-strategy | services-optimization | description-optimization | photo-audit | video-strategy | direct-booking]"
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch, Agent
---

# Local SEO: Google Business Profile Optimization

**Request:** $ARGUMENTS

10 GBP audit/optimization sub-commands covering prompts 1-10 of the local SEO system.

## Argument Parsing

Parse `$ARGUMENTS` for:
- **No arguments or `all`** = run all 10 audits sequentially (1 through 10)
- **`category-audit`** = run prompt 1 only
- **`attributes-audit`** = run prompt 2 only
- **`competitor-reviews`** = run prompt 3 only
- **`review-strategy`** = run prompt 4 only
- **`posts-strategy`** = run prompt 5 only
- **`services-optimization`** = run prompt 6 only
- **`description-optimization`** = run prompt 7 only
- **`photo-audit`** = run prompt 8 only
- **`video-strategy`** = run prompt 9 only
- **`direct-booking`** = run prompt 10 only

## Phase 0: Prerequisite (Every Run)

1. Read `./local-seo-context.json` to get the active business slug
2. Read `~/.claude/config/local-seo/{slug}.json` to load full business context (business name, city, service areas, target keywords, competitor names, services offered, current GBP URL)
3. If either file is missing or the slug is empty, stop and tell the user: "No business context found. Run `/local-seo-context setup` first."
4. Create `./reports/` and `./deliverables/` directories if they do not exist

### Site Type Gate

After loading business context, check `business.site_type`:
- If `local_service` or `professional_service`: proceed with all GBP audits
- If `saas_product`, `free_tool`, `ecommerce`, or `content_site`: Stop and tell the user:
  > "GBP optimization is designed for local service businesses. Your site ({site_type}) would benefit more from `/local-seo-website technical-audit` and `/local-seo-content entity-optimization` instead."
  > 
  > If you DO have a Google Business Profile listing and want to optimize it anyway, run: `/local-seo-gbp category-audit --force`
- If `unknown`: Ask the user if they have a Google Business Profile listing. If yes, proceed. If no, suggest alternatives.

**Override:** If the user passes `--force` after any sub-command name, skip the site type gate and run the audit anyway.

**Success criteria:** Business context loaded with at minimum: business name, city, top 3 target keywords, 3+ competitor names, list of services offered.

---

## Sub-Command 1: `category-audit` (Prompt 1)

**Goal:** Find GBP categories competitors use that the business is missing.

### Phase 1 - Data Gathering

- WebSearch `"[service] in [city]"` for each of the top 3 target keywords from context
- WebSearch each competitor name + city to find their GBP categories
- WebFetch competitor websites to cross-reference service offerings against listed categories

**Success criteria:** Category data gathered for at least 3 competitors.

### Phase 2 - Analysis

- Map which categories appear across ALL competitors vs. only some
- Identify categories every top competitor shares (non-negotiable, must-have)
- Identify categories only 1-2 competitors have (differentiation opportunities)
- Cross-reference against the business's actual services from context

**Success criteria:** Comparison table built with at least 5 categories mapped.

### Phase 3 - 2026 Enhancement: GBP Completeness Score

Score the business's GBP profile across 12 dimensions:

1. Categories
2. Attributes
3. Description
4. Services
5. Photos
6. Posts
7. Q&A
8. Products
9. Hours
10. Website link
11. Phone
12. Address

Report completeness percentage vs. competitors (estimate from available data).

**Success criteria:** Completeness score calculated for the business and at least 2 competitors.

### Output

Write `./reports/01-category-audit.md` containing:
- Competitor category comparison table
- Prioritized list of categories to add (with impact rating: HIGH/MEDIUM/LOW)
- GBP completeness score and competitor comparison
- Specific action items with estimated time to see results

---

## Sub-Command 2: `attributes-audit` (Prompt 2)

**Goal:** Find GBP attributes competitors have that the business is missing.

### Phase 1 - Data Gathering

WebSearch and WebFetch competitor GBP listings for attributes (veteran-owned, free estimates, 24/7, wheelchair-accessible, women-led, online appointments, etc.).

**Success criteria:** Attribute data gathered for at least 3 competitors.

### Phase 2 - Comparison

Compare attributes across all competitors, identify gaps between the business and competitors.

**Success criteria:** At least 8 attributes identified across competitors.

### Phase 3 - Categorization

Create three lists:
- **Table stakes:** All competitors have these (must add immediately)
- **Strong recommendations:** 2 out of 3 competitors have these (add within 1 week)
- **Differentiation:** Only 1 out of 3 has these (evaluate for fit)

**Success criteria:** Each list has at least 1 item.

### Phase 4 - Impact Rating

Rate each missing attribute on two dimensions:
- **Ranking impact:** How much this attribute affects local pack ranking (HIGH/MEDIUM/LOW)
- **CTR impact:** How much this attribute affects click-through rate from search (HIGH/MEDIUM/LOW)

**Success criteria:** Every missing attribute rated on both dimensions.

### Output

Write `./reports/02-attributes-audit.md` with standard report format plus the three categorized attribute lists.

---

## Sub-Command 3: `competitor-reviews` (Prompt 3)

**Goal:** Analyze competitor review velocity, themes, and language.

### Phase 1 - Data Gathering

WebSearch each competitor's GBP listing. Extract:
- Total review count
- Average star rating
- Recent review snippets (as many as WebSearch provides)

**Success criteria:** Review data gathered for at least 3 competitors.

### Phase 2 - Analysis

For each competitor, analyze:
- Total count and average rating
- Review velocity estimate (last 30/60/90 days from available WebSearch data)
- Most mentioned services, neighborhoods, and staff names
- Recurring complaints and negative themes
- Top keyword phrases customers use in reviews

**Success criteria:** At least 3 keyword themes identified per competitor.

### Phase 3 - 2026 Enhancement: Review Recency Weighting

The March 2026 core update weights review recency MORE than raw count. Calculate:
- **Effective review strength** = (estimated monthly velocity x 12) + (total count / 10)
- Compare this metric across all competitors and the business
- Flag if the business is losing on velocity even with a higher total count

**Success criteria:** Effective review strength calculated for the business and all competitors.

### Output

Write `./reports/03-competitor-reviews.md` with:
- Competitor review comparison table (count, rating, velocity, effective strength)
- Keyword frequency analysis
- Recommended review keywords for customers to naturally include

---

## Sub-Command 4: `review-strategy` (Prompt 4)

**Goal:** Build a complete review response template system.

### Phase 1 - Current State

WebSearch and WebFetch the business's current GBP listing. Analyze existing review responses for tone, length, keyword usage.

**Success criteria:** At least 5 existing responses analyzed (or note that none exist).

### Phase 2 - Competitor Patterns

Analyze how competitors respond to reviews. Note response rate, average response time, tone, keyword inclusion.

**Success criteria:** Response patterns documented for at least 2 competitors.

### Phase 3 - Template System

Build response templates. Each template must be 40-80 words, use natural voice, and include business keywords:

- **5-star reviews:** 3 variations (include service keyword + city keyword)
- **4-star reviews:** 3 variations (acknowledge feedback, reinforce positive)
- **3-star reviews:** 3 variations (empathize, offer resolution, invite return)
- **1-2 star reviews:** 3 variations (apologize, take offline, provide contact)

**Success criteria:** 12 templates total, each 40-80 words, each containing at least 1 target keyword.

### Phase 4 - 2026 Enhancement

Owner response rate is now a ranking factor per March 2026 update. Include in the strategy:
- Target: 100% response rate within 24 hours
- Weekly review monitoring checklist
- Escalation process for negative reviews

**Success criteria:** Response rate target and monitoring process documented.

### Output

Write `./reports/04-review-strategy.md` with standard report format.

Write `./deliverables/review-response-templates.md` with all 12 templates organized by star rating, ready for copy-paste use.

---

## Sub-Command 5: `posts-strategy` (Prompt 5)

**Goal:** Build an 8-week GBP posting calendar.

### Phase 1 - Competitor Analysis

WebSearch competitor GBP posts to analyze:
- Posting frequency
- Post types (updates, offers, events, products)
- Themes and topics
- Engagement patterns

**Success criteria:** Posting patterns documented for at least 2 competitors.

### Phase 2 - Gap Identification

Identify posting patterns and gaps. Note what competitors are NOT doing that represents an opportunity.

**Success criteria:** At least 3 gaps or opportunities identified.

### Phase 3 - Calendar Build

Build an 8-week calendar with 2-3 posts per week (16-24 total posts). Mix these content types:
- Seasonal service promotions
- Before/after project showcases
- Neighborhood-specific posts (mentioning each service area from context)
- Review highlights and customer stories
- Team spotlights
- Educational/problem-solving content

Each post entry must include:
- Week and day
- Post type
- At least one target keyword
- A clear CTA (call, visit, book, etc.)
- Image description (what photo to take or source)

**Success criteria:** Calendar has 16-24 posts, each service area mentioned at least once.

### Phase 4 - Full Copy

Write full copy for weeks 1-4 (100-150 words each, clear CTA, image description for what photo to take). Provide outlines only for weeks 5-8.

**Success criteria:** 8-12 posts with full copy, remaining posts with outlines.

### Output

Write `./reports/05-posts-strategy.md` with standard report format and competitor analysis.

Write `./deliverables/gbp-posts-calendar.md` with the full 8-week calendar and copy.

---

## Sub-Command 6: `services-optimization` (Prompt 6)

**Goal:** Optimize the GBP services section with keyword-rich descriptions.

### Phase 1 - Competitor Services

WebSearch and WebFetch competitor GBP listings and websites to catalog their services sections.

**Success criteria:** Services data gathered from at least 3 competitors.

### Phase 2 - Cross-Reference

Compare competitor services against the business's website services from context. Identify:
- Services competitors list that the business offers but hasn't added to GBP
- Services the business offers that no competitor lists (differentiation)
- Common service naming patterns in the industry

**Success criteria:** Complete service gap analysis.

### Phase 3 - Optimized Descriptions

Write optimized descriptions for ALL services the business offers:
- 2-3 sentences, 40-60 words each
- Natural keyword inclusion (not stuffed)
- At least one service area mention per description
- Specific benefit or outcome for the customer

**Success criteria:** Every service has a description within the 40-60 word range with at least 1 keyword.

### Output

Write `./reports/06-services-optimization.md` with all optimized service descriptions, ready for GBP entry.

---

## Sub-Command 7: `description-optimization` (Prompt 7)

**Goal:** Write 3 optimized GBP descriptions (750 characters each).

### Phase 1 - Competitor Descriptions

WebSearch and WebFetch competitor GBP descriptions. Analyze what top competitors mention: keywords, service areas, trust signals, CTAs.

**Success criteria:** At least 3 competitor descriptions analyzed.

### Phase 2 - Keyword Mapping

Identify which target keywords appear in competitor descriptions and which are missing. Map keywords to description slots.

**Success criteria:** Keyword priority list for description inclusion.

### Phase 3 - Three Versions

Write 3 GBP description versions (each exactly 750 characters or fewer):

- **Version 1: Keyword-focused** - Maximum ranking signal. Front-load primary keywords, mention all service areas, include every target service.
- **Version 2: Conversion-focused** - Written to generate calls. Lead with customer pain point, emphasize speed/reliability, strong CTA, phone number mention.
- **Version 3: Trust-focused** - Experience, reviews, local credibility. Years in business, review count, certifications, community involvement.

**Success criteria:** 3 descriptions, each 750 characters or fewer, each with a distinct strategic angle.

### Phase 4 - A/B Testing Recommendation

Include testing plan: run each version for 30 days, track GBP impressions, calls, direction requests, and website clicks. Recommend starting with Version 1.

**Success criteria:** Testing timeline and metrics documented.

### Output

Write `./reports/07-description-optimization.md` with standard report format and competitor analysis.

Write `./deliverables/gbp-description-drafts.md` with all 3 versions, character counts, and testing instructions.

---

## Sub-Command 8: `photo-audit` (Prompt 8)

**Goal:** Build an 8-week photo upload plan.

### Phase 1 - Competitor Photo Analysis

WebSearch competitor GBP listings for photo data: approximate count, types of photos, upload frequency.

**Success criteria:** Photo data estimated for at least 3 competitors.

### Phase 2 - Analysis

Analyze photo types, quality indicators, and frequency across competitors. Identify the top competitor's photo velocity.

**Success criteria:** Top competitor photo velocity identified.

### Phase 3 - 2026 Enhancement: GBP Vision AI

Google's AI now categorizes services from photos (2026 update). This means:
- High-quality, well-lit photos showing actual service work are critical
- Google can identify services, tools, and settings in photos
- Blurry, dark, or generic stock photos may hurt ranking
- Include file naming convention: `{service-keyword}-{city}-{description}.jpg`
- Include geotagging instructions for each service area (lat/long from context)

**Success criteria:** Naming convention and geotagging instructions documented.

### Phase 4 - Shot List

Build an 8-week photo upload plan:
- Target: beat top competitor's upload velocity by 50%
- Week-by-week shot list with:
  - Exact number to upload per week
  - What to photograph (specific scenes, angles)
  - Where to photograph (which service area, job site)
  - Why this photo matters (ranking signal, trust signal, service identification)
- Priority order across all weeks: before/afters first, then team shots, vehicles/equipment, completed work, office/storefront

**Success criteria:** 8-week plan with specific shot counts, descriptions, and locations.

### Output

Write `./reports/08-photo-audit.md` with standard report format and competitor analysis.

Write `./deliverables/photo-shot-list.md` with the complete 8-week shot list ready for a photographer or team member to execute.

## Sub-Command 9: `video-strategy` (2026 Enhancement)

**Goal:** Build a short-form video strategy for GBP. GBP video updates are now a primary ranking signal — Google's Vision AI processes video content to categorize services.

### Phase 1 - Competitor Video Analysis

- WebSearch each competitor's GBP for video content
- WebSearch `"{competitor name}" video {city}` for YouTube/social presence
- Note: which competitors post videos, how often, what type (testimonials, behind-the-scenes, how-to, before/after)

**Success criteria:** Video presence assessed for at least 3 competitors.

### Phase 2 - Video Strategy Build

Create a video content plan covering these types:
- **Video testimonials** (30-60 seconds) — happy customer sharing experience on camera
- **Behind-the-scenes** (15-30 seconds) — team arriving at job, setting up, working
- **Before/after showcases** (30-45 seconds) — dramatic transformation reveals
- **Quick tips / how-to** (30-60 seconds) — educational content positioning the business as expert
- **Meet the team** (30-45 seconds) — humanize the brand, build trust

For each video type, provide:
- Script outline (key talking points, not word-for-word)
- Filming tips (lighting, angle, background, audio)
- Optimal length for GBP
- Keywords to mention verbally (Google transcribes and indexes video audio)
- Thumbnail description

### Phase 3 - 8-Week Video Calendar

Build an 8-week video posting schedule:
- 1-2 videos per week (minimum to establish velocity)
- Alternate between video types for variety
- Each entry: week, video type, topic, script outline, keywords to mention, neighborhood/service area to feature
- Coordinate with the GBP posts calendar (videos and posts should complement, not duplicate)

### Phase 4 - Production Guide

Write a simple production guide for the business team:
- Equipment needed (smartphone is fine — list settings)
- Lighting basics (natural light, avoid backlighting)
- Audio tips (speak clearly, reduce background noise, face camera)
- Editing recommendations (free tools: CapCut, InShot)
- Upload process for GBP
- File naming convention: `{service}-{city}-{topic}-{date}.mp4`

**Success criteria:** 8-week calendar with 8-16 video entries, production guide written.

### Output

Write `./reports/09-video-strategy.md` with standard report format.

Write `./deliverables/gbp-video-scripts.md` with all video script outlines, calendar, and production guide.

---

## Sub-Command 10: `direct-booking` (2026 Enhancement)

**Goal:** Assess and recommend direct booking / messaging integration for GBP. "Reserve with Google" and direct messaging are now high-weight ranking factors.

### Phase 1 - Current State Assessment

- WebSearch `"{business name}" "book online"` and `"{business name}" "schedule appointment"` to check if online booking exists
- WebFetch business website to check for booking widgets, contact forms, scheduling tools
- Check if business type supports "Reserve with Google" (service businesses, restaurants, salons, medical)
- WebSearch competitor GBP listings for booking buttons and messaging availability

**Success criteria:** Current booking/messaging state documented for business and competitors.

### Phase 2 - Recommendations

Based on business type from context, recommend:

**Booking integration options (ranked by GBP impact):**
1. Reserve with Google (if eligible) — direct booking from GBP listing
2. Google Business Messages — real-time chat from GBP
3. Third-party booking widget on website (Calendly, Housecall Pro, ServiceTitan, etc.)
4. Simple contact form with "Request Appointment" CTA

**For each recommendation:**
- What it is and why it matters for ranking
- Setup steps (high-level — this varies by provider)
- Estimated setup time
- Monthly cost (if any)
- Ranking impact assessment (HIGH/MEDIUM/LOW)

### Phase 3 - Quick Win Actions

List immediate actions the business can take:
- Enable Google Business Messages (free, takes 5 minutes)
- Add "Book Online" button to GBP if booking system exists
- Add booking CTA to GBP description and posts
- Ensure phone number is click-to-call on mobile

**Success criteria:** Recommendations documented with setup steps and impact ratings.

### Output

Write `./reports/10-direct-booking.md` with standard report format and recommendations.

---

## Progress Tracking

After each sub-command completes, update the business context JSON at `~/.claude/config/local-seo/{slug}.json`:

```json
{
  "progress": {
    "01_category_audit": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/01-category-audit.md"},
    "02_attributes_audit": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/02-attributes-audit.md"},
    "03_competitor_reviews": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/03-competitor-reviews.md"},
    "04_review_strategy": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/04-review-strategy.md"},
    "05_posts_strategy": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/05-posts-strategy.md"},
    "06_services_optimization": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/06-services-optimization.md"},
    "07_description_optimization": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/07-description-optimization.md"},
    "08_photo_audit": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/08-photo-audit.md"},
    "09_video_strategy": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/09-video-strategy.md"},
    "10_direct_booking": {"status": "completed", "last_run": "YYYY-MM-DD", "output": "./reports/10-direct-booking.md"}
  }
}
```

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
6. **Next Steps** - Which audit to run next in the sequence

---

## Error Recovery

| Sub-command | Failure | Recovery |
|---|---|---|
| Any | No context file found | Tell user to run `/local-seo-context setup` first |
| Any | WebSearch returns limited data | Note data gaps in the report, proceed with available info, add manual verification action item |
| category-audit | Cannot find competitor categories | Try WebSearch with alternate queries (competitor name + "google business"), check competitor websites for service pages |
| attributes-audit | Few attributes found | Supplement with industry-standard attribute lists from WebSearch |
| competitor-reviews | Cannot access review data | Use aggregate data from WebSearch snippets (star count, total reviews visible in search results) |
| review-strategy | No existing responses to analyze | Note as finding (0% response rate), build templates from scratch using competitor patterns |
| posts-strategy | No competitor posts found | Note this as a competitive advantage, build calendar from industry best practices |
| services-optimization | Competitor services not visible | Use competitor website service pages as proxy |
| description-optimization | Cannot find competitor descriptions | Use WebSearch snippets as proxy, note limitation |
| photo-audit | Cannot assess photo quality or count | Focus on upload frequency strategy and naming conventions, estimate counts from search results |
| video-strategy | Cannot find competitor video content | Note as major competitive advantage — most local businesses aren't using video. Build strategy from best practices. |
| direct-booking | Business type not eligible for Reserve with Google | Focus on Google Business Messages and website booking widget recommendations |
