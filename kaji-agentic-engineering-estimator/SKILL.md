---
name: kaji-agentic-engineering-estimator
description: "Estimate Kaji Agentic Engineering engagements. Takes client use cases as input, classifies complexity, calculates dev streams needed, and outputs a full estimate with pricing, traditional comparison, savings percentage, and timeline. Supports all three engagement models: Shakudo Agentic Team, Co-Development, and Client-Led."
license: MIT
metadata:
  author: robert-shakudo
  version: "1.0"
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

Use this complexity classification (round **up** when in doubt):

| Complexity | Signals | Dev Streams | Price Per Stream |
|-----------|---------|-------------|-----------------|
| Simple | Single integration, dashboard, prototype, chatbot, automation workflow, single API | 1 | $8K-15K |
| Medium | Multi-source pipeline, ML model, copilot, multi-step automation, API platform, multi-integration | 2-3 | $10K-15K |
| Complex | Full product, multi-system build, production deployment with scaling, data platform, enterprise app | 4-6 | $10K-12K (volume) |

### Step 3: Calculate Traditional Estimate

- Simple: **$50-100K**, **2-3 months**
- Medium: **$150-300K**, **3-6 months**
- Complex: **$300K-700K+**, **6-12+ months**

Sum across use cases for a total traditional range. Use the pricing matrix for guidance on combining multiple use cases.

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
- Reference proven engagements when a use case is similar (QuadReal for data/ML, Hitachi for agent platforms, Gallo for enterprise apps, SPP for data pipelines, Annexus for automation + Salesforce, MCP for AI assistants)
- Do **not** show per-stream pricing breakdown to the client (internal only)

## Proven Engagement Benchmarks (Calibration Only)

| Client | Scope | Streams | Traditional | Kaji |
|--------|-------|---------|------------|------|
| QuadReal | 4 data sources, 3 ML models, 3 copilots, 4 dashboards | 3 | $700K | $300K |
| Hitachi | AI Agent Hub vs Copilot | 4 | $300K | $40K |
| MCP | Email assistant, HR docs, vibe coding, DMR reports | 4 | $200-300K | $50K |
| SPP | Purchasing optimization platform, 5 integrations | 5 | $400K | $60K |
| Gallo | Expense Express, GalloGPT, dev tools, n8n | 4 | $350K | $55K |
| Annexus | Pre-sales automation (4 stages), chatbot, Kaji deployment | 6 | $400K | $65K |

## Anti-Patterns

- Don't show the client the per-stream price breakdown (internal)
- Don't estimate without understanding what each use case actually involves
- Don't skip the complexity classification — it drives everything
- Don't commit to a price without Robert's review

## Integration

- **ClickUp** — Create estimate task for tracking
- **Notion** — Reference the Kaji Agentic Engineering blueprint page
- **Mattermost** — Share estimates in client channels (internal only until approved)

## References

- [Pricing Matrix](./references/pricing-matrix.md)
- [Complexity Guide](./references/complexity-guide.md)
- [Estimate Template](./assets/estimate-template.md)

---

## Skill Metadata

**Created**: 2026-02-19
**Last Updated**: 2026-02-19
**Author**: Robert @ Shakudo
**Version**: 1.0
