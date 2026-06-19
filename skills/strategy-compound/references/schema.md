# strategy-insights — frontmatter schema

Each insight is ONE markdown file at `docs/strategy-insights/<category>/<slug>.md`.

## Frontmatter (YAML)
```yaml
title: [clear one-line title]
date: [YYYY-MM-DD]            # pass the real date in; never guess
category: [see categories]
client: [client/company slug, or "general"]
problem_type: [see problem types]
horizon: [now | h1 | h2 | h3]   # Three-Horizons placement, optional
confidence: [high | medium | low]   # how proven is this insight
tags: [up-to-6, lowercase-hyphenated]
```

## Categories (the `docs/strategy-insights/` subdirectories)
`market-analysis · competitive-landscape · positioning · pricing · go-to-market ·
operating-model · customer-loyalty · board-narratives · blue-ocean · facilitation`

## problem_type (enum)
`market_read · competitive_position · differentiation · pricing_model · gtm ·
operating_model · loyalty_retention · org_alignment · narrative · risk_assumption ·
blue_ocean_opportunity · facilitation_pattern`

## Body sections
```markdown
# [Title]

## Context
[What case/situation produced this. Keep client-anonymous if the repo is public.]

## Insight
[The reusable strategic finding — what we now know.]

## Why it matters
[The consequence / what it changes.]

## When to apply
- [Conditions under which this insight is relevant to a future run]

## Evidence / caveats
[What it rests on, and where it might NOT hold. Strategy has no test suite — be honest about confidence.]

## Related
- [links to other insight files]
```

## Rules
- One insight per file. Update an existing file (add `last_updated: YYYY-MM-DD`) instead of duplicating
  when the new learning clearly overlaps an existing one.
- Anonymize for public repos: no client secrets, no named individuals.
- Confidence is mandatory and honest — a strategy insight is a bet, not a proof.
