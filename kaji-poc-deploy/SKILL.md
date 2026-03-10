---
name: kaji-poc-deploy
description: "One-command POC deployment. Reads a ClickUp spec (from kaji-poc-spec-builder), deploys all components to Shakudo using the demos git server, uses mocks for all unconfigured credentials, returns live URLs. Companion to kaji-poc-spec-builder. Use when asked to build, deploy, launch, go live, or spin up a POC."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.0"
  category: solutions-engineering
  tags:
    - poc
    - deploy
    - shakudo
    - microservice
    - demos
    - build
---

# Kaji POC Deploy

**Phase 2 of the POC workflow** — companion to `kaji-poc-spec-builder`.

Takes a ClickUp task ID or POC name. Reads the spec. Deploys all components to `dev.hyperplane.dev` in one pass. Uses mocks for every unconfigured credential. Returns live URLs immediately.

**Core principle**: POC deploys from the `demos` git server (`devsentient/demos`). All demos run with mock data by default. Real credentials are env vars — swap them at any time to go live.

---

## When to Activate

- "deploy the [name] POC"
- "go live with [ClickUp task URL]"
- "build and launch this spec"
- "spin up the demo for [client]"
- "redeploy / restart [service name]"
- "take the [name] POC live — here are the credentials"
- "deploy all demos"
- Any request to move from spec → running application

---

## Trigger Input Resolution

Resolve the target POC in this order:

1. **ClickUp task ID / URL** → `ClickUp_get_task(task: "86afvjudd")` → extract `name`, `description`, look up registry
2. **POC name** (gallo, hr, reagan, campbell) → look up in registry directly
3. **"the spec we just built"** → use most recently discussed ClickUp task in thread
4. **"all"** → deploy every POC in the registry sequentially

After resolving, confirm: *"Deploying [POC name] — [N] service(s). Using mocks for [systems]. Estimated time: ~2 min. Proceed?"*

---

## Two Paths

### Path A — Library POC (Gallo, HR Resume, Reagan, Campbell)

Use the [POC Registry](./references/poc-registry.md) — all deployment config is pre-defined.

→ Skip to **Phase 2: Deploy**

### Path B — New POC from ClickUp Spec

When the target POC doesn't exist in the registry yet:

1. Read the ClickUp task description (the output of `kaji-poc-spec-builder`)
2. Extract from the **POC Build Brief** section:
   - Service list (backend / frontend / worker)
   - Entry scripts (run.sh paths)
   - Environment variables (POC block)
   - Port numbers
3. Confirm the git folder name (ask if not in spec)
4. Follow Phase 2 below — treat as a single-service deploy until confirmed

---

## Phase 1: Pre-flight

Before deploying, always run these checks:

```
USER_EMAIL = read from env: $USER_EMAIL
```

If `USER_EMAIL` is not set, ask: "What email should own these services?"

Verify git server is available:
```
shakudo-platform_listGitServers()  → confirm "demos" is IN_SYNC
```

Check for existing deployments (avoid duplicates):
```
shakudo-platform_searchMicroservice({ searchTerm: "[service-name]" })
```

If a service already exists with the same name and status is `running`:
→ Ask: "A running [name] already exists. Restart it or deploy fresh?"
- **Restart**: `shakudo-platform_restartService({ id: existing_id })`
- **Fresh**: proceed with create (existing service will conflict — user must scale it down first)

---

## Phase 2: Deploy

Deploy every service defined in the registry for the target POC.

**Standard call pattern:**

```javascript
shakudo-platform_createMicroservice({
  name: "[service-name]",                    // lowercase, hyphens, max 63 chars
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",       // default; use "basic-medium" for heavier workloads
  gitServer: "demos",
  branch: "main",
  port: 8787,                                // default; override per service
  script: "[folder/run.sh]",                 // path relative to /tmp/git/monorepo/
  parameters: [                              // env vars — mocks by default
    { key: "OPENAI_API_KEY", value: "[mock-or-real]" },
    { key: "ANTHROPIC_API_KEY", value: "[mock-or-real]" }
  ]
})
```

**For multi-service POCs** (e.g., Gallo): create all services in sequence. Don't wait for one to reach `running` before starting the next.

After creating all services, confirm by summarizing:
```
Created [N] services:
  ✅ [name-1] — deploying...
  ✅ [name-2] — deploying...
```

---

## Phase 3: Poll & Verify

For each deployed service, poll status until `running` (timeout: 5 min):

```javascript
// Poll every 30s
shakudo-platform_searchMicroservice({ searchTerm: "[service-name]" })
// → look for: status === "running"
```

If not running after 3 polls, check logs:
```javascript
shakudo-platform_getPodEvents({ jobId: "[service-id]", tailLines: 50 })
```

Common failures and fixes:
| Log pattern | Cause | Fix |
|---|---|---|
| `ModuleNotFoundError` | Missing pip package | Check requirements.txt is in correct path |
| `No such file or directory: run.sh` | Wrong `script` path | Verify git folder name matches demos repo |
| `Address already in use` | Port conflict | Scale down existing service first |
| `git-server-demos: not synced` | Stale git | Wait 60s, retry |

---

## Phase 4: Return Live URLs

Once all services are `running`, output:

```
✅ [POC Name] is live

[Service]     → https://[name].dev.hyperplane.dev
[Service]     → https://[name].dev.hyperplane.dev

Running with: mock [systems list]

To go live with real credentials, set:
  [ENV_VAR]  → [what to provide]
  [ENV_VAR]  → [what to provide]

Kaji skill: [skill-name] (install from skill marketplace)
ClickUp spec: [task URL]
```

---

## Phase 5: Credential Upgrade (Go-Live)

When the user provides real credentials, upgrade the deployment without rebuilding.

**Update env vars and restart:**

1. Find the service ID:
   ```javascript
   shakudo-platform_searchMicroservice({ searchTerm: "[service-name]" })
   ```

2. Scale to zero, recreate with real params, wait for running:
   ```javascript
   // Option A: recreate (clean)
   shakudo-platform_deleteMicroservice({ id: "[id]", confirm: true })
   shakudo-platform_createMicroservice({
     ...same config...,
     parameters: [{ key: "OPENAI_API_KEY", value: "sk-real-key" }]
   })
   
   // Option B: restart (if platform supports env var update)
   shakudo-platform_restartService({ id: "[id]" })
   ```

3. Verify endpoints respond with real data.

---

## POC Registry Reference

See [poc-registry.md](./references/poc-registry.md) for:
- Exact service names, git folders, run.sh paths, ports
- Mock vs real env vars per POC
- Multi-service deployment order
- Live URLs and ClickUp spec IDs

---

## Deployment State Tracking

After a successful deployment, record in the Mattermost thread:

```
🚀 [POC Name] deployed
Services: [list]
URLs: [list]
Mode: Mock / Partial / Live
ClickUp: [task URL]
Deployed: [timestamp]
```

If the user has a ClickUp task open, offer to add a comment with deployment details.

---

## Scale Down / Teardown

When asked to "take down", "stop", or "clean up" a POC:

```javascript
// Scale to zero (preserves service config, saves resources)
shakudo-platform_scaleService({ id: "[service-id]", newReplicas: 0 })

// Full delete (only if explicitly requested)
shakudo-platform_deleteMicroservice({ id: "[service-id]", confirm: true })
```

Scale to zero by default — never delete unless the user says "delete" or "remove permanently".

---

## Execution Checklist

- [ ] Target POC identified (library lookup or new spec)
- [ ] `USER_EMAIL` resolved
- [ ] `demos` git server confirmed IN_SYNC
- [ ] Existing services checked (no duplicate deployment)
- [ ] All services created with correct `script` paths
- [ ] All services polled to `running` status
- [ ] Live URLs returned with mock/real status
- [ ] Credential upgrade path shown
- [ ] Skill name referenced for Kaji access

---

## Anti-Patterns

- **Never guess the `script` path** — use the exact paths from `poc-registry.md` or the ClickUp spec
- **Never hardcode USER_EMAIL** — always read from `$USER_EMAIL` env var
- **Never delete to redeploy** — always scale-to-zero first, then decide
- **Never deploy without checking git server sync** — stale code is the #1 deployment failure
- **Never skip Phase 4 verification** — confirm running status before returning URLs
- **Never block on one service** — create all services first, then poll all in parallel

---

## Related Skills

- **kaji-poc-spec-builder** — Phase 1: generate the spec this skill deploys
- **shakudo-microservice** — Low-level microservice management reference
- **kaji-agentic-engineering-estimator** — Price the engagement after the POC is approved
