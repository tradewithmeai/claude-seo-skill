# Agent instructions — claude-seo-skill repo

This repo contains one file that matters: `SKILL.md`. Everything else supports it.

## What you are working on

`SKILL.md` is a Claude Code skill file. It is read by Claude Code when the user types `/seo`. It must remain in valid skill file format:

```
---
name: seo
description: [one-line description used for skill matching]
---

[skill body — markdown]
```

Do not add YAML fields beyond `name` and `description`. Do not change the frontmatter format.

## Rules for editing SKILL.md

- Every audit step must have: a name, detection method (how to find the issue from HTML/CSS source), pass/fail criteria, output format, and a fix template.
- Schema templates must only include fields that are universally safe to add. No `aggregateRating`, `award`, `numberOfEmployees`, or any field that requires verified external data.
- The contrast formula in Step 8 is mathematically precise. Do not simplify it. If you improve it, verify the formula produces correct results for these known cases before committing:
  - `#ffffff` on `#000000` → 21:1
  - `#777777` on `#ffffff` → 4.48:1 (fails AA normal text)
  - `rgba(241,241,241,0.4)` on `#000a1a` → approx 3.3:1 (fails AA)
  - `rgba(241,241,241,0.65)` on `#000a1a` → approx 7.1:1 (passes AA)
- The output contract (the `## SEO Audit:` format) must not change. Downstream tooling may parse it.
- Do not add step numbers beyond 10 without renumbering. If a new check belongs in an existing step, add it there.

## Contribution workflow

1. Read the existing `SKILL.md` in full before making any changes.
2. Identify which step your change belongs to, or propose a new step.
3. Make the minimum edit required. Do not rewrite surrounding content.
4. After editing, verify the frontmatter is intact and the file starts with `---`.
5. Update `README.md` audit steps summary table if you added a new step.
6. Commit with a message in this format: `[step N] brief description of change`

## What not to do

- Do not add prose explanation of what Claude Code is — that belongs in README.md, not SKILL.md.
- Do not add examples of bad SEO to SKILL.md. The skill tells the agent what to look for; it does not need training data.
- Do not remove the Honest Framing section. It prevents the skill from making false promises.
- Do not add platform-specific instructions (WordPress, Shopify, etc.) to the main skill body. Add them as a separate appendix section at the bottom of SKILL.md, clearly labelled.
