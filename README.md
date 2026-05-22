# claude-seo-skill

A Claude Code skill that runs a 10-step technical SEO audit on small business websites. Type `/seo` in any project and Claude reads your HTML files, computes contrast ratios, checks your sitemap, validates schema, and outputs a prioritised report with concrete fixes.

Free and open source. No paid tools required. Works on any static or server-rendered site.

---

## What it audits

| Step | Check |
|------|-------|
| 1 | Sitemap and robots.txt — are your pages crawlable? |
| 2 | Title tag — 50–60 chars, keyword first |
| 3 | Meta description — 140–155 chars, clear intent |
| 4 | Heading hierarchy — one H1 per page, keyword included |
| 5 | Schema / structured data — Organization, Service, LocalBusiness |
| 6 | Internal links — orphaned pages, generic anchor text |
| 7 | Images — missing alt text, no lazy loading |
| 8 | Colour contrast — WCAG AA formula, opacity blending |
| 9 | Page speed signals — render-blocking scripts, missing dimensions |
| 10 | Search intent match — does the page actually answer the query? |

Findings are output as **Critical / High / Medium / Low** with a concrete fix for each.

---

## Install

**Requirements:** [Claude Code](https://claude.ai/code) installed and authenticated.

1. Create the skills directory if it doesn't exist:
   ```bash
   mkdir -p ~/.claude/skills/seo
   ```

2. Download the skill file:
   ```bash
   curl -o ~/.claude/skills/seo/SKILL.md \
     https://raw.githubusercontent.com/tradewithmeai/claude-seo-skill/main/SKILL.md
   ```

3. In any Claude Code session, type:
   ```
   /seo
   ```

That's it. Claude will ask which pages to audit, or you can specify them directly: `/seo run audit on index.html and about.html`.

---

## Example output

```
## SEO Audit: homepage (index.html)
**Primary keyword target**: "plumber in Bristol"

### Critical (fix before anything else)
- No sitemap.xml found — Google is discovering your pages by luck, not by design.
  Fix: create sitemap.xml listing all public pages and add Sitemap: directive to robots.txt.

### High (fix this week)
- Title is 72 chars and truncated in Google SERPs — "Joe Smith Plumbing — Emergency Plumber Bristol Certified Gas Safe"
  Fix: shorten to "Emergency Plumber Bristol | Joe Smith" (38 chars, keyword first).
- Meta description missing on services.html
  Fix: add <meta name="description" content="..."> (140–155 chars, include location and CTA).

### Medium (fix this month)
- No schema markup found
  Fix: add LocalBusiness schema to homepage (template in SKILL.md).

### Already good
- H1 present and contains primary keyword on all pages checked.
- All images have alt text.
- robots.txt present, Googlebot not blocked.
```

---

## Contributing

Found a check that should be in the skill? Improved the contrast formula? Spotted a pattern specific to a platform (WordPress, Squarespace, Framer)?

- **Raise an issue** to suggest a new check or report a false positive
- **Open a PR** against `SKILL.md` with your improvement
- **Share your audit findings** — if you ran this on your site and found something the skill missed, that's the most valuable contribution

The skill gets better with real-world use cases. All contributions welcome.

---

## Background

This skill was written for [solvX.uk](https://solvx.uk) as part of a public SEO experiment — fixing a site's search visibility from scratch and documenting every step. The full case study, with live traffic data, is at [solvx.uk/seo-lab.html](https://solvx.uk/seo-lab.html).

The skill is generalised here so anyone can use it on their own site.

---

## Licence

MIT — use it, fork it, improve it.
