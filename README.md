# claude-seo-skill

A Claude Code skill that performs a 10-step technical SEO audit on small business websites. Reads HTML files directly, computes contrast ratios, checks sitemaps, validates schema markup, and outputs a prioritised findings report with concrete fixes.

---

## AGENT INSTRUCTIONS

This repository contains a single Claude Code skill. The canonical file is `SKILL.md`. Everything below describes how to install, invoke, and extend it.

### Repository structure

```
claude-seo-skill/
├── SKILL.md       ← The skill. Install this file. Do not modify for per-project use.
├── CLAUDE.md      ← Instructions for agents working inside this repo (contributors).
└── README.md      ← This file.
```

### Install path

```
~/.claude/skills/seo/SKILL.md
```

This is the global Claude Code skills directory. Skills installed here are available in every project via `/seo`.

### Invocation

```
/seo
```

Or with arguments:

```
/seo run audit on index.html and services.html
/seo audit all pages
/seo check contrast on homepage
```

### Capability declaration

| Capability | Input | Output |
|---|---|---|
| Full 10-step audit | HTML file path(s) or glob | Prioritised findings report (Critical / High / Medium / Low) |
| Single-step audit | Step name + HTML path | Focused findings for that step only |
| Direct fix | Authorisation phrase + findings | Edited HTML files with stated changes |
| Sitemap generation | List of public page URLs | Valid sitemap.xml |
| Schema injection | Page type + business details | `<script type="application/ld+json">` block |
| Contrast check | CSS colour values (hex/rgba + opacity) | Pass/fail ratio with fix suggestion |

### Audit steps (summary)

```
1  Crawlability   — sitemap.xml, robots.txt, page listed, Googlebot not blocked
2  Title tag      — 50–60 chars, keyword-first, brand at end
3  Meta desc      — 140–155 chars, intent match, soft CTA
4  Headings       — exactly one H1 per page, keyword present, no skipped levels
5  Schema         — Organization / Service / LocalBusiness / Person; no fabricated fields
6  Internal links — orphaned pages, descriptive anchor text
7  Images         — alt text on content images, loading="lazy" below fold
8  Contrast       — WCAG AA formula + opacity blending; thresholds 4.5:1 / 3:1
9  Page speed     — render-blocking scripts, missing image dimensions, font preconnect
10 Intent match   — primary keyword stated; content answers the query
```

### Output contract

Every audit produces output in this exact structure:

```
## SEO Audit: [page name] ([file path])
**Primary keyword target**: [phrase]

### Critical
- [finding] — [why] — Fix: [action]

### High
### Medium
### Low / Nice to have
### Already good
```

Sections with zero findings are omitted.

### Editing rules (enforced when authorised to edit)

1. No keyword stuffing.
2. No fabricated schema fields (awards, ratings, employee counts, accreditations).
3. No invented E-E-A-T signals (qualifications, years of experience, client counts).
4. Title and meta edits only — no body copy rewrites without explicit approval.
5. State each change and its reason before editing.
6. Do not alter HTML structure, class names, or JS when fixing meta/schema.

---

## Contributing

Contributions are improvements to `SKILL.md`. A valid contribution is one of:

- A new audit check with: name, how to detect from source, pass/fail criteria, output format, fix template
- An improvement to an existing check: more precise detection logic, better fix template, reduced false positives
- A new schema template: must use only verifiable fields, no fabricated data
- A platform-specific appendix: WordPress, Squarespace, Framer, etc.

See `CLAUDE.md` for the contribution workflow an agent should follow when working inside this repo.

---

## Background

Built for [solvX.uk](https://solvx.uk) as part of a public SEO experiment documented at [solvx.uk/seo-lab.html](https://solvx.uk/seo-lab.html). Generalised here for any small business site.

MIT licence.
