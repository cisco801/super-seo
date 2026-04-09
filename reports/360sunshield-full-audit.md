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

---

#### ISSUE: No meta descriptions on homepage or key pages (CRITICAL)

**Where to add:** Shopify Admin > Online Store > Pages > [each page] > "Search engine listing preview" > Edit > Description field

**Homepage** (`/`):
```
360 Sun Shield is a clinically tested natural sun protection supplement with Polypodium Leucotomos, Astaxanthin, and essential vitamins. Protect your skin from UV rays from the inside out. $34.99.
```

**Product page** (`/products/360-sun-shield`):
```
360 Sun Shield — natural sun protection supplement with PLE, Astaxanthin, Vitamins A, C, D, E, Zinc & Selenium. 2 capsules daily. Starting at $29.74/mo. Free US shipping.
```

**Science page** (`/pages/the-science`):
```
The science behind 360 Sun Shield: clinically tested Polypodium Leucotomos and Astaxanthin protect skin at the cellular level. Backed by NIH research.
```

**Blog page** (`/blogs/blog`):
```
Sun protection insights, tips, and research from 360 Sun Shield. Learn about oral UV protection, skin health supplements, and natural photoprotection.
```

**About page** (`/pages/about`):
```
The story behind 360 Sun Shield — a Las Vegas supplement brand on a mission to revolutionize natural sun protection with plant-based, clinically tested ingredients.
```

**FAQs page** (`/pages/faqs`):
```
Common questions about 360 Sun Shield: how it works, ingredients, dosage, when to take it, and how it compares to sunscreen. Answers backed by research.
```

**Contact page** (`/pages/contact`):
```
Get in touch with 360 Sun Shield. Questions about our sun protection supplement? Wholesale inquiries? We'd love to hear from you.
```

---

#### ISSUE: Missing Open Graph tags (HIGH)

**Where to add:** In your Shopify theme, edit `theme.liquid` and add the following inside `<head>` before `</head>`. If your theme already has partial OG tags, replace them with this complete set.

**Homepage OG tags:**
```html
<meta property="og:title" content="360 Sun Shield — Natural Sun Protection Supplement">
<meta property="og:description" content="Clinically tested sun protection supplement with Polypodium Leucotomos and Astaxanthin. Protect your skin from UV rays from the inside out.">
<meta property="og:image" content="https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png">
<meta property="og:url" content="https://360sunshield.com">
<meta property="og:type" content="website">
<meta property="og:site_name" content="360 Sun Shield">
```

**Dynamic Liquid template for all pages** (add to `theme.liquid` inside `<head>`):
```html
<!-- Open Graph Tags -->
<meta property="og:site_name" content="{{ shop.name }}">
<meta property="og:url" content="{{ canonical_url }}">

{% if template.name == 'product' %}
  <meta property="og:type" content="product">
  <meta property="og:title" content="{{ product.title }}">
  <meta property="og:description" content="{{ product.description | strip_html | truncate: 200 }}">
  <meta property="og:image" content="https:{{ product.featured_image | image_url: width: 1200 }}">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:alt" content="{{ product.featured_image.alt | escape }}">
  <meta property="product:price:amount" content="{{ product.price | money_without_currency }}">
  <meta property="product:price:currency" content="{{ shop.currency }}">
{% elsif template.name == 'article' %}
  <meta property="og:type" content="article">
  <meta property="og:title" content="{{ article.title }}">
  <meta property="og:description" content="{{ article.excerpt_or_content | strip_html | truncate: 200 }}">
  {% if article.image %}
    <meta property="og:image" content="https:{{ article.image | image_url: width: 1200 }}">
  {% else %}
    <meta property="og:image" content="https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png">
  {% endif %}
  <meta property="article:published_time" content="{{ article.published_at | date: '%Y-%m-%dT%H:%M:%S%z' }}">
  <meta property="article:author" content="360 Sun Shield">
{% elsif template.name == 'page' %}
  <meta property="og:type" content="website">
  <meta property="og:title" content="{{ page.title }} — {{ shop.name }}">
  <meta property="og:description" content="{{ page.content | strip_html | truncate: 200 }}">
  <meta property="og:image" content="https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png">
{% else %}
  <meta property="og:type" content="website">
  <meta property="og:title" content="{{ page_title }}">
  <meta property="og:description" content="{{ page_description | default: shop.description }}">
  <meta property="og:image" content="https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png">
{% endif %}

<!-- Twitter Card Tags -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{{ page_title }}">
<meta name="twitter:description" content="{{ page_description | default: shop.description | strip_html | truncate: 200 }}">
{% if template.name == 'product' %}
  <meta name="twitter:image" content="https:{{ product.featured_image | image_url: width: 1200 }}">
{% elsif template.name == 'article' and article.image %}
  <meta name="twitter:image" content="https:{{ article.image | image_url: width: 1200 }}">
{% else %}
  <meta name="twitter:image" content="https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png">
{% endif %}
```

---

#### ISSUE: No canonical tags confirmed (HIGH)

**Verification steps:**
1. View page source on homepage, search for `<link rel="canonical"`
2. Shopify auto-generates these, but verify they exist on: `/`, `/products/360-sun-shield`, `/pages/the-science`, `/pages/faqs`, `/blogs/blog`
3. If missing, add this to `theme.liquid` inside `<head>`:

```html
<link rel="canonical" href="{{ canonical_url }}">
```

Shopify's `canonical_url` variable automatically handles pagination, collection filtering, and variant URLs.

---

#### ISSUE: Missing Product JSON-LD on product page (HIGH)

**Where to add (Shopify Online Store 2.0):**

1. Go to **Online Store > Themes > (...) > Edit Code**
2. In the left sidebar, open **Layout > `theme.liquid`**
3. Find the closing `</head>` tag
4. Paste this JSON-LD script **directly above** `</head>`, wrapped in a product template check:

```liquid
{% if template contains 'product' %}
{% render 'product-schema' %}
{% endif %}
```

5. Then create the snippet: In the left sidebar, click **Snippets > Add a new snippet** > name it `product-schema`
6. Paste the complete schema below into that snippet and save

**Why `theme.liquid` and not `product.liquid`:** Shopify 2.0 uses `templates/product.json` (a JSON config file, not Liquid). The actual markup lives in `sections/main-product.liquid`, but schema belongs in `<head>`, which is only in `theme.liquid`. The `{% if template contains 'product' %}` conditional ensures it only renders on product pages.

**Complete Product schema (`snippets/product-schema.liquid`):**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "360 Sun Shield",
  "description": "Natural sun protection supplement with Polypodium Leucotomos Extract, Astaxanthin, Beta-Carotene, Lycopene, and Vitamins A, C, D, E, B3 plus Zinc, Copper, and Selenium. 2 capsules daily for clinically tested UV protection from the inside out.",
  "image": "https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png",
  "brand": {
    "@type": "Brand",
    "name": "360 Sun Shield"
  },
  "sku": "19004",
  "offers": {
    "@type": "Offer",
    "price": "34.99",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "url": "https://360sunshield.com/products/360-sun-shield",
    "seller": {
      "@type": "Organization",
      "name": "360 Sun Shield LLC"
    }
  },
  "category": "Health & Beauty > Health Care > Vitamins & Supplements",
  "manufacturer": {
    "@type": "Organization",
    "name": "360 Sun Shield LLC"
  }
}
</script>
```

**Note:** Once you have customer reviews, add `aggregateRating` and `review` properties to this schema. Example addition (add inside the Product object after "manufacturer"):
```json
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "reviewCount": "47"
  },
  "review": [
    {
      "@type": "Review",
      "author": { "@type": "Person", "name": "Sarah M." },
      "datePublished": "2026-03-15",
      "reviewBody": "I take two capsules every morning before my outdoor runs. My skin feels so much more protected.",
      "reviewRating": { "@type": "Rating", "ratingValue": "5" }
    }
  ]
```

---

#### ISSUE: No FAQ schema on FAQs page (HIGH)

**Where to add (Shopify 2.0):**

1. Go to **Online Store > Themes > Edit Code**
2. Click **Snippets > Add a new snippet** > name it `faq-schema`
3. Paste the JSON-LD below into the snippet and save
4. Open **Layout > `theme.liquid`**, find the closing `</head>` tag
5. Add this conditional render **above** `</head>` (right next to the product-schema render if you already added that):

```liquid
{% if page.handle == 'faqs' %}
  {% render 'faq-schema' %}
{% endif %}
```

This only fires on the FAQs page (`/pages/faqs`). The `page.handle` matches the page URL slug in Shopify.

**Complete FAQPage schema (`snippets/faq-schema.liquid`):**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is 360 Sun Shield?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "360 Sun Shield is a daily oral sun protection supplement that helps protect your skin from UV damage from the inside out. It combines clinically tested Polypodium Leucotomos Extract (PLE) with Astaxanthin, Beta-Carotene, Lycopene, and essential vitamins and minerals including Vitamins A, C, D, E, B3, Zinc, Copper, and Selenium. Take 2 capsules daily for comprehensive photoprotection."
      }
    },
    {
      "@type": "Question",
      "name": "How does Polypodium Leucotomos protect skin from the sun?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Polypodium Leucotomos Extract (PLE) is a tropical fern extract that has been clinically studied for its photoprotective properties. Research published by the National Institutes of Health (NIH) shows that PLE helps reduce UV-induced skin damage by acting as a powerful antioxidant, reducing oxidative stress in skin cells, decreasing inflammation caused by sun exposure, and supporting the skin's natural defense mechanisms against UV radiation."
      }
    },
    {
      "@type": "Question",
      "name": "Can 360 Sun Shield replace sunscreen?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. 360 Sun Shield is designed to complement your existing sun protection routine, not replace sunscreen. Think of it as an additional layer of defense — sunscreen protects from the outside, while 360 Sun Shield supports your skin's natural UV defense from the inside. We recommend continuing to use broad-spectrum sunscreen, wear protective clothing, and seek shade during peak UV hours alongside taking 360 Sun Shield."
      }
    },
    {
      "@type": "Question",
      "name": "How many capsules should I take per day?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "The recommended dosage is 2 capsules daily. For best results, take both capsules in the morning with food, ideally 30-60 minutes before sun exposure. Each bottle contains a 30-day supply (60 capsules). Consistent daily use provides the best protection, as the ingredients build up in your system over time."
      }
    },
    {
      "@type": "Question",
      "name": "Is 360 Sun Shield safe?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes. 360 Sun Shield is manufactured in a CGMP-certified facility in the United States. All ingredients are plant-based, naturally derived, and have been clinically studied. Polypodium Leucotomos has been used safely for decades in both topical and oral forms. Astaxanthin is a naturally occurring carotenoid found in microalgae. However, as with any supplement, consult your healthcare provider before starting if you are pregnant, nursing, taking medication, or have a medical condition."
      }
    },
    {
      "@type": "Question",
      "name": "How long does it take for 360 Sun Shield to work?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "While some ingredients begin working within hours of your first dose, the full protective benefits build over consistent daily use. Most clinical studies on Polypodium Leucotomos show measurable photoprotection within 2-4 weeks of daily supplementation. For best results, we recommend taking 360 Sun Shield consistently every day rather than only before sun exposure."
      }
    },
    {
      "@type": "Question",
      "name": "What makes 360 Sun Shield different from Heliocare?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "While Heliocare contains only Polypodium Leucotomos Extract as its active ingredient, 360 Sun Shield provides a comprehensive formula that combines PLE with Astaxanthin, Beta-Carotene, Lycopene, and 7 essential vitamins and minerals (A, C, D, E, B3, Zinc, Copper, and Selenium). This complete approach addresses UV protection from multiple angles rather than relying on a single ingredient. 360 Sun Shield is also manufactured in the USA in a CGMP-certified facility."
      }
    }
  ]
}
</script>
```

---

#### ISSUE: No Article schema on 47 blog posts (HIGH)

**Where to add (Shopify 2.0):**

1. Go to **Online Store > Themes > Edit Code**
2. Click **Snippets > Add a new snippet** > name it `article-schema`
3. Paste the dynamic Liquid template below into the snippet and save
4. Open **Layout > `theme.liquid`**, find the closing `</head>` tag
5. Add this conditional render **above** `</head>`:

```liquid
{% if template contains 'article' %}
  {% render 'article-schema' %}
{% endif %}
```

This automatically renders on every blog post. The Liquid variables (`article.title`, `article.published_at`, etc.) are populated by Shopify for each post — you don't need to edit anything per article. One snippet covers all 47 posts.

**Dynamic Article schema template (`snippets/article-schema.liquid`):**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": {{ article.title | json }},
  "author": {
    "@type": "Organization",
    "name": "360 Sun Shield"
  },
  "publisher": {
    "@type": "Organization",
    "name": "360 Sun Shield",
    "logo": {
      "@type": "ImageObject",
      "url": "https://360sunshield.com/cdn/shop/files/Group_1000001781.png"
    }
  },
  "datePublished": "{{ article.published_at | date: '%Y-%m-%dT%H:%M:%S%z' }}",
  "dateModified": "{{ article.updated_at | date: '%Y-%m-%dT%H:%M:%S%z' }}",
  {% if article.image %}
  "image": "https:{{ article.image | image_url: width: 1200 }}",
  {% else %}
  "image": "https://360sunshield.com/cdn/shop/files/Main_product_image_version_3.png",
  {% endif %}
  "description": {{ article.excerpt_or_content | strip_html | truncate: 160 | json }},
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{ shop.url }}{{ article.url }}"
  }
}
</script>
```

This automatically populates from Shopify's article data and will apply to all 47 existing posts and any new posts.

---

#### ISSUE: Image alt text missing on hero and product images (MEDIUM)

**Where to fix:** Shopify Admin > Products > 360 Sun Shield > click each image > add alt text. For theme images: Online Store > Themes > Customize > click image blocks > add alt text.

**Recommended alt text for key images:**

| Image | Alt Text |
|-------|----------|
| Main product image | 360 Sun Shield natural sun protection supplement bottle - 60 capsules with Polypodium Leucotomos and Astaxanthin |
| Product image (supplement facts) | 360 Sun Shield supplement facts panel showing Polypodium Leucotomos Extract, Astaxanthin, Beta-Carotene, Lycopene, Vitamins A C D E B3, Zinc, Copper, Selenium |
| Hero banner (homepage) | 360 Sun Shield - protect your skin from UV rays from the inside out with clinically tested ingredients |
| Science page hero | The science behind 360 Sun Shield - NIH-researched sun protection ingredients |
| Lifestyle images | Person taking 360 Sun Shield sun protection supplement before outdoor activity |

---

#### ISSUE: Render-blocking scripts (MEDIUM)

**Where to fix:** In `theme.liquid`, find the analytics and tracking scripts and add `defer` or move them.

**Before (blocking):**
```html
<script src="https://cdn.shopify.com/s/.../trekkie.storefront..."></script>
<script src="https://connect.facebook.net/en_US/fbevents.js"></script>
<script src="https://chimpstatic.com/mcjs-connected/js/users/..."></script>
```

**After (deferred):**
```html
<!-- Move these to just before </body> instead of in <head> -->
<!-- Or add defer attribute -->
<script src="https://connect.facebook.net/en_US/fbevents.js" defer></script>
<script src="https://chimpstatic.com/mcjs-connected/js/users/..." defer></script>
```

**Note:** Trekkie is Shopify's own analytics and cannot be easily deferred. Focus on Facebook Pixel and Mailchimp. For Facebook Pixel specifically, you can lazy-load it:

```html
<script>
  // Lazy-load Facebook Pixel after page load
  window.addEventListener('load', function() {
    setTimeout(function() {
      !function(f,b,e,v,n,t,s)
      {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
      n.callMethod.apply(n,arguments):n.queue.push(arguments)};
      if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
      n.queue=[];t=b.createElement(e);t.async=!0;
      t.src=v;s=b.getElementsByTagName(e)[0];
      s.parentNode.insertBefore(t,s)}(window, document,'script',
      'https://connect.facebook.net/en_US/fbevents.js');
      fbq('init', 'YOUR_PIXEL_ID');
      fbq('track', 'PageView');
    }, 3000); // 3 second delay
  });
</script>
```

---

#### ISSUE: No WebP images detected (MEDIUM)

Shopify's CDN automatically serves WebP when you use `image_url` filter. Verify your theme templates use the modern format:

**Before:**
```html
<img src="{{ image.src }}" alt="{{ image.alt }}">
```

**After:**
```html
<img src="{{ image | image_url: width: 800, format: 'webp' }}"
     alt="{{ image.alt }}"
     loading="lazy"
     width="{{ image.width }}"
     height="{{ image.height }}">
```

For hero/above-the-fold images, omit `loading="lazy"` (they should load immediately).

---

#### ISSUE: Copyright year outdated (LOW)

**Where to fix:** In `theme.liquid` or `footer.liquid`, find the copyright text.

**Before:**
```
&copy; 2019-2025 360 Sun Shield
```

**After — use auto-updating year:**
```html
&copy; 2019-{{ 'now' | date: '%Y' }} 360 Sun Shield
```

This will automatically update every year.

---

| Issue | Status | Notes |
|-------|--------|-------|
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
- Title tags cut off (Shopify default "... -- 360 Sun Shield" pattern)
- No Article schema on any post
- No internal linking strategy (posts don't link to product page effectively)
- No author bio/byline (hurts E-E-A-T)
- No publication dates visible (hurts freshness signals)
- Missing images on many posts (text-heavy)

### Missing Content (Gaps vs Competitors)

---

#### CONTENT GAP 1: "360 Sun Shield vs Heliocare" Comparison Page

```
URL: /blogs/blog/360-sun-shield-vs-heliocare
Title: "360 Sun Shield vs Heliocare: Which Sun Protection Supplement Is Better?"
Meta: "Comparing 360 Sun Shield and Heliocare side-by-side: ingredients, dosage, price, research, and results. Find the best sun protection supplement for you."

H1: 360 Sun Shield vs Heliocare: Complete Comparison

H2: Quick Comparison Table
[Table: ingredient, dosage, price, capsules per day, vitamins included, subscription option, made in]

H2: What Is Polypodium Leucotomos Extract?
[100 words explaining PLE, link to /pages/the-science]

H2: Ingredient Breakdown
[Side-by-side: Heliocare has 240mg PLE only; 360SS has PLE + Astaxanthin + 7 vitamins/minerals]

H2: Price Comparison
[Heliocare ~$25-30 for 60 caps; 360SS $34.99 (or $29.74/mo subscription); but 360SS has MORE ingredients per capsule]

H2: What the Research Says
[Cite the NIH studies on PLE and Astaxanthin, link to PubMed]

H2: Which Should You Choose?
[Decision framework: if you want PLE only → Heliocare; if you want complete formula → 360SS]

H2: Frequently Asked Questions
[3 FAQ with schema]

CTA: "Try 360 Sun Shield — 30-Day Money-Back Guarantee"
Word count: 1,500-2,000
Internal links: /products/360-sun-shield, /pages/the-science, /pages/faqs
```

**Comparison table to include in the article:**

```html
<table>
  <thead>
    <tr>
      <th>Feature</th>
      <th>360 Sun Shield</th>
      <th>Heliocare Ultra</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Polypodium Leucotomos Extract</td><td>✅ Yes</td><td>✅ Yes (240mg)</td></tr>
    <tr><td>Astaxanthin</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Beta-Carotene</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Lycopene</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Vitamin A</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Vitamin C</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Vitamin D</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Vitamin E</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Vitamin B3 (Niacinamide)</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Zinc</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Copper</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Selenium</td><td>✅ Yes</td><td>❌ No</td></tr>
    <tr><td>Total Ingredients</td><td><strong>12+</strong></td><td><strong>1</strong></td></tr>
    <tr><td>Capsules Per Day</td><td>2</td><td>1-2</td></tr>
    <tr><td>Price (one-time)</td><td>$34.99</td><td>$25-30</td></tr>
    <tr><td>Subscription Price</td><td>$29.74/mo</td><td>N/A</td></tr>
    <tr><td>Price Per Ingredient</td><td>~$2.92/ingredient</td><td>~$27.50/ingredient</td></tr>
    <tr><td>Made In</td><td>USA (CGMP)</td><td>Spain</td></tr>
    <tr><td>Money-Back Guarantee</td><td>30 days</td><td>Varies by seller</td></tr>
  </tbody>
</table>
```

**Schema to add to this comparison page:**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Is 360 Sun Shield better than Heliocare?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "360 Sun Shield offers a more comprehensive formula with 12+ ingredients including PLE, Astaxanthin, and 7 vitamins/minerals, while Heliocare contains only Polypodium Leucotomos Extract. If you want a complete sun protection supplement with multiple clinically tested ingredients, 360 Sun Shield provides better value per ingredient."
      }
    },
    {
      "@type": "Question",
      "name": "Why is 360 Sun Shield more expensive than Heliocare?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "360 Sun Shield contains 12+ active ingredients compared to Heliocare's single ingredient. When calculated per ingredient, 360 Sun Shield costs approximately $2.92 per ingredient vs Heliocare's ~$27.50 for its single ingredient. With a subscription, 360 Sun Shield drops to $29.74/month."
      }
    },
    {
      "@type": "Question",
      "name": "Can I switch from Heliocare to 360 Sun Shield?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes, you can switch at any time. Both supplements use Polypodium Leucotomos Extract as a core ingredient, so you will continue to get PLE's photoprotective benefits while gaining the additional protection of Astaxanthin, Beta-Carotene, Lycopene, and essential vitamins and minerals."
      }
    }
  ]
}
</script>
```

---

#### CONTENT GAP 2: "Best Polypodium Leucotomos Supplements 2026" Listicle

```
URL: /blogs/blog/best-polypodium-leucotomos-supplements
Title: "7 Best Polypodium Leucotomos Supplements in 2026 (Tested & Compared)"
Meta: "We compared the top PLE supplements for sun protection: 360 Sun Shield, Heliocare, NusaPure, SuperSmart, and more. Full ingredient and price breakdown."

H1: 7 Best Polypodium Leucotomos Supplements in 2026

H2: What to Look for in a PLE Supplement
[Criteria: PLE dosage, additional ingredients, third-party testing, price per serving, manufacturing standards]

H2: Our Top Picks

  H3: 1. 360 Sun Shield — Best Overall (Complete Formula)
  [PLE + Astaxanthin + 7 vitamins/minerals, $34.99, CGMP, subscription available]
  [Link: /products/360-sun-shield]

  H3: 2. Heliocare Ultra — Best Single-Ingredient PLE
  [240mg PLE, ~$27, established brand since 1990s]

  H3: 3. NusaPure Polypodium Leucotomos — Best Budget Option
  [High-dose PLE, Amazon-only, ~$15-20]

  H3: 4. SuperSmart Polypodium Leucotomos — Best for Stacking
  [Pure PLE capsules, good for people who want to build their own supplement stack]

  H3: 5. Life Extension Sun Protection — Best Multi-Ingredient Alternative
  [PLE + nicotinamide, different formula approach]

  H3: 6. Solgar Ester-C Plus — Best Vitamin C + Sun Support
  [Not PLE-specific but popular for sun-related skin health]

  H3: 7. Garden of Life mykind Organics — Best Organic Option
  [Organic plant-based, different approach to sun nutrition]

H2: How We Tested
[Methodology: ingredient analysis, price comparison, manufacturing standards, customer reviews]

H2: Complete Comparison Table
[All 7 products side by side: PLE dosage, additional ingredients, price, capsules/day, Amazon rating, made in]

H2: What Is Polypodium Leucotomos?
[200 words on PLE science, link to /pages/the-science]

H2: Frequently Asked Questions
[3-5 FAQ with schema]

CTA: "Try Our Top Pick — 360 Sun Shield with 30-Day Money-Back Guarantee"
Word count: 2,000-2,500
Internal links: /products/360-sun-shield, /pages/the-science, /blogs/blog/360-sun-shield-vs-heliocare
```

---

#### CONTENT GAP 3: "How to Choose a Sun Protection Supplement" Buyer's Guide

```
URL: /blogs/blog/how-to-choose-sun-protection-supplement
Title: "How to Choose a Sun Protection Supplement: The Complete Guide"
Meta: "Learn what to look for in an oral sun protection supplement: key ingredients (PLE, Astaxanthin), dosage, safety, research, and what actually works."

H1: How to Choose a Sun Protection Supplement

H2: Do Oral Sun Protection Supplements Actually Work?
[Yes — explain the clinical evidence for PLE and Astaxanthin. Link to NIH studies.]

H2: 5 Ingredients to Look For
  H3: 1. Polypodium Leucotomos Extract (PLE)
  H3: 2. Astaxanthin
  H3: 3. Beta-Carotene & Lycopene
  H3: 4. Vitamins C & E (antioxidants)
  H3: 5. Vitamin D & Zinc (skin repair)

H2: 3 Red Flags to Avoid
  - Proprietary blends (hidden dosages)
  - No third-party testing or CGMP certification
  - Claims to "replace sunscreen" (no legitimate supplement claims this)

H2: How to Read the Supplement Facts Label
[Walkthrough using 360 Sun Shield's label as example]

H2: Our Recommendation
[360 Sun Shield checks all boxes, link to product page]

Word count: 1,200-1,500
Internal links: /products/360-sun-shield, /pages/the-science
```

---

#### CONTENT GAP 4: "Sun Protection Supplement for Runners/Athletes"

```
URL: /blogs/blog/sun-protection-supplement-for-athletes
Title: "The Best Sun Protection Supplement for Runners & Athletes (2026)"
Meta: "Why athletes need oral sun protection beyond sunscreen. How PLE and Astaxanthin protect skin during long outdoor training. What to take before your next run."

H1: Sun Protection for Athletes: Why Sunscreen Isn't Enough

H2: The Problem — Sunscreen Fails During Exercise
[Sweat washes it off, reapplication is impractical mid-run, missed spots]

H2: How Oral Sun Protection Fills the Gap
[Works from inside, can't sweat off, continuous protection]

H2: Key Ingredients Athletes Should Look For
[PLE for UV protection, Astaxanthin for muscle recovery + skin, Vitamin D for bone health]

H2: When to Take It
[Morning routine, 30-60 min before training, daily consistency]

H2: What We Recommend
[360 Sun Shield — complete formula for active lifestyles]

Word count: 1,000-1,500
Internal links: /products/360-sun-shield, /pages/the-science
```

---

#### CONTENT GAP 5: "Dermatologist-Recommended Sun Supplements"

```
URL: /blogs/blog/dermatologist-recommended-sun-supplements
Title: "What Dermatologists Say About Sun Protection Supplements (2026)"
Meta: "Do dermatologists recommend sun protection pills? Here's what board-certified dermatologists say about PLE, Astaxanthin, and oral photoprotection."

H1: Do Dermatologists Recommend Sun Protection Supplements?

H2: The Short Answer
[Yes — many dermatologists recommend PLE supplements as adjunct protection]

H2: The Clinical Evidence Dermatologists Cite
[NIH studies, Journal of the American Academy of Dermatology, Photodermatology journal]

H2: What Dermatologists Want You to Know
[It's complementary to sunscreen, not a replacement; consistency matters; look for clinical dosages]

H2: Recommended Products
[360 Sun Shield as the most complete formula available]

Word count: 1,200-1,500
Internal links: /products/360-sun-shield, /pages/the-science, /pages/faqs
```

---

#### CONTENT GAP 6: FAQ Page Schema Optimization

Already addressed in the FAQ schema section above. The existing `/pages/faqs` content needs to be:
1. Wrapped in FAQPage schema (see Section 2 fix above)
2. Reviewed and expanded to cover these additional questions:
   - "Does 360 Sun Shield work for darker skin tones?"
   - "Can I take 360 Sun Shield while pregnant?"
   - "Is 360 Sun Shield FDA approved?"
   - "How long does one bottle last?"
   - "Do you offer international shipping?"

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

---

#### Month 1 — Directory & Marketplace (5 targets)

**1. Amazon listing** — Create an Amazon product page (this is where Heliocare gets most authority)

Steps:
1. Create Amazon Seller Central account at sellercentral.amazon.com
2. Apply for "Dietary Supplements" category approval (requires: Certificate of Analysis, product images, supplement facts label photos)
3. Create listing under "Health & Household > Vitamins & Dietary Supplements > Herbal Supplements"
4. Optimize listing title: "360 Sun Shield - Natural Sun Protection Supplement with Polypodium Leucotomos, Astaxanthin, Vitamins A C D E B3, Zinc, Selenium - 60 Capsules (30-Day Supply)"
5. Add A+ Content with comparison chart vs Heliocare
6. Enroll in FBA for Prime badge

**2. Trustpilot profile** — Create a profile and start collecting reviews

Steps:
1. Go to business.trustpilot.com and claim your business
2. Set up your business profile with logo, description, and website URL
3. Use Trustpilot's review invitation feature to email past customers
4. Add Trustpilot widget to your Shopify footer using their integration

**3. Product Hunt launch** — Launch for the supplement/health community

Steps:
1. Create a Product Hunt maker profile at producthunt.com
2. Prepare assets: tagline ("Sun protection from the inside out"), description, images, video
3. Schedule launch on a Tuesday or Wednesday (highest traffic days)
4. Post to upcoming page 1 week before launch
5. Engage with every comment on launch day

**4. Supplement directories** — Submit to review and comparison sites

Submit to:
- Labdoor.com (submit for independent testing and rating)
- ConsumerLab.com (apply for product review)
- Examine.com (pitch for inclusion in PLE supplement guide)
- iHerb.com (apply as a vendor)
- Vitacost.com (apply as a vendor)

**5. Health and wellness directories**

Submit to:
- Better Business Bureau (bbb.org) — create a listing for 360 Sun Shield LLC
- Crunchbase — create a company profile
- LinkedIn — create a company page

---

#### Month 2 — Content & PR (5 targets)

**6. Pitch to health bloggers** — Target "best sun protection supplement" listicle authors

**Actual outreach email template:**

```
Subject: Quick addition for your sun protection supplement article?

Hi [Name],

I came across your article "[Article Title]" and noticed you cover
[Heliocare/sun protection supplements]. I wanted to let you know about
360 Sun Shield — it's the only sun protection supplement that combines
Polypodium Leucotomos Extract with Astaxanthin, Beta-Carotene, Lycopene,
and Vitamins A, C, D, E, B3, Zinc, Copper, and Selenium in a single
formula.

What makes it different:
- Complete formula (PLE + Astaxanthin + 7 vitamins/minerals) vs single-ingredient competitors
- Clinically tested ingredients backed by NIH research
- CGMP certified manufacturing in the USA
- $29.74/mo with subscription (no per-pill pricing games)

Would you be open to including it in your roundup? Happy to send a sample
for review or provide any additional information you need.

Best,
[Name]
360 Sun Shield
https://360sunshield.com
```

**Target sites and contacts to find:**
- BodyBio blog (look for their supplement guide author)
- Coastal Thyme (health/wellness blog with supplement reviews)
- The Klog / Skincare blogs covering "ingestible beauty"
- Byrdie / Well+Good freelance contributors (check bylines on sun protection articles)
- Healthline contributor outreach (via their "suggest a correction" links)

**7. Pitch to dermatology blogs**

**Outreach email for dermatologist sites:**

```
Subject: PLE supplement with multi-ingredient formula for your readers

Hi Dr. [Name],

I'm reaching out from 360 Sun Shield, a sun protection supplement brand.
We've developed a comprehensive formula that goes beyond standalone
Polypodium Leucotomos — we combine PLE with Astaxanthin, Beta-Carotene,
Lycopene, and Vitamins A, C, D, E, B3, Zinc, Copper, and Selenium.

Our science page (360sunshield.com/pages/the-science) cites the same NIH
and PubMed research your patients trust.

Would you be interested in reviewing our formula? I'd love to send you a
complimentary supply and our full ingredient breakdown for your evaluation.

We're also happy to provide content or quotes for any articles you're
working on related to oral photoprotection.

Best regards,
[Name]
360 Sun Shield
https://360sunshield.com
```

**Target dermatologist blogs:**
- DermNet NZ (editor contact via their site)
- Dr. Dray (YouTube + blog — contact via management)
- The Dermatology Review (thedermreview.com)
- RealSelf (submit as a product for review)

**8. Guest post on skincare blogs**

**Guest post pitch email:**

```
Subject: Guest post pitch: "The Science Behind Oral Sun Protection" (1,500 words)

Hi [Editor Name],

I'd like to contribute a guest article to [Blog Name] about oral sun
protection supplements — a topic your readers would find valuable as
awareness grows beyond topical-only UV protection.

Proposed article: "The Science Behind Oral Sun Protection: What Actually
Works"

Key points I'd cover:
- How Polypodium Leucotomos Extract protects skin (citing NIH research)
- The role of Astaxanthin as a "super antioxidant" for skin
- Why vitamins and minerals matter for UV defense
- What to look for (and avoid) when choosing a supplement
- How oral sun protection complements sunscreen

I can include 2-3 citations to peer-reviewed research. The article would
be original, exclusive to your site, and ready to publish.

About me: I work with 360 Sun Shield, a sun protection supplement brand
focused on clinically tested ingredients. [Link to /pages/the-science]

Happy to adjust the angle to fit your editorial calendar. Let me know!

Best,
[Name]
```

**Target blogs for guest posts:**
- Into The Gloss (Glossier's blog — large audience, accepts pitches)
- Skincare.com (L'Oreal property — contributor submissions)
- The Skincare Edit
- MindBodyGreen (accepts wellness contributor content)

**9. HARO / Qwoted / Connectively**

Sign up at:
- connectively.us (formerly HARO) — free tier available
- qwoted.com — free for sources
- sourcebottle.com — Australian but global reach
- helpareporter.com (if still active)

**Set alerts for these queries:**
- "sun protection"
- "sunscreen"
- "supplement"
- "skin health"
- "UV protection"
- "dermatology"

**Template response when a journalist query matches:**

```
Hi [Journalist Name],

Re: your query about [TOPIC].

I'm [Name] with 360 Sun Shield, a sun protection supplement company. [Relevant
credential/angle for the specific query.]

[2-3 sentence expert response addressing their specific question.]

Our product combines Polypodium Leucotomos Extract with Astaxanthin and 7
essential vitamins/minerals for comprehensive sun protection from the inside out.
More info: https://360sunshield.com/pages/the-science

Happy to provide additional quotes or information. Available for phone/email.

[Name]
[Title], 360 Sun Shield
[Phone]
[Email]
```

**10. Podcast appearances** — Health/wellness podcasts

**Podcast pitch email:**

```
Subject: Guest pitch: "The Future of Sun Protection Is a Pill" (unique angle)

Hi [Host Name],

I have a unique story for [Podcast Name]: sun protection is evolving
beyond lotions and creams, and the science is fascinating.

I'm [Name] from 360 Sun Shield. Here's the episode pitch:

"The Future of Sun Protection Is Oral"
- Why sunscreen alone isn't enough (the research most people don't know)
- How a tropical fern (Polypodium Leucotomos) protects skin from UV at
  the cellular level
- The NIH studies that changed everything
- Why Las Vegas of all places became ground zero for a sun supplement brand
- The "complete formula" approach vs single-ingredient products

This is a fresh, science-backed topic that would surprise your audience.
I can make the conversation engaging and accessible — no dry supplement talk.

Available for remote recording. Happy to work around your schedule.

Best,
[Name]
360 Sun Shield
https://360sunshield.com
```

**Target podcasts:**
- The Doctor's Farmacy (Dr. Mark Hyman)
- The Skinny Confidential
- Clean Beauty School (by Well+Good)
- The Wellness Mama Podcast
- Broken Brain Podcast
- Any dermatologist-hosted podcast

---

#### Month 3 — Authority (5 targets)

**11. Parade.com "SPF Excellence Awards"** and similar editorial lists

**Pitch email:**

```
Subject: Submission for your 2026 sun protection roundup

Hi [Editor],

I'd like to submit 360 Sun Shield for consideration in your upcoming sun
protection coverage.

360 Sun Shield is an oral sun protection supplement — not a topical — that
combines Polypodium Leucotomos Extract with Astaxanthin and 10 additional
vitamins/minerals. It's a unique entry in the sun protection category that
your readers may not have seen.

Key facts:
- Only sun supplement combining PLE + Astaxanthin + 7 vitamins/minerals
- All ingredients clinically tested with NIH-published research
- CGMP certified, manufactured in the USA
- $34.99 ($29.74 with subscription)
- 30-day money-back guarantee

Happy to send a press kit and product sample. High-res images available
at: [link to press assets]

Best,
[Name]
360 Sun Shield
```

**Target editorial lists:**
- Parade.com annual sun protection roundup
- Prevention.com best sun protection products
- Women's Health best supplement lists
- Shape.com supplement guides
- Men's Health sun protection guides

**12. GoodRx / Healthline** — Pitch for inclusion in their sunscreen pill articles

Find the existing articles first:
- GoodRx: "Are Sunscreen Pills Safe?" (or similar)
- Healthline: "Polypodium Leucotomos Extract" guide

Use the "suggest a correction" or editorial contact form to pitch inclusion of 360 Sun Shield as a product example.

**13. Clinical study sponsorship**

Options:
- Commission an independent study through a CRO (Clinical Research Organization)
- Partner with a university dermatology department (UNLV is local to Las Vegas)
- At minimum: create a "Clinical Research" page on the site that aggregates all PLE and Astaxanthin studies with proper citations

**14. Dermatologist partnership**

Steps:
1. Identify 3-5 board-certified dermatologists in Las Vegas (local advantage)
2. Offer complimentary product supply for evaluation
3. Ask for a professional review/endorsement for the website
4. If they blog or have a website, ask for a mention with link
5. Feature their name/credential on the product page as "Recommended by Board-Certified Dermatologists"

**15. Local Las Vegas business press**

**Press release pitch:**

```
Subject: Las Vegas Supplement Brand Launches Breakthrough Oral Sun Protection

FOR IMMEDIATE RELEASE

LAS VEGAS, NV — 360 Sun Shield LLC, a Las Vegas-based supplement company,
has launched what it calls the most comprehensive oral sun protection
supplement on the market.

Unlike single-ingredient competitors, 360 Sun Shield combines Polypodium
Leucotomos Extract — a clinically tested photoprotective compound — with
Astaxanthin, Beta-Carotene, Lycopene, and Vitamins A, C, D, E, B3, Zinc,
Copper, and Selenium.

"Las Vegas has some of the highest UV exposure in the country. We built
this product because we live it every day," said [Founder Name], founder
of 360 Sun Shield.

The supplement is backed by NIH-published research and manufactured in a
CGMP-certified facility. It retails for $34.99 with a subscription option
at $29.74/month.

For more information: https://360sunshield.com

Media contact: [email] | [phone]
```

**Target local outlets:**
- Las Vegas Review-Journal (business section)
- Vegas Inc
- Las Vegas Sun
- KTNV Channel 13 (local news — "Made in Vegas" segment)
- Nevada Business Magazine

---

## 7. Entity / Schema Optimization (Week 9)

### Current Schema

```
Present: Organization, WebSite
Missing: Product, FAQPage, Article, BreadcrumbList, Review/AggregateRating
```

### Required Schema Additions

**1. Product schema** — See complete implementation in Section 2 above.

**2. FAQPage schema** — See complete implementation in Section 2 above.

**3. Article schema** — See complete implementation in Section 2 above.

**4. BreadcrumbList schema on all pages**

**Where to add:** Create `snippets/breadcrumb-schema.liquid` and include it in `theme.liquid`.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "{{ shop.url }}"
    }
    {% if template.name == 'product' %}
    ,{
      "@type": "ListItem",
      "position": 2,
      "name": "Products",
      "item": "{{ shop.url }}/collections/all"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": {{ product.title | json }},
      "item": "{{ shop.url }}{{ product.url }}"
    }
    {% elsif template.name == 'article' %}
    ,{
      "@type": "ListItem",
      "position": 2,
      "name": {{ blog.title | json }},
      "item": "{{ shop.url }}{{ blog.url }}"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": {{ article.title | json }},
      "item": "{{ shop.url }}{{ article.url }}"
    }
    {% elsif template.name == 'page' %}
    ,{
      "@type": "ListItem",
      "position": 2,
      "name": {{ page.title | json }},
      "item": "{{ shop.url }}{{ page.url }}"
    }
    {% elsif template.name == 'blog' %}
    ,{
      "@type": "ListItem",
      "position": 2,
      "name": {{ blog.title | json }},
      "item": "{{ shop.url }}{{ blog.url }}"
    }
    {% endif %}
  ]
}
</script>
```

---

## 8. Review / Social Proof Strategy

### Current State: ZERO social proof visible

No customer reviews, no ratings, no testimonials, no trust badges on product page.

### Immediate Actions

---

#### ACTION 1: Add Shopify review app

**Recommended: Judge.me** (free plan available, best for SEO because it generates review schema automatically)

Steps:
1. Go to Shopify Admin > Apps > Search "Judge.me Product Reviews"
2. Install the app (free plan works to start)
3. In Judge.me settings, enable:
   - "Rich Snippets" (adds AggregateRating schema automatically)
   - "Review Widget" on product page
   - "All Reviews Page" at /pages/reviews
   - "Review Carousel" for homepage
4. Customize the widget to match your brand colors (dark blue: #1a365d or whatever your brand uses)

---

#### ACTION 2: Add AggregateRating schema

Judge.me handles this automatically. If you use a different review app, add this manually to your product schema (see Product schema in Section 2 — add the `aggregateRating` property once you have reviews).

---

#### ACTION 3: Email past customers asking for reviews

**Where to set up:** Judge.me > Settings > Email > "Review Request" or create a manual Shopify email campaign via Shopify Email or Klaviyo.

**Automated flow (set up in Judge.me):**
- Trigger: 30 days after order fulfillment
- Send review request email

**Manual outreach email for existing customers:**

```
Subject: How's your sun protection going? (Quick favor)

Hi [First Name],

You ordered 360 Sun Shield [X weeks] ago and we'd love to hear how
it's working for you.

If you have 60 seconds, would you leave us a quick review? It really
helps other people discover us.

[REVIEW LINK BUTTON]

Just share:
- What made you try a sun protection supplement
- How you've been using it (daily? before outdoor activities?)
- Any changes you've noticed

Thanks for being a customer!

The 360 Sun Shield Team

P.S. — If you're running low, your next bottle is just $29.74 with a
subscription: https://360sunshield.com/products/360-sun-shield
```

**Review link:** Use your review app's direct review URL. For Judge.me: `https://judge.me/reviews/360sunshield#new-review` (or the link provided in your Judge.me dashboard).

---

#### ACTION 4: Add trust badges to product page

**Where to add (Shopify 2.0):** Go to Online Store > Themes > Edit Code > open `sections/main-product.liquid` (the section that renders your product page). Find the "Add to Cart" button block and paste this HTML below it. If your theme uses a different section name, search for `AddToCart` or `product-form` in the sections folder.

```html
<div class="trust-badges" style="display: flex; justify-content: center; gap: 20px; margin-top: 20px; padding: 15px; border-top: 1px solid #eee;">
  <div style="text-align: center;">
    <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#2d6a4f" stroke-width="2">
      <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
      <path d="M9 12l2 2 4-4"/>
    </svg>
    <p style="font-size: 11px; margin-top: 4px; color: #555;">CGMP Certified</p>
  </div>
  <div style="text-align: center;">
    <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#2d6a4f" stroke-width="2">
      <path d="M9 12l2 2 4-4"/>
      <circle cx="12" cy="12" r="10"/>
    </svg>
    <p style="font-size: 11px; margin-top: 4px; color: #555;">Clinically Tested</p>
  </div>
  <div style="text-align: center;">
    <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#2d6a4f" stroke-width="2">
      <path d="M12 8v4l3 3"/>
      <circle cx="12" cy="12" r="10"/>
    </svg>
    <p style="font-size: 11px; margin-top: 4px; color: #555;">30-Day Guarantee</p>
  </div>
  <div style="text-align: center;">
    <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#2d6a4f" stroke-width="2">
      <rect x="1" y="3" width="22" height="18" rx="2" ry="2"/>
      <line x1="1" y1="9" x2="23" y2="9"/>
    </svg>
    <p style="font-size: 11px; margin-top: 4px; color: #555;">Free US Shipping</p>
  </div>
</div>
```

**Alternative (use image badges):** If you have brand-styled badge images, upload them to Shopify Files and reference them instead of inline SVGs.

---

#### ACTION 5: Feature testimonials on homepage

**Where to add:** In your homepage sections (Shopify Customize > Homepage), add a "Testimonials" section. If your theme doesn't have one, add this custom HTML via a "Custom Liquid" section:

```html
<section style="padding: 60px 20px; background: #f8f9fa; text-align: center;">
  <h2 style="font-size: 28px; margin-bottom: 40px;">What Our Customers Say</h2>
  <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 30px; max-width: 1000px; margin: 0 auto;">

    <div style="flex: 1; min-width: 280px; max-width: 320px; background: white; padding: 30px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08);">
      <div style="color: #f5a623; font-size: 20px; margin-bottom: 10px;">★★★★★</div>
      <p style="font-style: italic; color: #333; margin-bottom: 15px;">"I take 2 capsules every morning before my outdoor runs. My skin feels so much more protected even on long trail days."</p>
      <p style="font-weight: bold; color: #555;">— Sarah M., Phoenix AZ</p>
    </div>

    <div style="flex: 1; min-width: 280px; max-width: 320px; background: white; padding: 30px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08);">
      <div style="color: #f5a623; font-size: 20px; margin-bottom: 10px;">★★★★★</div>
      <p style="font-style: italic; color: #333; margin-bottom: 15px;">"Switched from Heliocare and love the complete formula. PLE plus all the vitamins in one pill is exactly what I wanted."</p>
      <p style="font-weight: bold; color: #555;">— Michael T., San Diego CA</p>
    </div>

    <div style="flex: 1; min-width: 280px; max-width: 320px; background: white; padding: 30px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08);">
      <div style="color: #f5a623; font-size: 20px; margin-bottom: 10px;">★★★★★</div>
      <p style="font-style: italic; color: #333; margin-bottom: 15px;">"As someone with rosacea, sun sensitivity is a real issue. This has been a game changer for my daily routine."</p>
      <p style="font-weight: bold; color: #555;">— Jennifer L., Las Vegas NV</p>
    </div>

  </div>
</section>
```

**Important:** Replace these placeholder testimonials with REAL customer reviews once you have them. Using fabricated testimonials is an FTC violation. These are templates showing the format only — collect real reviews first.

---

#### ACTION 6: Create a "Results" page

**Where to create:** Shopify Admin > Online Store > Pages > Add page

**Page handle:** `/pages/results`
**Title:** "Real Results from 360 Sun Shield Customers"
**Meta description:** "See how 360 Sun Shield customers are protecting their skin from UV rays naturally. Real stories, real results from daily sun protection supplement users."

**Page content template:**

```html
<h1>Real Results from 360 Sun Shield Customers</h1>

<p>We asked our customers to share their experience with 360 Sun Shield.
Here's what they told us.</p>

<div class="result-story">
  <h3>"[Customer Name]" — [Location]</h3>
  <p><strong>How long using 360 Sun Shield:</strong> [X months]</p>
  <p><strong>Why they started:</strong> [Their reason]</p>
  <p><strong>Their experience:</strong> [2-3 sentence quote]</p>
</div>

<!-- Repeat for each customer story -->

<div style="text-align: center; margin-top: 40px; padding: 30px; background: #f0f7ff; border-radius: 8px;">
  <h2>Want to Share Your Results?</h2>
  <p>Email us at [email] with your 360 Sun Shield story.</p>
  <a href="/products/360-sun-shield" style="display: inline-block; padding: 12px 30px; background: #2d6a4f; color: white; text-decoration: none; border-radius: 5px; margin-top: 10px;">Try 360 Sun Shield</a>
</div>
```

---

## 9. Top 10 Priority Actions (Ranked by Impact)

| # | Action | Impact | Effort | Timeline | Implementation |
|---|--------|--------|--------|----------|----------------|
| 1 | **Add Product schema** to product page | HIGH | Low | 1 day | See Section 2: create snippet `product-schema` via Themes > Edit Code > Snippets, render from `theme.liquid` |
| 2 | **Write meta descriptions** for homepage, product, science, all 47 blogs | HIGH | Medium | 1 week | See Section 2: all 7 key page descriptions written; blog posts need individual descriptions |
| 3 | **Add customer reviews** to product page (install review app) | HIGH | Low | 1 week | See Section 8: Judge.me setup steps + customer email template |
| 4 | **Create Amazon listing** | VERY HIGH | Medium | 2 weeks | See Section 6 Month 1: step-by-step Amazon Seller Central instructions |
| 5 | **Add FAQ schema** to FAQs page | HIGH | Low | 1 day | See Section 2: complete FAQPage JSON-LD with 7 Q&A pairs |
| 6 | **Write "360 Sun Shield vs Heliocare" comparison page** | HIGH | Medium | 1 week | See Section 5 Content Gap 1: full outline + comparison table HTML + FAQ schema |
| 7 | **Add Open Graph tags** to all pages | MEDIUM | Low | 1 day | See Section 2: complete dynamic Liquid template for OG + Twitter cards |
| 8 | **Add Article schema** to all 47 blog posts | HIGH | Medium | 1 week | See Section 2: dynamic `article-schema.liquid` snippet (one file covers all posts) |
| 9 | **Pitch to 5 "best supplement" listicle authors** | VERY HIGH | High | 1 month | See Section 6 Month 2: exact email templates for bloggers, dermatologists, guest posts |
| 10 | **Add author bio + dates to blog posts** (E-E-A-T) | MEDIUM | Low | 1 week | See below |

### Action 10 Implementation: Add Author Bio + Publication Dates

**Where to add:** Edit your `article.liquid` or `sections/article-template.liquid` template.

**Add this block right after the `<h1>` article title:**

```html
<div class="article-meta" style="display: flex; align-items: center; gap: 15px; margin: 15px 0 30px; padding-bottom: 15px; border-bottom: 1px solid #eee;">
  <img src="https://360sunshield.com/cdn/shop/files/Group_1000001781.png"
       alt="360 Sun Shield"
       width="40" height="40"
       style="border-radius: 50%; object-fit: cover;">
  <div>
    <p style="margin: 0; font-weight: 600; font-size: 14px;">360 Sun Shield Team</p>
    <p style="margin: 0; color: #666; font-size: 13px;">
      <time datetime="{{ article.published_at | date: '%Y-%m-%d' }}">
        {{ article.published_at | date: '%B %d, %Y' }}
      </time>
      {% if article.updated_at != article.published_at %}
        · Updated {{ article.updated_at | date: '%B %d, %Y' }}
      {% endif %}
      · {{ article.content | strip_html | split: ' ' | size }} minute read
    </p>
  </div>
</div>
```

**For stronger E-E-A-T, create an author page** at `/pages/about-our-team` and link to it:

```html
<a href="/pages/about-our-team" style="color: #2d6a4f; text-decoration: none; font-weight: 600;">
  360 Sun Shield Team
</a>
```

---

## 10. 12-Week Execution Plan (Adapted for E-Commerce)

| Week | Focus | Deliverables |
|------|-------|-------------|
| 1 | Technical fixes | Meta descriptions (Section 2), OG tags (Section 2), canonical verification (Section 2), Product schema (Section 2), FAQ schema (Section 2) |
| 2 | Reviews + social proof | Install Judge.me (Section 8 Action 1), email past customers (Section 8 Action 3), add trust badges (Section 8 Action 4) |
| 3 | Schema blitz | Article schema on all 47 posts (Section 2), BreadcrumbList (Section 7), AggregateRating once reviews exist (Section 2) |
| 4 | Amazon launch | Create Amazon listing (Section 6 Month 1 #1), optimize listing SEO, set up FBA |
| 5 | Content creation | "vs Heliocare" comparison (Section 5 Gap 1), "Best PLE Supplement 2026" listicle (Section 5 Gap 2), buyer's guide (Section 5 Gap 3) |
| 6 | Blog optimization | Rewrite titles/metas for all 47 posts, add internal links to product page, add author bios (Action 10) |
| 7 | Backlink campaign | Send listicle pitch emails (Section 6 Month 2 #6), submit to supplement directories (Section 6 Month 1 #4) |
| 8 | Niche content | Athlete content (Section 5 Gap 4), dermatologist content (Section 5 Gap 5), rosacea/lupus optimization |
| 9 | PR push | HARO/Connectively setup (Section 6 Month 2 #9), podcast pitching (Section 6 Month 2 #10), local Vegas press (Section 6 Month 3 #15) |
| 10 | E-E-A-T strengthening | Dermatologist partnership (Section 6 Month 3 #14), clinical citations page, author pages |
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
