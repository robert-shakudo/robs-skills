# Robs Skills

Custom Kaji AI Agent skills for client onboarding, deployment, sales, and workflow automation.

## Skills

### kaji-agentic-engineering-estimator

Estimate Kaji Agentic Engineering engagements. Takes client use cases as input, classifies complexity (Simple/Medium/Complex), calculates dev streams needed, and outputs a full estimate with pricing, traditional comparison, savings percentage, and timeline.

**Usage:**
```
openskills read kaji-agentic-engineering-estimator
```

**What it does:**
1. Classifies use cases by complexity tier
2. Calculates dev streams needed per use case
3. Prices per stream with engagement model modifiers (Full Delivery / Co-Dev / Client-Led)
4. Generates traditional vs Kaji cost comparison
5. Outputs savings % and timeline estimates

**Output:** Scoped estimate with use case breakdown, dev stream pricing, traditional comparison, and engagement recommendation.

---

### kaji-client-onboarding

Interactive onboarding intake for new Kaji deployments. Walks clients through a structured 7-section questionnaire, classifies requirements into must-haves vs nice-to-haves, and generates a deployment timeline estimate.

**Usage:**
```
openskills read kaji-client-onboarding
```

**What it covers:**
1. Model requirements (MaaS + local)
2. API key configuration
3. Use cases
4. Integration selection (from 20+ supported MCPs and skills)
5. New integration requests
6. Communication preference
7. Security & guardrails

**Output:** Deployment summary with must-have/nice-to-have breakdown and estimated business days to go-live.

## Adding New Skills

Follow the [Agent Skills format](https://github.com/anthropics/agent-skills). Each skill is a directory containing:

```
skill-name/
├── SKILL.md              # Required: YAML frontmatter + instructions
├── references/           # Optional: detailed reference docs
├── assets/               # Optional: templates, resources
└── scripts/              # Optional: executable helpers
```

Keep `SKILL.md` under 500 lines. Move detailed content to `references/`.
