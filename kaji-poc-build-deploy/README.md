# kaji-poc-build-deploy

**Build from a completed spec or operate an already-built app on Shakudo.**

Companion to `kaji-poc-spec-builder`. This skill takes the completed spec, simplifies the architecture when needed, builds only the minimum credible components, and deploys with `shakudo-microservice-lite`.

---

## What changed in this version

This skill now:
- uses **`shakudo-microservice-lite`** as the default deployment workflow
- treats deployment as a **GraphQL-first Shakudo workflow** driven by `shakudo-microservice-lite`
- adds an **architecture sanity check** before building so POCs are not overbuilt
- expects the spec to include **Architecture Options Considered**, **Recommended Build Strategy**, and **Component Selection Rationale**
- defers detailed lifecycle semantics to the upstream `shakudo-microservice-lite` skill while staying generic downstream

## Two main functions

This skill has two main jobs:

1. **Build / rebuild from a completed spec or registry definition**
   Consume a finished `kaji-poc-spec-builder` output or an existing library POC definition, build what is missing, and deploy it.

2. **Operate an already-built app**
   Resolve an existing service and handle redeploy / restart / stop / start / scale using `shakudo-microservice-lite`.

`kaji-poc-spec-builder` creates the spec. `kaji-poc-build-deploy` consumes that spec or operates an existing deployed app.

---

## How to Use

```text
@kaji deploy the dynex POC
@kaji build and deploy this spec: https://app.clickup.com/t/...
@kaji redeploy gallo-ui
@kaji restart dynex-mbs-api
@kaji scale dynex-mbs-ui to 0
@kaji recreate the campbell ops POC
@kaji stop the dynex api
```

Kaji will either:

### For build / rebuild requests
1. validate the spec and deployment brief
2. check whether the architecture is too heavy for the POC
3. verify credentials and git sync
4. create or recreate services with `shakudo-microservice-lite`
5. poll deployment status and smoke test
6. return live URLs and mock / real status

### For existing built apps
1. resolve the exact service or services from the registry or by search
2. inspect current state and decide whether this is a redeploy / restart / stop / start / scale request
3. follow the current `shakudo-microservice-lite` lifecycle workflow
4. verify the result with status checks and smoke tests before reporting back

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

This skill now expects `kaji-poc-spec-builder` to provide the following for **new build / rebuild requests**:
- Architecture Options Considered
- Recommended Build Strategy
- Component Selection Rationale
- Shakudo Build Stack / Platform Map
- Service Deployment Specs with `workingDir` and `pipelineYamlPath`

If those are missing, this skill should stop and ask for the spec to be completed first.

For **restart / stop / scale / redeploy of an already-built app**, the registry and current service state may be enough even when no new spec work is needed.

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
