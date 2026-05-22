---
name: seo
description: Audit and improve SEO on small business websites. Use when the user asks for an SEO review, wants to improve search rankings, or wants to fix meta tags, structured data, contrast, or page speed. Performs page-by-page audits, outputs a prioritised findings report, and edits HTML directly when authorised.
---

# SEO Skill for Claude Code

A 10-step technical SEO audit for small business websites. Runs as a Claude Code skill — type `/seo` in any project to start an audit of your HTML pages.

SEO for a small business site is a 2–6 month compounding investment. Technical fixes (titles, schema, sitemap) take effect within weeks of Google re-crawling. Content and backlinks drive gains over months. This skill handles the technical layer.

---

## When to invoke

- "Why aren't we getting traffic?"
- "Can you do an SEO audit?"
- "Fix our meta tags / structured data"
- "Check our page speed / Core Web Vitals signals"
- "Help us rank for [keyword]"
- Any request mentioning SEO, search, Google, schema, sitemap, robots.txt

---

## Audit Workflow

Run these steps in order for each page under audit. State which page you're auditing at the start.

### Step 1 — Crawlability Check
- Read `sitemap.xml` and `robots.txt` (if they exist)
- Verify the page is listed in the sitemap; flag if sitemap is missing entirely
- Confirm robots.txt does not block the page or Googlebot

### Step 2 — Title Tag
- Read `<title>` from the HTML
- **Good**: 50–60 chars, primary keyword near the front, brand name at end
- **Bad**: generic (brand name only), duplicate across pages, over 60 chars (truncated in SERPs), empty
- Target keyword should match what a real customer would search — not internal jargon
- Example good title: `"Business Automation for UK SMEs | YourBrand"`

### Step 3 — Meta Description
- Read `<meta name="description">` from the HTML
- **Good**: 140–155 chars, answers the searcher's intent, includes a soft CTA ("Learn more", "Book a call")
- **Bad**: missing, over 155 chars (truncated), duplicate across pages, keyword-stuffed
- Meta description does not directly affect ranking but affects click-through rate

### Step 4 — Heading Hierarchy
- Read all `<h1>`, `<h2>`, `<h3>` tags
- Each page: exactly one `<h1>` that contains the primary keyword
- `<h2>` tags: section labels that match secondary search intent
- No skipped levels (h1 → h3 with no h2); no decorative headings that dilute keyword signal

### Step 5 — Schema / Structured Data
- Read any `<script type="application/ld+json">` blocks
- Check for: Organization (site-wide), Service (per service page), Person (practitioner E-E-A-T)
- Validate against templates below — never fabricate credentials, awards, addresses, or employee counts
- Missing schema is a medium-priority fix; wrong schema (fake claims) is a critical risk

### Step 6 — Internal Links
- Count internal links on the page; flag pages with zero inbound links from other pages
- Anchor text should be descriptive ("business automation services"), not generic ("click here")
- Each service page should link to: homepage, at least one other service, contact section

### Step 7 — Images
- Check all `<img>` tags for: `alt` attribute (non-empty, descriptive), `loading="lazy"` on below-fold images
- File names should be descriptive (`plumber-bristol.jpg` not `img123.jpg`) — flag if not
- Missing alt on content images is a WCAG + SEO fail; `alt=""` is correct for decorative images

### Step 8 — Colour Contrast (Accessibility + Page Experience)

Google's page experience signals include accessibility. WCAG AA contrast ratios are the minimum bar.

**How to audit from source code:**
1. Find all text colour + background colour pairs in the CSS (`:root` variables, inline styles, class definitions)
2. Compute the contrast ratio using the WCAG formula (see below)
3. Flag any pair that fails the threshold for its text size

**WCAG AA thresholds:**
- Normal text (< 18pt / < 14pt bold): minimum **4.5:1**
- Large text (≥ 18pt / ≥ 14pt bold): minimum **3:1**
- UI components and icons: minimum **3:1**

**Contrast ratio formula:**
```
ratio = (L1 + 0.05) / (L2 + 0.05)
where L1 = lighter luminance, L2 = darker luminance

Relative luminance of a hex colour:
  for each channel c in [R, G, B]:
    sRGB = c / 255
    linear = sRGB / 12.92                    if sRGB <= 0.04045
           = ((sRGB + 0.055) / 1.055) ^ 2.4  otherwise
  L = 0.2126*R_linear + 0.7152*G_linear + 0.0722*B_linear
```

**Common fail patterns to check on every page:**
- Light text on a light card/section background
- Dark text on a dark page background
- Muted text via `opacity` — opacity reduces effective contrast; compute the blended colour against the actual background before checking the ratio
- `color: #888` or similar mid-greys — passes on white but can fail on tinted dark backgrounds
- Inline `style="color: ..."` overrides that conflict with the section background

**Opacity blending formula** (when text has opacity < 1 on a solid background):
```
effective_channel = (text_channel × opacity) + (bg_channel × (1 - opacity))
```
Compute contrast ratio of the effective colour against the background.

**Output format for contrast findings:**
```
Contrast issue: "[element/selector]" — [text colour] on [bg colour] = [ratio]:1 (need [threshold]:1)
Fix: change [property] to [suggested value] (computed ratio: [X]:1)
```

### Step 9 — Page Speed / Core Web Vitals Signals
- Check for: render-blocking scripts in `<head>` without `defer` or `async`
- Check for: large inline `<style>` blocks that could be cached as external CSS
- Check for: images without explicit `width`/`height` (causes CLS — Cumulative Layout Shift)
- Check for: Google Fonts or external font CDN calls (add `preconnect` if present)
- Flag obvious issues only — do not run Lighthouse (no browser access)

### Step 10 — Search Intent Match
- What is the primary keyword this page targets? State it explicitly.
- Does the page content actually answer that query? (If someone searched the keyword and landed here, would they find what they need?)
- Mismatch between keyword and content = page will not rank regardless of technical fixes

---

## Output Format

Always output findings in this structure. Omit categories with zero findings.

```
## SEO Audit: [page name] ([file path])
**Primary keyword target**: [one phrase]

### Critical (fix before anything else)
- [Finding] — [Why it matters] — Fix: [concrete action]

### High (fix this week)
- [Finding] — [Why it matters] — Fix: [concrete action]

### Medium (fix this month)
- [Finding] — [Why it matters] — Fix: [concrete action]

### Low / Nice to have
- [Finding] — [Why it matters] — Fix: [concrete action]

### Already good
- [List what's working — be specific]
```

---

## Editing Rules

When authorised to make edits directly:

1. **No keyword stuffing** — do not repeat a keyword more than naturally fits. Google penalises this.
2. **No fake schema claims** — do not add `"award"`, `"aggregateRating"`, accreditation bodies, or employee counts that are not verified and documented.
3. **No fabricated E-E-A-T signals** — do not invent qualifications, years of experience, or client counts.
4. **Title/meta changes only** — do not rewrite page body copy without explicit user approval. SEO copy changes affect user experience and brand voice.
5. **State before editing** — say what you're changing and why before making the edit. Do not batch silent edits across multiple files.
6. **Preserve existing structure** — do not alter HTML structure, class names, or JS behaviour when fixing meta tags or schema. Edit only what's needed.

---

## Schema Templates

Use these exact templates. Fill in only what is true and verifiable. Leave out fields you cannot verify.

### Organization (add to the homepage — site-wide signal)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Your Business Name",
  "url": "https://yourdomain.com",
  "email": "hello@yourdomain.com",
  "sameAs": [
    "https://twitter.com/yourhandle",
    "https://linkedin.com/company/yourcompany"
  ]
}
```

### Service (add to each service sub-page)
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "[Service Name]",
  "provider": {
    "@type": "Organization",
    "name": "Your Business Name",
    "url": "https://yourdomain.com"
  },
  "areaServed": {
    "@type": "Country",
    "name": "United Kingdom"
  },
  "description": "[One sentence, plain English, matches page copy]"
}
```

### LocalBusiness (for businesses with a physical location or area)
```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Your Business Name",
  "url": "https://yourdomain.com",
  "telephone": "+44XXXXXXXXXX",
  "areaServed": "Bristol, UK",
  "description": "[One sentence description]"
}
```

### Person — E-E-A-T practitioner signal
```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Your Name",
  "url": "https://yourdomain.com",
  "worksFor": {
    "@type": "Organization",
    "name": "Your Business Name"
  }
}
```
Only add this if the page features the practitioner prominently (e.g. about page, service page).

---

## Sitemap & Robots.txt

If `sitemap.xml` does not exist, create one:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://yourdomain.com/</loc><changefreq>weekly</changefreq><priority>1.0</priority></url>
  <url><loc>https://yourdomain.com/services.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://yourdomain.com/about.html</loc><changefreq>monthly</changefreq><priority>0.7</priority></url>
</urlset>
```

If `robots.txt` does not exist:
```
User-agent: *
Allow: /
Sitemap: https://yourdomain.com/sitemap.xml

Disallow: /admin/
Disallow: /private/
```

---

## UK Local SEO Checklist

Run this when the user asks about local search or "near me" queries.

- [ ] Google Business Profile — created and verified? (Requires real business address)
- [ ] NAP consistency — Name, Address, Phone identical on site, GBP, and any directories
- [ ] Phone number visible in HTML text (not just an image)
- [ ] Footer contains city/region name if targeting a specific area
- [ ] `areaServed` in schema matches actual service area
- [ ] Registered on UK directories: Yell, Yelp UK, Trustpilot (free tier), FreeIndex

---

## Honest Framing

Tell the user this when starting any SEO engagement:

> SEO compounds over 2–6 months. Technical fixes (titles, schema, sitemap) are table stakes — they take effect within weeks of Google re-crawling. Content and backlinks drive ranking gains over months. The immediate priority is technical correctness so the site is crawlable and indexable, then building even one or two inbound links from relevant sites.

Do not promise ranking positions. Do not suggest paid tools unless asked. Focus on actions the user can take today with zero budget.
