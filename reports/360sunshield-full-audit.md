# Full SEO Pipeline Audit: 360 Sun Shield

**Date:** 2026-04-09

**Website:** https://360sunshield.com

**Site Type:** E-commerce (Shopify supplement brand)

**Overall Grade: C**

---

## Quick Health Check

| Check | Status | Details |
|-------|--------|---------|
| Indexation | WARNING | ~10 pages visible in Google vs 47+ blog posts + pages in sitemap |
| Meta Description | CRITICAL | Not found on homepage |
| Sitemap | OK | 4 sub-sitemaps (products, pages, collections, blogs) |
| Canonical | WARNING | Not confirmed on homepage |
| Schema | PARTIAL | Organization + WebSite only — missing Product, FAQPage, Article |

---

## 1. Executive Summary

360 Sun Shield is a single-product Shopify supplement brand selling a Polypodium Leucotomos-based sun protection supplement at $34.99 ($29.74/mo subscription). The site has 47 blog posts, a science page with NIH citations, and standard Shopify infrastructure. Key problems: **no GBP listing, zero backlinks from authority sources, not appearing in any "best supplement" listicles, missing Product schema on the product page, no reviews/ratings displayed, and the dominant competitor (Heliocare) owns virtually every search result.** The biggest opportunity is the blog content — 47 articles targeting relevant keywords — but most lack meta descriptions and optimized titles.

---

## 2. Technical Audit (Week 1)

### Core Web Vitals

| Metric | Status | Notes |
|--------|--------|-------|
| LCP | AT RISK | Multiple render-blocking Shopify scripts, Trekkie analytics, Facebook pixel, Mailchimp |
| INP | AT RISK | Heavy JS payload from analytics + cart scripts |
| CLS | LIKELY OK | Standard Shopify layout |

### Technical Issues Found

| Issue | Severity | Fix |
|-------|----------|-----|
| **No meta descriptions** on homepage or key pages | CRITICAL | Write unique 150-160 char descriptions for every page |
| **Missing Open Graph tags** | HIGH | Add og:title, og:description, og:image, og:url to all pages |
| **No canonical tags confirmed** | HIGH | Shopify should auto-generate these — verify in page source |
| **Missing Product JSON-LD** on product page | HIGH | Add complete Product schema with price, availability, reviews |
| **No FAQ schema** on FAQs page | HIGH | Add FAQPage JSON-LD — direct rich snippet opportunity |
| **No Article schema** on 47 blog posts | HIGH | Add Article JSON-LD with author, datePublished, image |
| **Image alt text missing** on hero images and product images | MEDIUM | Add descriptive alt text with target keywords |
| **Render-blocking scripts** (3+ analytics bundles) | MEDIUM | Defer non-critical scripts (Mailchimp, FB pixel, Trekkie) |
| **No WebP images** detected | MEDIUM | Convert product/hero images to WebP via Shopify CDN |
| **Copyright year outdated** — says 2019-2025 | LOW | Update to 2019-2026 |
| **Font loading** optimized (font-display: swap) | OK | No action needed |
| **Viewport meta** present | OK | No action needed |

---

## 3. Keyword Gap Analysis (Week 4)

### Target Keywords: 360 Sun Shield vs Competitors

| Keyword | Monthly Search Est. | 360SS Ranking | Heliocare Ranking | Opportunity |
|---------|-------------------|--------------|-------------------|-------------|
| sun protection supplement | HIGH | NOT FOUND | Page 1 (Amazon) | CRITICAL — need to rank |
| polypodium leucotomos supplement | MEDIUM | NOT FOUND | Page 1 (Amazon, WebMD, Healthline) | HIGH |
| oral sun protection | MEDIUM | NOT FOUND | Page 1 (multiple) | HIGH |
| best sun protection supplement | MEDIUM | NOT FOUND | Page 1 (listicles) | HIGH — need listicle inclusion |
| sunscreen pill | MEDIUM | NOT FOUND | Page 1 (GoodRx, medical sites) | MEDIUM — educational opportunity |
| astaxanthin sun protection | MEDIUM | NOT FOUND | Page 1 (Ingenious Labs) | HIGH — have blog content |
| supplement for sun damage | LOW-MEDIUM | NOT FOUND | Scattered | MEDIUM |
| polypodium leucotomos extract benefits | LOW-MEDIUM | Blog post exists | Healthline, WebMD | MEDIUM — optimize existing |
| oral sunscreen supplement | LOW | NOT FOUND | Page 1 | MEDIUM |
| natural UV protection supplement | LOW | NOT FOUND | Various | HIGH — low competition |

### Key Finding

**360 Sun Shield ranks for ZERO high-value keywords.** The site is invisible for every money keyword in its niche. Heliocare (via Amazon listings, medical site mentions, and brand recognition) dominates the entire category. The 47 blog posts are targeting the right topics but not ranking.

---

## 4. Competitor Analysis

### Primary Competitor: Heliocare

| Factor | Heliocare | 360 Sun Shield |
|--------|----------|----------------|
| Amazon presence | YES (multiple listings, reviews) | NO |
| Medical site mentions | YES (Mayo Clinic, dermatologist recommendations) | NO |
| "Best of" listicle inclusion | YES (6+ articles) | NO |
| Product schema | YES (via Amazon) | NO |
| Review count | 1000+ on Amazon | ZERO visible |
| Price | ~$25-30 | $34.99 ($29.74 subscription) |
| Brand recognition | Established since 1990s | Unknown |

### Secondary Competitors

- **VitaArmor** — newer brand, ranking for "sun protection supplement"
- **Farma Dorsch** — European brand with niacinamide focus
- **NusaPure** — Amazon-dominant, budget option
- **SuperSmart** — supplement marketplace with PLE product

### Competitive Gaps (Opportunities for 360SS)

1. **No competitor emphasizes "complete formula"** — 360SS has PLE + astaxanthin + vitamins in one pill
2. **No competitor owns "clinically tested" angle** with NIH citations
3. **rosacea/lupus niches** are underserved — 360SS has blog content for these
4. **"Natural" and "plant-based"** positioning is underused in the supplement space

---

## 5. Content Audit (Week 8)

### Blog Content: 47 Posts

**Strengths:**
- Good topic coverage (PLE, astaxanthin, vitamin D, sun safety, rosacea, lupus)
- Science-backed content with PubMed/NIH citations on /the-science page
- Targeting long-tail keywords (sun and skin rash, drinks for healthy skin)

**Weaknesses:**
- No meta descriptions on blog posts
- Title tags cut off (Shopify default "... – 360 Sun Shield" pattern)
- No Article schema on any post
- No internal linking strategy (posts don't link to product page effectively)
- No author bio/byline (hurts E-E-A-T)
- No publication dates visible (hurts freshness signals)
- Missing images on many posts (text-heavy)

### Missing Content (Gaps vs Competitors)

| Content Needed | Target Keyword | Type |
|---------------|---------------|------|
| "360 Sun Shield vs Heliocare" comparison | "heliocare alternative" | Comparison page |
| "Best Polypodium Leucotomos Supplements 2026" | "best PLE supplement" | Listicle (own the category) |
| "How to Choose a Sun Protection Supplement" | "sun protection supplement guide" | Buyer's guide |
| "Sun Protection Supplement for Runners/Athletes" | "sun supplement for athletes" | Niche targeting |
| "Dermatologist-Recommended Sun Supplements" | "dermatologist recommended sun pill" | Authority content |
| FAQ page with schema | "is oral sunscreen safe" + 10 more | FAQPage schema |

---

## 6. Authority / Backlink Analysis (Week 7)

### Current Backlink Profile

**Essentially zero authority backlinks detected.** The site has:
- Own social media (Facebook, Instagram, TikTok)
- No mentions in any medical/health authority sites
- No mentions in any "best supplement" listicles
- Not listed on Amazon, G2, Trustpilot, or any review platform
- Not listed on any supplement comparison sites

### 90-Day Link Building Plan

**Month 1 — Directory & Marketplace (5 targets):**
1. **Amazon listing** — Create an Amazon product page (this is where Heliocare gets most authority)
2. **Trustpilot/Reviews.io** — Create a profile, start collecting reviews
3. **Product Hunt** — Launch for the supplement/health community
4. **Supplement comparison directories** — Submit to iHerb, Vitacost, supplement review sites
5. **Shopify App Store / ecosystem** — Get listed in any relevant Shopify supplement directories

**Month 2 — Content & PR (5 targets):**
6. **Pitch to health bloggers** — "Best sun protection supplement" listicle authors (BodyBio, Coastal Thyme, Integrative Dermatology)
7. **Pitch to dermatology blogs** — Dr. Hoffman, DermNet NZ, dermatologist sites
8. **Guest post on skincare blogs** — Offer expert content on PLE research
9. **HARO / Qwoted** — Respond to journalist queries about sun protection
10. **Podcast appearances** — Health/wellness podcasts discussing sun protection

**Month 3 — Authority (5 targets):**
11. **Parade.com "SPF Excellence Awards"** — They publish annual best sun protection lists
12. **GoodRx / Healthline** — Pitch for inclusion in their sunscreen pill articles
13. **Clinical study sponsorship** — Commission or cite a study, get mentioned in PubMed
14. **Dermatologist partnership** — Get a board-certified derm to endorse on their site
15. **Local Las Vegas business press** — Vegas-based supplement brand story

---

## 7. Entity / Schema Optimization (Week 9)

### Current Schema

```
Present: Organization, WebSite
Missing: Product, FAQPage, Article, BreadcrumbList, Review/AggregateRating
```

### Required Schema Additions

**1. Product schema on /products/360-sun-shield:**
```json
{
  "@type": "Product",
  "name": "360 Sun Shield",
  "description": "Natural sun protection supplement with Polypodium Leucotomos, Astaxanthin, and essential vitamins",
  "brand": { "@type": "Brand", "name": "360 Sun Shield" },
  "offers": {
    "@type": "Offer",
    "price": "34.99",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "url": "https://360sunshield.com/products/360-sun-shield"
  },
  "image": "https://360sunshield.com/cdn/shop/files/Main_product_image.png"
}
```

**2. FAQPage schema on /pages/faqs** — convert existing FAQ content to structured data

**3. Article schema on all 47 blog posts** — with author, datePublished, image, publisher

**4. BreadcrumbList** on all pages (helps Google understand site structure)

---

## 8. Review / Social Proof Strategy

### Current State: ZERO social proof visible

No customer reviews, no ratings, no testimonials, no trust badges on product page.

### Immediate Actions

1. **Add Shopify review app** (Judge.me, Loox, or Stamped.io) — display reviews on product page
2. **Add AggregateRating schema** once reviews exist
3. **Email past customers** asking for reviews (30-day post-purchase flow)
4. **Add trust badges** to product page: CGMP certified, clinically tested, 30-day guarantee
5. **Feature testimonials** on homepage (even 3-5 makes a huge difference)
6. **Create a "Results" or "Before/After" page** with customer stories

---

## 9. Top 10 Priority Actions (Ranked by Impact)

| # | Action | Impact | Effort | Timeline |
|---|--------|--------|--------|----------|
| 1 | **Add Product schema** to product page | HIGH | Low | 1 day |
| 2 | **Write meta descriptions** for homepage, product, science, all 47 blogs | HIGH | Medium | 1 week |
| 3 | **Add customer reviews** to product page (install review app) | HIGH | Low | 1 week |
| 4 | **Create Amazon listing** | VERY HIGH | Medium | 2 weeks |
| 5 | **Add FAQ schema** to FAQs page | HIGH | Low | 1 day |
| 6 | **Write "360 Sun Shield vs Heliocare" comparison page** | HIGH | Medium | 1 week |
| 7 | **Add Open Graph tags** to all pages | MEDIUM | Low | 1 day |
| 8 | **Add Article schema** to all 47 blog posts | HIGH | Medium | 1 week |
| 9 | **Pitch to 5 "best supplement" listicle authors** | VERY HIGH | High | 1 month |
| 10 | **Add author bio + dates to blog posts** (E-E-A-T) | MEDIUM | Low | 1 week |

---

## 10. 12-Week Execution Plan (Adapted for E-Commerce)

| Week | Focus | Deliverables |
|------|-------|-------------|
| 1 | Technical fixes | Meta descriptions, OG tags, canonical verification, Product schema, FAQ schema |
| 2 | Reviews + social proof | Install review app, email past customers, add trust badges |
| 3 | Schema blitz | Article schema on all 47 posts, BreadcrumbList, AggregateRating |
| 4 | Amazon launch | Create Amazon listing, set up FBA or FBM, optimize listing SEO |
| 5 | Content creation | "vs Heliocare" comparison, "Best PLE Supplement 2026" listicle, buyer's guide |
| 6 | Blog optimization | Rewrite titles/metas for all 47 posts, add internal links to product page |
| 7 | Backlink campaign | Pitch to 10 health bloggers, submit to supplement directories |
| 8 | Niche content | Rosacea, lupus, athlete-specific content (underserved niches) |
| 9 | PR push | HARO responses, podcast pitching, local Vegas press |
| 10 | E-E-A-T strengthening | Add author bios, dermatologist endorsement, clinical study citations |
| 11 | First performance report | GSC analysis, keyword tracking, review count check |
| 12 | Re-audit + Q2 plan | Measure what moved, plan next quarter |

---

## Site Type Notes

360 Sun Shield is an **e-commerce single-product supplement brand**, not a local service business. The following standard local-seo audits were **skipped as not applicable:**
- GBP category/attribute audit (no GBP listing needed for e-commerce)
- Citation NAP audit (no local directories to check)
- Map pack optimization (not a local business)
- Review response templates (no GBP reviews)
- Video/photo GBP strategy (no GBP listing)
- Service + city pages (not a service business)

**Applicable audits that were run:**
- Technical audit (universal)
- Keyword gap analysis (universal)
- Competitor analysis (universal)
- Content gap analysis (universal)
- Entity/schema optimization (universal)
- Backlink analysis (universal)
- Review/social proof strategy (adapted for e-commerce)
