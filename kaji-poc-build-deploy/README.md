# kaji-poc-build-deploy

**One-command POC build and deploy. Spec → running app on Shakudo.**

Companion to `kaji-poc-spec-builder`. This skill takes the completed spec, simplifies the architecture when needed, builds only the minimum credible components, and deploys with `shakudo-microservice-lite`.

---

## What changed in this version

This skill now:
- uses **`shakudo-microservice-lite`** as the default deployment workflow
- treats deployment as a **GraphQL-first Shakudo workflow** driven by `shakudo-microservice-lite`
- adds an **architecture sanity check** before building so POCs are not overbuilt
- expects the spec to include **Architecture Options Considered**, **Recommended Build Strategy**, and **Component Selection Rationale**
- defers detailed lifecycle semantics to the upstream `shakudo-microservice-lite` skill while staying generic downstream

---

## How to Use

```text
@kaji deploy the dynex POC
@kaji build and deploy this spec: https://app.clickup.com/t/...
@kaji redeploy gallo-ui
@kaji recreate the campbell ops POC
@kaji stop the dynex api
```

Kaji will:
1. validate the spec and deployment brief
2. check whether the architecture is too heavy for the POC
3. verify credentials and git sync
4. create services with `shakudo-microservice-lite`
5. poll deployment status and smoke test
6. return live URLs and mock / real status

---

## Lifecycle behavior

| Operation | Preferred path | Lite-only behavior |
|---|---|---|
| Deploy / create | `shakudo-microservice-lite` | create from registry or spec |
| Inspect status | `shakudo-microservice-lite` | search + `pipelineJobs` + `getPodEvents` |
| Start an absent service | `shakudo-microservice-lite` | create |
| Start a stopped service | `shakudo-microservice-lite` lifecycle workflow | follow the current upstream lite-skill guidance |
| Stop a running service | `shakudo-microservice-lite` lifecycle workflow | follow the current upstream lite-skill guidance |
| Restart | `shakudo-microservice-lite` lifecycle workflow | follow the current upstream lite-skill guidance |
| Delete | `shakudo-microservice-lite` | delete by exact ID |

For exact start / stop / restart semantics, always follow the current `shakudo-microservice-lite` skill rather than hard-coding assumptions here.

---

## POC Library

See [POC Registry](./references/poc-registry.md) for exact deployment definitions for:
- HR Resume Processor
- Gallo Freight Exception Hub
- Reagan NLP-to-SQL
- Campbell Ops Co-Pilot
- Dynex MBS Intelligence Platform

---

## Expected spec outputs

This skill now expects `kaji-poc-spec-builder` to provide:
- Architecture Options Considered
- Recommended Build Strategy
- Component Selection Rationale
- Shakudo Build Stack / Platform Map
- Service Deployment Specs with `workingDir` and `pipelineYamlPath`

If those are missing, this skill should stop and ask for the spec to be completed first.

---

## File Structure

```text
kaji-poc-build-deploy/
├── SKILL.md
├── README.md
└── references/
    └── poc-registry.md
```

---

## Related Skills

- **kaji-poc-spec-builder** — creates the buildable spec and architecture rationale
- **shakudo-microservice-lite** — default deployment workflow
- **kaji-agentic-engineering-estimator** — pricing / scoping after the POC is defined
