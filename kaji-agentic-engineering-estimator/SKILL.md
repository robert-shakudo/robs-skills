---
name: kaji-agentic-engineering-estimator
description: "Estimate Kaji Agentic Engineering engagements. Takes client use cases as input, classifies complexity, calculates dev streams needed, and outputs a full estimate with pricing, traditional comparison, savings percentage, and timeline. Supports all three engagement models: Shakudo Agentic Team, Co-Development, and Client-Led."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.1"
  category: sales
---

# Kaji Agentic Engineering Estimator

Estimate client engagements for Kaji Agentic Engineering. Converts use cases into complexity tiers, dev stream counts, and pricing for quick estimates or full proposal-level summaries.

## When to Activate

- Someone asks to estimate, scope, or price a client engagement
- Someone says "estimate [client]", "how much would [use case] cost", or "scope this engagement"
- Pre-sales needs a pricing estimate or proposal numbers
- Any mention of dev stream pricing or engagement costing

## Core Concepts

This skill has **two modes**:

1. **Quick Estimate** — Given use cases, output a table with streams + pricing
2. **Full Proposal Estimate** — Detailed breakdown with traditional comparison, engagement model recommendation, and timeline

## Estimator Logic

### Step 1: Gather Input

Ask the team member (not the client) for:

- Client name
- List of use cases (short natural-language descriptions)
- Preferred engagement model (Shakudo Agentic Team, Co-Development, Client-Led) or "recommend"

If any use case is vague, clarify what systems, data sources, integrations, or outputs are involved.

### Step 2: Classify Each Use Case

See [Pricing Matrix](./references/pricing-matrix.md) for the full complexity classification table, per-stream pricing, and engagement model multipliers. Round **up** when in doubt.

**Summary:**
- Simple (1 stream, $8K–15K) — single integration, dashboard, chatbot, prototype
- Medium (2–3 streams, $10K–15K) — multi-source pipeline, ML model, copilot, multi-step automation
- Complex (4–6 streams, $10K–12K volume) — full product, multi-system build, enterprise app

### Step 3: Calculate Traditional Estimate

- Simple: **$50-100K**, **2-3 months**
- Medium: **$150-300K**, **3-6 months**
- Complex: **$300K-700K+**, **6-12+ months**

Sum across use cases for a total traditional range.

### Step 4: Calculate Kaji Price

1. Sum all **dev streams × price per stream** (pick a defensible price within the range)
2. Apply engagement model modifier:
   - **Shakudo Agentic Team** (full delivery): base price
   - **Co-Development**: base × **0.85**
   - **Client-Led with Kaji**: base × **0.5**
3. Apply volume rules for **5+ streams** (see Pricing Matrix)

### Step 5: Generate Output

Use the full template in [assets/estimate-template.md](./assets/estimate-template.md). The output must include:

- Use case breakdown with complexity, streams, and Kaji price
- Summary table with traditional vs Kaji cost
- Savings percentage
- Timeline summary
- Notes + engagement model recommendation

### Step 6: Offer Next Steps

- "Want me to create a proposal from this estimate?"
- "Want me to add this to ClickUp?"
- "Want me to adjust any pricing or scope?"

## Practical Guidance

- Always present the estimate to the team member for review **before** sharing with the client
- Pricing is a starting point — Robert adjusts final numbers based on dev stream complexity
- When in doubt, **round UP** on complexity (better to over-deliver than under-scope)
- Reference proven engagements when a use case is similar — see [Pricing Matrix](./references/pricing-matrix.md) for calibration benchmarks
- Do **not** show per-stream pricing breakdown to the client (internal only)

## Anti-Patterns

- Don't show the client the per-stream price breakdown (internal)
- Don't estimate without understanding what each use case actually involves
- Don't skip the complexity classification — it drives everything
- Don't commit to a price without Robert's review

## Error Handling

- Use case too vague to classify → ask for clarification on systems, data sources, integrations, and outputs involved
- No engagement model preference provided → default to Shakudo Agentic Team, note it in the estimate
- Pricing outside normal range → flag it for Robert's review before presenting

## Execution Checklist

- [ ] All use cases gathered and clarified
- [ ] Each use case classified by complexity tier (rounded up when uncertain)
- [ ] Dev streams calculated per use case
- [ ] Engagement model modifier applied
- [ ] Volume discount applied if 5+ streams
- [ ] Traditional estimate calculated for comparison
- [ ] Savings percentage calculated
- [ ] Estimate template filled out (assets/estimate-template.md)
- [ ] Presented to team member for review before sharing with client
- [ ] Next steps offered (proposal, ClickUp task, adjustments)

## Integration

- **ClickUp** — Create estimate task for tracking
- **Notion** — Reference the Kaji Agentic Engineering blueprint page
- **Mattermost** — Share estimates in client channels (internal only until approved)

## References

- [Pricing Matrix](./references/pricing-matrix.md) — Per-stream pricing, engagement model multipliers, volume rules, and calibration benchmarks
- [Complexity Guide](./references/complexity-guide.md)
- [Estimate Template](./assets/estimate-template.md)
