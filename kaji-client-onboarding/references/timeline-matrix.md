# Deployment Timeline Estimation Matrix

Use this matrix to calculate deployment timelines based on onboarding intake answers.

## Base Setup

| Item | Days | Notes |
|------|------|-------|
| Base Kaji deployment (single MaaS model) | 1 | Includes agent config, model connection, basic testing |

## Section 1: Model Requirements

| Item | Days | Classification |
|------|------|----------------|
| First MaaS model (Opus/GPT/Gemini) | 0 | Included in base — Must-Have |
| Each additional MaaS model | 0.5 | Must-Have |
| Local model setup + testing | 3 | Nice-to-Have (unless no MaaS) |
| Local model performance tuning | 2 | Nice-to-Have |

## Section 2: API Key Configuration

| Item | Days | Classification |
|------|------|----------------|
| Client uses own API key | 0.5 | Must-Have (key validation + config) |
| Company-provided key provisioning | 1 | Must-Have (key generation + rotation policy) |
| Dual key setup (dev + prod) | 1.5 | Must-Have if selected |

## Section 3: Use Cases

Use cases don't directly add days — they inform which integrations and skills are needed.
However, complex or unusual use cases may require custom configuration:

| Item | Days | Classification |
|------|------|----------------|
| Standard use cases (code gen, review, debug) | 0 | Covered by base setup |
| DevOps / CI/CD automation | 1 | Must-Have if selected |
| Data analysis & querying | 1 | Must-Have if selected |
| Custom/specialized use case | 2-5 | Depends on complexity |

## Section 4: Integrations (Existing)

### Built-in Skills (no additional setup)

| Item | Days | Classification |
|------|------|----------------|
| Playwright | 0 | Included — Must-Have |
| Frontend UI/UX | 0 | Included — Must-Have |
| Git Master | 0 | Included — Must-Have |
| Dev Browser | 0 | Included — Must-Have |

### MCP Integrations

| Item | Days | Classification |
|------|------|----------------|
| Supabase | 1 | Must-Have — DB connection + schema review |
| Dremio | 1 | Must-Have — Connection + permissions |
| Neo4j | 1 | Must-Have — Graph setup + index config |
| Graphiti Memory | 0.5 | Must-Have — Already deployed, needs graph init |
| Context7 | 0 | Included — no setup needed |
| Mattermost | 0.5 | Must-Have — Channel config + permissions |
| Fireflies | 1 | Must-Have — API key + meeting sync |
| ClickUp | 1 | Must-Have — Workspace auth + permissions |
| Notion | 1 | Must-Have — Integration token + page access |
| Tavily Search | 0.5 | Must-Have — API key config |
| Exa Web Search | 0.5 | Must-Have — API key config |
| Grep.app | 0 | Included — no setup needed |
| Shakudo Platform | 0 | Included — native integration |
| Gamma | 0.5 | Must-Have — API key config |
| NanoBanana | 0.5 | Must-Have — API key + model config |
| MarkItDown | 0 | Included — no setup needed |

## Section 5: New Integration Requests

| Item | Days | Classification |
|------|------|----------------|
| Simple API integration (REST, well-documented) | 5 | Nice-to-Have |
| Complex integration (OAuth, webhooks, custom logic) | 10 | Nice-to-Have |
| Enterprise integration (SAML, SCIM, custom connector) | 15 | Nice-to-Have |

## Section 6: Communication Channel

| Item | Days | Classification |
|------|------|----------------|
| Mattermost | 0 | Included — Must-Have |
| CLI / Terminal | 0 | Included — Must-Have |
| Slack integration | 3 | Nice-to-Have |
| Microsoft Teams integration | 5 | Nice-to-Have |
| Custom channel | 5-10 | Nice-to-Have |

## Section 7: Security & Guardrails

| Item | Days | Classification |
|------|------|----------------|
| Approval-required for git operations | 0 | Included in base — Must-Have |
| No access to production environments | 0.5 | Must-Have — path exclusion config |
| Approval-required for all file changes | 1 | Must-Have if selected |
| Restricted file/directory access | 1 | Must-Have if selected |
| Read-only mode | 1.5 | Nice-to-Have |
| Audit logging of all agent actions | 3 | Nice-to-Have |
| Custom guardrails (rule engine) | 2-4 | Nice-to-Have — depends on complexity |

## Quick Estimation Guide

### Small Deployment (1-2 developers, simple use case)
- 1 MaaS model + own API key + Git + Mattermost
- **Estimate: 2-3 business days**

### Medium Deployment (team of 5-10, multiple integrations)
- 2 MaaS models + company keys + 3-4 integrations + basic security
- **Estimate: 5-8 business days**

### Large Deployment (enterprise, full stack)
- Multiple models + all integrations + Slack/Teams + custom security + audit logging
- **Estimate: 15-25 business days**

### Formula

```
Total Days = Base (1)
           + Model additions
           + API key setup
           + Sum(selected integrations)
           + Communication channel setup
           + Security setup
           + 20% buffer (round up)
```

Always round the final number UP to the nearest whole business day.
