# kaji-poc-build-deploy

**One-command POC build and deploy. Spec → running app in ~2 minutes.**

Companion to `kaji-poc-spec-builder`. Phase 1 builds the spec. This builds and deploys it.

Reads a ClickUp task (from kaji-poc-spec-builder), deploys all components to `dev.hyperplane.dev` using the `demos` git server, uses mocks for every unconfigured credential, and returns live URLs.

---

## How to Use

```
@kaji deploy the HR resume POC
@kaji go live with https://app.clickup.com/t/86afvjudd
@kaji spin up the gallo demo
@kaji deploy all POCs
@kaji take the campbell ops demo live — here's the Anthropic key: sk-ant-...
```

Kaji will:
1. Identify the POC from name or ClickUp task ID
2. Check the `demos` git server is synced
3. Create all microservices on Shakudo
4. Poll until running (typically 60–120 seconds)
5. Return live URLs with mock/real status

---

## POC Library (deploy any of these with one command)

| POC | Command | Live URL(s) |
|---|---|---|
| HR Resume Processor | `@kaji deploy hr resume POC` | https://hr-resume-demo.dev.hyperplane.dev |
| Gallo Freight Exception Hub | `@kaji deploy gallo POC` | https://gallo-ui.dev.hyperplane.dev + api |
| Reagan NLP-to-SQL | `@kaji deploy reagan POC` | https://reagan-crm-chat.dev.hyperplane.dev |
| Campbell Ops Co-Pilot | `@kaji deploy campbell POC` | https://campbell-ops-demo.dev.hyperplane.dev |

---

## Mock → Live Upgrade

Every POC runs on mock data out of the box. When you're ready to go live:

```
@kaji take the gallo POC live — OPENAI_API_KEY=sk-real-...
@kaji upgrade campbell to use real Anthropic key: sk-ant-real-...
```

Kaji will redeploy with the real credentials. No code changes required.

---

## For New POCs

After running `kaji-poc-spec-builder` and getting a ClickUp spec:

```
@kaji deploy this spec: https://app.clickup.com/t/[task-id]
```

Kaji reads the **POC Build Brief** section of the spec (env vars, service list, entry scripts) and deploys. First deployment is always in mock mode.

---

## File Structure

```
kaji-poc-build-deploy/
├── SKILL.md                       ← Kaji execution logic
├── README.md                      ← This file
└── references/
    └── poc-registry.md            ← Exact deployment config for all 4 POCs
```

---

## Related Skills

- **kaji-poc-spec-builder** — Phase 1: generate the spec (run this first)
- **shakudo-microservice** — Low-level platform reference
- **kaji-agentic-engineering-estimator** — Price the engagement after the POC is built
