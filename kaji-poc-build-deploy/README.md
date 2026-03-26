# kaji-poc-build-deploy

**One-command POC build and deploy. Spec → running app on Shakudo.**

Companion to `kaji-poc-spec-builder`. This skill takes the completed spec, simplifies the architecture when needed, builds only the minimum credible components, and deploys with `shakudo-microservice-lite`.

---

## What changed in this version

This skill now:
- uses **`shakudo-microservice-lite`** as the default deployment workflow
- treats deployment as a **GraphQL create / inspect / delete** flow, not an MCP-only flow
- adds an **architecture sanity check** before building so POCs are not overbuilt
- expects the spec to include **Architecture Options Considered**, **Recommended Build Strategy**, and **Component Selection Rationale**
- makes **stop / start / restart** behavior explicit when only lite lifecycle is available

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
| Start a stopped service | full `shakudo-microservice` if available | delete + recreate after confirmation |
| Stop a running service | full `shakudo-microservice` if scale-to-zero is available | delete only after confirmation |
| Restart | full `shakudo-microservice` if available | delete + recreate |
| Delete | `shakudo-microservice-lite` | delete by exact ID |

If only lite is available, the skill must clearly tell the user that stop / restart are destructive.

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
- **shakudo-microservice** — optional full lifecycle operations
- **kaji-agentic-engineering-estimator** — pricing / scoping after the POC is defined
