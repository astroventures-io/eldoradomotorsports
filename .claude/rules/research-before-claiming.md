---
description: "When Claude is about to make claims about industry practices, pricing norms, competitor behavior, or market standards"
globs:
  - "**"
---

# Research Before Claiming — No Unverified Business Claims

When making claims about industry practices, pricing, competitor behavior, or market norms:

1. **NEVER present opinions or assumptions as facts.** Phrases like "industry standard is..." or "most companies do..." require actual research.
2. **Load relevant skills first** via `mcp__av-workspace__skill_discovery_load`: `brand-marketing:pricing-strategy`, `brand-marketing:competitor-alternatives`, `market-research-reports`, whichever fits the question.
3. **Do the research:** Use WebSearch/WebFetch to verify claims with real data from real companies. Cite sources.
4. **If you can't verify it, say so.** "I believe X but haven't verified" is honest. "X is the industry standard" without research is not.

## Why

An orbitcleaning session confidently stated "industry standard is full upfront payment" for cleaning services without any research. When challenged, Claude admitted it was guessing. The actual competitive landscape (Handy vs MaidThis vs Turno vs local Bay Area companies) showed 3 distinct models — not one "standard."

## Applies To

- Pricing decisions, payment models, billing strategies
- Competitor analysis, market positioning
- Legal/compliance claims (load Lyra skills)
- Technology choices framed as "best practice"
- Any "most companies do X" or "the standard approach is Y" statement
