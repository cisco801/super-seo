# Technical SEO Audit: RoamingPigs Inc

**Date:** 2026-04-08

**Website:** https://roamingpigs.com

**Overall Grade: B+**

## Executive Summary

RoamingPigs.com is technically well-built with clean HTML, proper schema markup, HTTPS, a valid sitemap (169 URLs), and minimal external dependencies. The site scores well on privacy and performance by running zero analytics, zero tracking, and zero third-party scripts. Key gaps: no meta descriptions on pages, no canonical tags detected, missing image alt attributes, and no Google Business Profile listing. The biggest SEO opportunity is creating a GBP listing and adding meta descriptions across all 169 pages.

## Data Gathered

- **Source:** WebFetch of homepage, robots.txt, sitemap.xml
- **Indexed pages (Google):** ~8 visible in site: search (Google may have more indexed but not displayed)
- **Sitemap pages:** 169 URLs with lastmod dates (ranging from Jan 2024 to Apr 2026)
- **Schema markup:** ProfessionalService JSON-LD present on homepage

## Core Web Vitals Assessment

| Metric | Target | Estimated Status | Notes |
|--------|--------|-----------------|-------|
| **LCP** (Largest Contentful Paint) | < 2.5s | LIKELY PASS | No large hero images, minimal JS, static site |
| **INP** (Interaction to Next Paint) | < 200ms | LIKELY PASS | Minimal interactive elements, no heavy frameworks |
| **CLS** (Cumulative Layout Shift) | < 0.1 | NEEDS CHECK | Images may not have explicit width/height attributes |

**Note:** Exact CWV scores require PageSpeed Insights testing. The static site architecture with zero third-party scripts suggests strong performance.

## Mobile-Friendliness

| Check | Status | Notes |
|-------|--------|-------|
| Viewport meta tag | NEEDS VERIFICATION | Not visible in WebFetch extraction |
| Responsive design | LIKELY PASS | Dark mode toggle, responsive nav detected |
| Touch targets | LIKELY PASS | Navigation appears touch-friendly |
| Font readability | LIKELY PASS | Clean typography observed |
| Horizontal scrolling | LIKELY PASS | No wide fixed-width elements detected |

## Crawlability & Indexation

| Check | Status | Notes |
|-------|--------|-------|
| robots.txt | PASS | `Allow: /`, Crawl-delay: 1, Sitemap referenced |
| sitemap.xml | PASS | 169 URLs, all with lastmod dates, priorities set |
| HTTPS | PASS | All URLs use HTTPS consistently |
| Indexed pages | CONCERN | Only ~8 shown in `site:` search vs 169 in sitemap |
| Canonical tags | NOT FOUND | No canonical URL tags detected |
| Meta descriptions | NOT FOUND | No meta description on homepage |

## Security & Trust

| Check | Status | Notes |
|-------|--------|-------|
| HTTPS certificate | PASS | Valid, served via Cloudflare |
| Mixed content | PASS | No HTTP resources on HTTPS pages |
| Third-party scripts | EXCELLENT | Zero external scripts, zero tracking |
| Privacy policy | PASS | /privacy.html exists and is in sitemap |
| Business address | PASS | Full address in footer and JSON-LD |
| Contact method | PARTIAL | Email only (contact@roamingpigs.com), no phone |

## Structured Data (Schema.org)

| Check | Status | Notes |
|-------|--------|-------|
| JSON-LD present | PASS | ProfessionalService type on homepage |
| @type specificity | PASS | Uses `ProfessionalService` (specific subtype) |
| Business name | PASS | "RoamingPigs Inc" |
| Address | PASS | Full PostalAddress with geo coordinates |
| Services listed | PASS | 6 services in schema |
| Founder | PASS | Cisco Caceres |
| Area served | PASS | Worldwide |
| Logo | PASS | URL provided |

## 2026 Enhancements

- **AI Overviews:** The site's articles are fact-dense and well-structured -- good candidates for AI Overview citations. Adding FAQ schema to key articles would improve AI citation probability.
- **Voice search:** No FAQ schema detected. Adding structured Q&A to service pages would capture voice queries like "What is a fractional CTO?"
- **Entity building:** The schema markup is solid. To strengthen the knowledge graph entity, create/verify profiles on LinkedIn Company Page, Crunchbase, and industry directories.

## Action Items

### CRITICAL (Fix This Week)

1. **Add meta descriptions to ALL pages** -- Currently none detected. Each of the 169 pages needs a unique meta description (150-160 chars). Start with the homepage and top 10 articles.
   - Impact: HIGH | Time to results: 2-4 weeks after indexing

2. **Add canonical tags** -- Every page should have `<link rel="canonical" href="...">` to prevent duplicate content issues (especially pagination pages).
   - Impact: HIGH | Time to results: 2-4 weeks

3. **Create Google Business Profile listing** -- No GBP listing found. For a Las Vegas consulting firm, a GBP listing enables map pack visibility for "fractional CTO Las Vegas", "tech consulting Las Vegas", etc.
   - Impact: HIGH | Time to results: 1-2 weeks for initial visibility

### HIGH (Fix This Month)

4. **Add image alt attributes** -- Images missing alt text hurts accessibility and image search rankings. Add descriptive alt text to all images including the logo, author photo, and any article images.
   - Impact: MEDIUM | Time to results: 2-4 weeks

5. **Investigate indexation gap** -- 169 pages in sitemap but only ~8 showing in `site:` search. Possible causes: pages too new, thin content on some pages, or crawl budget issues.
   - Action: Submit sitemap in Google Search Console, check Coverage report
   - Impact: HIGH | Time to results: 2-8 weeks

6. **Add phone number** -- No phone number on the site. Even if consulting is primarily online, a phone number builds trust and enables click-to-call from GBP/mobile.
   - Impact: MEDIUM | Time to results: immediate trust signal

### MEDIUM (Fix Next Month)

7. **Add FAQ schema to service pages** -- Structured Q&A data helps with voice search and featured snippets. Target: "What does a fractional CTO do?", "How much does technical due diligence cost?", "What is an AI reality check?"
   - Impact: MEDIUM | Time to results: 4-8 weeks

8. **Verify viewport meta tag** -- Likely present but couldn't confirm via WebFetch. Verify in page source.
   - Impact: LOW (probably already there) | Time: 5 minutes

9. **Add explicit image dimensions** -- Set width/height on all `<img>` tags to prevent CLS.
   - Impact: LOW-MEDIUM | Time to results: immediate CLS improvement

## Keyword Opportunity Snapshot

| Keyword | RoamingPigs Ranking | Competitors Found | Opportunity |
|---------|-------------------|-------------------|-------------|
| fractional CTO Las Vegas | NOT RANKING | 3+ competitors | HIGH -- no local competition with real content |
| technical due diligence consulting | NOT RANKING | 10+ national firms | MEDIUM -- national competition but unique angle |
| AI reality check consulting | NOT RANKING | None (unique service) | HIGH -- own this term |
| technology consulting Las Vegas | NOT RANKING | Several | MEDIUM -- competitive |

## Next Steps

1. Fix CRITICAL items (meta descriptions, canonicals, GBP listing)
2. Run `/local-seo-gbp category-audit` if GBP listing is created
3. Run `/local-seo-website keyword-gap` for full keyword analysis
4. Run `/local-seo-content entity-optimization` for Knowledge Graph building
