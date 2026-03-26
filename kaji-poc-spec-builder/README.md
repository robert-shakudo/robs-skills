# kaji-poc-spec-builder

**Convert a client use case into a complete, buildable POC spec and architecture decision package.**

This skill does more than write a nice requirements doc. It analyzes alternative architectures, chooses the best Shakudo component stack for the use case, explains what should be built now vs later, and produces a deployment-ready build brief for `kaji-poc-build-deploy`.

---

## What this skill now produces

For a Full Spec, Kaji should deliver:
- full app spec (sections 0–12)
- architecture options considered
- recommended build strategy
- component selection rationale
- mock / real system map
- Shakudo build stack / platform map
- service deployment specs
- POC build brief
- optional presenter walk-through

---

## How this skill works

Kaji loads and uses the following files:

| File | What it does |
|---|---|
| `SKILL.md` | Execution flow, rules, anti-patterns, and output order |
| `assets/app-spec-template.md` | Sections 0–12 of the core application spec |
| `assets/demo-architecture-template.md` | Architecture analysis, build stack, service deployment spec, and build brief format |
| `references/shakudo-platform-capabilities.md` | Best-practice component catalog and architecture patterns |
| `references/component-selection-rubric.md` | Decision framework for choosing the right Shakudo stack |
| `references/mock-patterns.md` | Standard mock strategies by system type |
| `references/demo-library.md` | Existing POCs to reuse or fork |

---

## Trigger phrases

```text
@kaji build me a spec for [client] — [use case]
@kaji spec this out: [problem / notes]
@kaji build a spec from this ClickUp task: [URL]
@kaji rewrite this into a buildable POC
@kaji what would we build for [client / problem]?
```

Walk-through on demand:

```text
@kaji show me the walk-through
@kaji walk-through for this spec
@kaji how do we present this POC?
```

---

## What Kaji should do step by step

1. **Read all available sources first** — ClickUp, Mattermost, Notion, Fireflies, pasted notes.
2. **Ask only a few questions if needed** — max 3, after reading everything.
3. **Ask the infrastructure question** — default the POC to Shakudo dev if client infra is unknown.
4. **Classify the app type** — operational assistant, analytics, workflow automation, knowledge assistant, multi-agent, or hybrid.
5. **Evaluate multiple architecture options** — do not jump straight to one stack.
6. **Choose the best component stack** — based on the component selection rubric.
7. **Write the full spec** — sections 0–12.
8. **Produce the architecture package** — build strategy, build stack, deployment specs, build brief.
9. **Show the output before offering a ClickUp update**.

---

## Best-practice philosophy

This skill should bias toward:
- the **simplest credible POC**
- **reuse of existing demo patterns**
- **one primary user surface** unless more is truly necessary
- **explicit reasoning** about why Kaji, microservices, n8n, Dremio, RAG, or other components are used
- **clear separation** between what is built now, mocked now, and deferred to later phases

It should avoid:
- overbuilding
- defaulting to multi-agent designs
- adding RAG when the value is actually structured analytics
- adding a full custom UI when chat is enough
- vague build briefs with missing deployment details

---

## File structure

```text
kaji-poc-spec-builder/
├── SKILL.md
├── README.md
├── assets/
│   ├── app-spec-template.md
│   └── demo-architecture-template.md
└── references/
    ├── shakudo-platform-capabilities.md
    ├── component-selection-rubric.md
    ├── mock-patterns.md
    └── demo-library.md
```

---

## Related skills

- **kaji-poc-build-deploy** — builds and deploys the chosen architecture
- **kaji-agentic-engineering-estimator** — prices the work after the spec is approved
- **kaji-client-onboarding** — plans implementation after the POC is validated
