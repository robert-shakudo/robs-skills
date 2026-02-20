# Complexity Guide — Kaji Agentic Engineering

Use this guide to classify use cases consistently. If a use case spans multiple tiers, **round up**.

## Simple (1 stream)

**Signals**
- Single integration or single API
- Single workflow automation or chatbot
- Prototype or dashboard with limited data sources

**Examples**
- Single-source dashboard or reporting view
- One-step automation (e.g., ingest → summarize → deliver)
- Basic assistant with one knowledge base

## Medium (2–3 streams)

**Signals**
- Multi-source pipeline or data transformation flow
- ML model training/tuning or copilot with multiple tools
- Multi-step automation or API platform foundation

**Examples**
- Copilot that reads multiple systems and writes back to one
- Multi-step automation across 2–3 tools
- ML model with a production inference endpoint

## Complex (4–6 streams)

**Signals**
- Full product build or enterprise platform
- Multi-system data platform with deployment, scaling, and monitoring
- Production-grade app with security, audit, and high availability

**Examples**
- Enterprise data platform with ingestion, orchestration, and dashboards
- Multi-system app with production deployment and scaling requirements
- Full agent platform with governance + analytics

## Decision Tree

1. **Does it involve only one system or API?** → Simple
2. **Does it involve multiple systems or a multi-step flow?** → Medium
3. **Is it a production-grade product or platform with scaling/security?** → Complex
4. **Still unsure?** → Round up to the higher tier.

## Edge Cases

- **Single integration but strict enterprise security/compliance** → Medium
- **Prototype with multi-system orchestration** → Medium
- **Client expects production reliability from day one** → Complex
- **Use case includes both pipeline + app layer** → Complex

## Real Engagement Calibration (Reference)

- QuadReal (data sources + ML + copilots + dashboards) → Complex
- Hitachi (agent hub platform) → Complex
- SPP (multi-integration data platform) → Complex
- Annexus (multi-stage automation + deployment) → Complex
- MCP (assistant suite) → Medium/Complex depending on scope details
- Gallo (multi-product delivery) → Complex
