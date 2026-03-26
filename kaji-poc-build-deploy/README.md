# kaji-poc-build-deploy

**One-command POC build and deploy. Spec → running app on Shakudo.**

Companion to `kaji-poc-spec-builder`. This skill takes the completed spec, simplifies the architecture when needed, builds only the minimum credible components, and deploys with Shakudo microservice workflows.

---

## What changed in this version

This skill now:
- uses **`shakudo-microservice-lite`** as the default deployment workflow for create / inspect / delete
- documents the **full Shakudo lifecycle action map** available through the underlying platform GraphQL API and the older `shakudo-microservice` skill
- treats deployment as a **GraphQL-first workflow** rather than an MCP-only black box
- adds an **architecture sanity check** before building so POCs are not overbuilt
- expects the spec to include **Architecture Options Considered**, **Recommended Build Strategy**, and **Component Selection Rationale**
- distinguishes between:
  - **documented lite-skill flows**
  - **full lifecycle actions available in platform GraphQL** like scale, restart, replica tuning, and cancel

---

## How to Use

```text
@kaji deploy the dynex POC
@kaji build and deploy this spec: https://app.clickup.com/t/...
@kaji redeploy gallo-ui
@kaji restart the campbell ops POC
@kaji stop the dynex api
@kaji start the dynex ui
```

Kaji will:
1. validate the spec and deployment brief
2. check whether the architecture is too heavy for the POC
3. verify credentials and git sync
4. create or update services using the appropriate Shakudo action
5. poll deployment status and smoke test
6. return live URLs and mock / real status

---

## Lifecycle behavior

| Operation | Preferred path | Notes |
|---|---|---|
| Deploy / create | `shakudo-microservice-lite` or GraphQL `createShakudoService` | standard path |
| Inspect status | GraphQL `searchMicroservice`, `pipelineJobs`, `getPodEvents` | use exact IDs for follow-up actions |
| Start an absent service | create from registry / spec | standard create flow |
| Start a scaled-down service | GraphQL `scaleService(..., newReplicas: 1)` | non-destructive |
| Stop a running service | GraphQL `scaleService(..., newReplicas: 0)` | non-destructive |
| Restart a running service | GraphQL `restartService(id)` | preferred over delete + recreate |
| Tune autoscaling bounds | GraphQL `updateServiceReplicas(...)` | use when HPA ranges must change |
| Cancel a broken rollout | GraphQL `cancelService(id)` | use for stuck or invalid jobs |
| Delete permanently | delete by exact ID | destructive; confirmation required |

### Practical rule

- Use **create** for new services.
- Use **scaleService** for non-destructive start / stop.
- Use **restartService** for refresh after code or config changes.
- Use **delete + recreate** only when the service is unrecoverable or the user explicitly wants deletion.

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
- Architecture Scorecard
- Recommended Build Strategy
- POC Simplification Decisions
- Component Selection Rationale
- Shakudo Build Stack / Platform Map
- Service Deployment Specs with `workingDir`, `pipelineYamlPath`, `lifecycleMode`, and `recreateNotes`

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
- **shakudo-microservice-lite** — default deployment workflow for create / inspect / delete
- **shakudo-microservice** — older/full lifecycle reference for scale / restart / monitor behavior
- **kaji-agentic-engineering-estimator** — pricing / scoping after the POC is defined
