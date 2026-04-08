# Technical SEO Audit: Abundera Sign

**Date:** 2026-04-08

**Website:** https://sign.abundera.ai

**Type:** SaaS product (e-signature platform) — NOT a local service business

**Overall Grade: C+**

## Executive Summary

sign.abundera.ai is a well-built e-signature SaaS landing page with excellent schema markup (SoftwareApplication + Organization), strong content depth (~2,800 words), and good internal linking (~20+ links). However, it has critical SEO gaps: zero Google indexation for most pages (only 1 template page indexed), no meta descriptions, no canonical tags, no Open Graph tags, missing image optimization, and large inline JavaScript. The site has 87 pages in its sitemap (including 21 language variants) but Google has indexed essentially none of them. This is the #1 problem to solve.

## Data Gathered

- **Source:** WebFetch homepage, robots.txt, sitemap.xml, Google site: search
- **Sitemap pages:** 87 URLs (21 language variants, features, compare, docs, auth pages)
- **Google indexed:** ~1 page (1 template page found in site: search)
- **Schema markup:** SoftwareApplication + Organization JSON-LD
- **Launch status:** Pre-launch (June 1, 2026 US launch)

## Core Web Vitals Assessment

| Metric | Target | Estimated Status | Notes |
|--------|--------|-----------------|-------|
| LCP | < 2.5s | AT RISK | Large inline JS payload could delay rendering |
| INP | < 200ms | AT RISK | Multiple form handlers, event listeners, Intersection Observer |
| CLS | < 0.1 | LIKELY PASS | No obvious layout shift triggers |

## Crawlability & Indexation

| Check | Status | Notes |
|-------|--------|-------|
| robots.txt | PASS | Allows /, blocks /api/, /sign/, /portal/, /internal/, /functions/ |
| sitemap.xml | PASS | 87 URLs, all dated 2026-04-07 |
| HTTPS | PASS | Consistent |
| Indexed pages | **CRITICAL FAIL** | Only ~1 of 87 pages indexed by Google |
| Canonical tags | NOT FOUND | No canonical tags detected |
| Meta descriptions | NOT FOUND | None on homepage |
| Open Graph tags | NOT FOUND | No og:title, og:description, og:url |
| hreflang tags | UNKNOWN | 21 language variants in sitemap — need hreflang on pages |

## Structured Data

| Check | Status | Notes |
|-------|--------|-------|
| JSON-LD present | PASS | SoftwareApplication + Organization |
| Business name | PASS | Abundera, Inc. |
| Address | PASS | Full address with coordinates |
| Product info | PASS | Application category, pricing ($29/mo) |
| Aggregate rating | NOT FOUND | No reviews/ratings in schema |
| FAQ schema | NOT FOUND | FAQ content exists but not in schema |

## Competitive Landscape

**Abundera Sign does NOT appear in any "best e-signature" rankings.** Top competitors:
- DocuSign, HelloSign/Dropbox Sign, Adobe Sign (enterprise)
- OpenSign, Inkless, SignWell, Xodo Sign (free/open-source)
- Zoho Sign, PandaDoc (SMB)

**Key differentiator not being leveraged:** PAdES digital signatures + blockchain anchoring + court-ready evidence. No competitor emphasizes legal evidence quality this heavily — this is the SEO angle.

## Action Items

### CRITICAL

1. **Fix indexation** — 1 of 87 pages indexed is catastrophic. Possible causes:
   - Site is pre-launch (June 2026) — may not have been submitted to GSC
   - No canonical tags causing confusion
   - No meta descriptions means Google may skip pages
   - Action: Submit sitemap in Google Search Console immediately
   - Impact: CRITICAL | Time: 2-8 weeks for full indexation

2. **Add meta descriptions to ALL pages** — 87 pages with zero meta descriptions
   - Homepage: "Evidence-first e-signature platform. PAdES digital signatures, blockchain-anchored audit trails, and court-ready evidence on every document. Starting at $29/mo."
   - Impact: HIGH | Time: 2-4 weeks after indexation

3. **Add Open Graph tags** — No og:title, og:description, og:image, og:url
   - Critical for social sharing (product launches rely on social amplification)
   - Impact: HIGH | Time: immediate for social CTR

4. **Add canonical tags** — 21 language variants without hreflang/canonical = recipe for duplicate content penalties
   - Each language page needs: `<link rel="canonical">` + `<link rel="alternate" hreflang="xx">`
   - Impact: HIGH | Time: prevents future indexation issues

### HIGH

5. **Add FAQ schema** — The site has FAQ-worthy content (what is PAdES? why blockchain anchoring?) but no FAQPage schema
   - Would enable featured snippets for "what is PAdES digital signature"
   - Impact: MEDIUM-HIGH | Time: 4-8 weeks

6. **Create comparison/SEO content pages:**
   - "Abundera Sign vs DocuSign" (comparison page)
   - "What is PAdES digital signature" (educational, targets featured snippet)
   - "Best e-signature for lawyers" (targets legal vertical)
   - "Court-admissible e-signature" (unique differentiator keyword)
   - Impact: HIGH | Time: 4-12 weeks per page

7. **Reduce inline JavaScript** — Large inline JS bundle delays LCP
   - Move to external deferred/async scripts
   - Impact: MEDIUM | Time: immediate CWV improvement

### MEDIUM

8. **Add image alt attributes and optimization** — Logo lacks alt text, no WebP, no lazy loading
9. **Add phone number** — Builds trust, enables click-to-call
10. **Build backlink profile** — Zero backlinks detected. Target: legal tech blogs, e-signature comparison sites, SaaS directories (G2, Capterra, Product Hunt)
11. **Submit to comparison lists** — None of the "best e-signature 2026" articles mention Abundera Sign

## Keyword Opportunities

| Keyword | Competition | Abundera Angle |
|---------|------------|----------------|
| court admissible e-signature | LOW | Own this — no competitor emphasizes this |
| PAdES digital signature | LOW | Educational content opportunity |
| blockchain e-signature | MEDIUM | Unique differentiator |
| e-signature with audit trail | MEDIUM | Core feature |
| best e-signature for lawyers | HIGH | Vertical targeting |
| free e-signature alternative | VERY HIGH | Crowded market |

## Site Type Assessment

**This is a SaaS product site, not a local service business.** The local-seo plugin's GBP audits, citation checks, and map pack optimization are NOT the right fit. The useful audits are:
- Technical audit (this report)
- Keyword gap analysis (competitor keyword research)
- Content gap analysis (comparison pages, educational content)
- Entity optimization (schema, Product Hunt, G2, Capterra)
- Backlink audit (SaaS directory submissions, tech blog outreach)
