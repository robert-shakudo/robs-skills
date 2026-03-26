---
name: kaji-poc-build-deploy
description: "One-command POC build and deploy. Reads a ClickUp spec (from kaji-poc-spec-builder), runs an architecture sanity check, builds the minimum credible app, and deploys to Shakudo using shakudo-microservice-lite GraphQL workflows. Uses mocks for unconfigured systems and returns live URLs."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.2"
  category: solutions-engineering
  tags:
    - poc
    - build
    - deploy
    - shakudo
    - microservice
    - demos
    - architecture
---

# Kaji POC Build & Deploy

**Phase 2 of the POC workflow** — companion to `kaji-poc-spec-builder`.

This skill turns a completed POC spec into a running application on Shakudo. It validates the spec, checks credentials, simplifies the architecture when the spec is overbuilt for a POC, builds only the minimum credible components, deploys with `shakudo-microservice-lite`, and returns live URLs.

## Core principles

- **Simplest credible POC wins.** Do not overbuild just because the final production vision is large.
- **Reuse before creating.** Prefer forking patterns from the demo library before inventing a fresh stack.
- **Ask for missing credentials before coding.** Never discover blockers mid-build.
- **Deploy only from synced git.** Local code, remote branch, and Shakudo git server must agree.
- **Use `shakudo-microservice-lite` by default.** It is the standard workflow for create / inspect / delete. Use the full `shakudo-microservice` skill only when scale or restart behavior is actually available and needed.

---

## When to Activate

- "build and deploy the [name] POC"
- "go live with [ClickUp task URL]"
- "build and deploy this spec"
- "spin up the demo for [client]"
- "deploy / redeploy [service name]"
- "start [POC name]" or "recreate [POC name]"
- "stop / teardown / delete [POC name]"
- "take the [name] POC live — here are the credentials"
- "build and deploy all demos"

---

## Two Paths

### Path A — Library POC

For existing POCs like Gallo, HR Resume, Reagan, Campbell, and Dynex:
- skip new architecture design
- run a quick architecture sanity check
- go straight to deploy / recreate using the registry

Use [POC Registry](./references/poc-registry.md) for exact service definitions.

### Path B — New POC from Spec

For a new ClickUp spec:
- validate the spec outputs
- confirm the recommended build strategy still makes sense
- build the missing components
- commit and push to `devsentient/demos`
- deploy each service

---

## Required Inputs from `kaji-poc-spec-builder`

Do not build unless the spec includes all of the following:

| Required output | Why it matters |
|---|---|
| Sections 0–12 of the app spec | Core product, users, systems, scope, UX |
| Architecture Options Considered | Shows what approaches were evaluated |
| Recommended Build Strategy | Tells you what to build now vs later |
| Component Selection Rationale | Explains why each Shakudo component was chosen |
| Mock / Real System Map | Determines which credentials are required |
| POC Build Brief | Defines service list, env vars, deploy order, smoke tests |
| Service Deployment Specs | Provides `jobName`, `workingDir`, `pipelineYamlPath`, `port`, `jobType` |

If anything is missing, stop and say:

> "The spec is missing [X]. Update the spec first with `kaji-poc-spec-builder`, or tell me [X] now so I can complete the build brief before deploying."

---

## Phase 0: Spec Validation

Read the ClickUp task and confirm all required sections are present.

| Check | Where to look | What to confirm |
|---|---|---|
| Service list | POC Build Brief | Every backend, UI, worker, or webhook service is named |
| Entry scripts | Service Deployment Specs | `run.sh` or equivalent path is explicit |
| Env vars | POC Build Brief | Mock vs real credentials are named |
| Port numbers | Build Brief / deployment specs | Explicit or intentionally standardized to `8787` |
| Architecture outputs | Architecture Analysis block | Recommended stack and rejected alternatives are written |
| External systems | Section 6 + mock/real map | Each system is classified as available, mocked, or needs build |
| Smoke tests | Build Brief | At least one health or business-path check per service |

If the build brief is incomplete, do not improvise a deployment plan from scattered notes.

---

## Phase 1: Architecture Sanity Check

Before touching code, verify that the spec is still the right shape for a POC.

### Questions to answer

1. Is this **chat-first**, **UI-first**, **workflow-first**, **analytics-first**, or **hybrid**?
2. Which components are required to tell the story in the meeting?
3. Which components can be deferred to Year 2 without hurting the POC?
4. Is there an existing demo whose patterns should be reused?
5. Is the current architecture overbuilt for the available timeline?

### Default simplification rules

- Prefer **one primary user surface**. Do not build both a full UI and a rich chat interface unless both are truly necessary.
- Prefer **one reasoning layer**. Avoid multi-agent orchestration unless there are clearly separate agent roles.
- Prefer **mocked external actions** before building real integration plumbing.
- Prefer **chat + API** or **single UI + API** over multiple independently deployed surfaces.
- Prefer **analytics over structured data** via Dremio / SQL before using RAG.
- Prefer **RAG** only when the key value comes from documents, policies, or narrative knowledge.
- Prefer **n8n** for approvals / orchestration, not for core application logic that should live in code.

### Required output before Phase 2

Produce a short internal build summary:

```md
## Deployment Architecture Check
- Selected architecture from spec:
- Still best for POC speed? yes / no
- Components to build now:
- Components to defer:
- Closest existing demo to reuse:
- Notes on simplifications:
```

If the answer is "no", simplify first and explain the change.

---

## Phase 2: Credential Pre-flight

Run this before writing code or deploying anything.

### 2a — Scan the spec for external systems

Read Section 6, the mock/real map, and the component rationale. For every system or component that is not mocked, verify the required credential.

### 2b — Check core credentials

```bash
# Preferred identity for GraphQL deployment calls
export USER_EMAIL="${USER_EMAIL:-$KAJI_USER}"

# Git access
cd /path/to/repo && git remote -v
cat /root/.git-credentials 2>/dev/null

# n8n (if used)
cat /root/n8n-v2-api-key 2>/dev/null

# LLM / platform keys
printenv OPENAI_API_KEY
printenv ANTHROPIC_API_KEY
```

If `USER_EMAIL` is still empty after the fallback, stop and ask the user to provide it.

### 2c — Validate n8n if the spec uses it

```bash
N8N_KEY=$(cat /root/n8n-v2-api-key 2>/dev/null)
curl -s -o /dev/null -w "%{http_code}"   -H "X-N8N-API-KEY: $N8N_KEY"   "https://n8n-v2.dev.hyperplane.dev/api/v1/workflows?limit=1"
```

If the key is missing or expired, stop and ask whether to:
- provide a fresh key
- skip workflow creation for now
- deploy with the workflow mocked

### 2d — Show the pre-flight summary

```text
📋 Pre-flight — [POC Name]

Architecture now: [chat + api / ui + api / workflow-first / etc.]
Services to build: [list]
Components deferred: [list]
Repo: devsentient/demos/[folder]

Credentials:
  ✅ USER_EMAIL
  ✅ git credentials
  ✅ OPENAI_API_KEY / ANTHROPIC_API_KEY
  ⚠️ n8n key — [missing / expired / not needed]

What works in mock mode:
  ✅ [list]

What requires a real key:
  ⚠️ [list]

ETA: [estimate]
Proceed?
```

Do not start until the user confirms.

---

## Phase 3: Build Rules and Git Structure

### Repository convention

**Single deployment source of truth: `devsentient/demos`**

All deployable POC code lives in the demos monorepo as a subfolder.

| Artifact | Pattern | Example |
|---|---|---|
| Monorepo folder | `{client}-{type}` | `dynex-mbs` |
| Service names | `{client}-{type}-api`, `{client}-{type}-ui` | `dynex-mbs-api` |
| Live URLs | `{service-name}.dev.hyperplane.dev` | `dynex-mbs-ui.dev.hyperplane.dev` |

### Required file structure

```text
{client}-{type}/
├── README.md
├── APP_INFO_AND_ARCHITECTURE.md
├── api/ or backend/
│   ├── main.py (or app.py)
│   ├── requirements.txt
│   └── run.sh
├── ui/ or frontend/              # only if a frontend is truly needed
│   ├── package.json
│   ├── src/
│   └── run.sh
└── skill/
    ├── SKILL.md
    └── README.md
```

### `run.sh` defaults

**Backend**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
cd "$SCRIPT_DIR"
pip install -q -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8787 --workers 1
```

**Frontend**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
cd "$SCRIPT_DIR"
npm install --legacy-peer-deps
VITE_API_URL=https://{client}-{type}-api.dev.hyperplane.dev npm run build
npx serve -s dist -l 8787
```

### Git workflow

```bash
git clone --depth 1 https://${GH_TOKEN}@github.com/devsentient/demos.git /tmp/demos-build
cp -r /tmp/{build-dir} /tmp/demos-build/{folder-name}/
cd /tmp/demos-build

git add -f {folder-name}/**/*.csv 2>/dev/null || true
git add {folder-name}/
git commit -m "feat({folder-name}): add [POC name]"
git push origin main
```

If data files are required, always force-add them if `.gitignore` blocks them.

---

## Phase 4: Deploy with `shakudo-microservice-lite`

Load `shakudo-microservice-lite` and follow its GraphQL workflow. This is the standard deployment path.

### 4a — Deployment prerequisites

```bash
ENDPOINT=http://api-server.hyperplane-core.svc.cluster.local:80/graphql
JQ_REQUIRED=true
```

The repo mount inside deployed services is always rooted at:

```bash
/tmp/git/monorepo
```

Do not derive container paths from your local checkout path.

### 4b — Verify local repo state

Use the bundled script from the lite skill:

```bash
bash /skills/shakudo-microservice-lite/scripts/check-local-git-sync.sh /path/to/repo
```

Stop if the target repo is:
- dirty
- behind
- ahead
- diverged
- missing an upstream

Exception: if the repo is dirty only because of unrelated files outside the target app path, inspect carefully before blocking the deploy.

### 4c — Resolve the correct git server

Do not assume `demos` blindly. Resolve it from the repo remote URL via the lite skill's `hyperplaneVCServers` query.

Confirm:
- `gitServerName`
- `gitSyncStatus`
- `lastSyncedCommitHash`
- `defaultBranch`

### 4d — Confirm git server sync

Compare:
- local `HEAD`
- git server `lastSyncedCommitHash`
- git server `gitSyncStatus`

Rules:
- if `HEAD` != `lastSyncedCommitHash`, stop and reconcile git first
- if `gitSyncStatus` looks unhealthy, stop and inspect before create
- if `lastSyncedCommitHash` is null, do not deploy

### 4e — Choose the correct path pairing

Use one of the two valid patterns from the lite skill:

**Pattern A — app directory working dir**
```bash
WORKING_DIR="/tmp/git/monorepo/my-app/"
PIPELINE_PATH="run.sh"
```

**Pattern B — repo root working dir**
```bash
WORKING_DIR="/tmp/git/monorepo/"
PIPELINE_PATH="my-app/run.sh"
```

Current demo-library services usually use **Pattern B**.

### 4f — Create the service

Use the lite skill's `createShakudoService` mutation with:
- `jobName`
- `subdomain`
- `jobType`
- `pipelineYamlPath`
- `workingDir`
- `port`
- `gitServerName`
- `branchName`
- `userEmail`
- `parameters`

For multi-service apps:
1. backend / API first
2. frontend second
3. worker / cron / webhook services after the API is stable

### 4g — Poll the PipelineJob

A successful create response is **not** a successful deployment.

After create, poll `pipelineJobs` by job ID and confirm:
- status moves beyond the initial pending state
- `workerPodName` becomes non-null
- `exposedPort` is correct

### 4h — Inspect events and logs if stuck

If the job stays pending or status is unclear, use `getPodEvents` from the lite skill.

Treat a long-lived `pending` job with `workerPodName: null` as a platform reconciliation blocker, not proof that the app code is broken.

### 4i — Verify the in-cluster service URL

Derive the internal URL from the first segment of the job ID:

```bash
JOB_PREFIX="${JOB_ID%%-*}"
INTERNAL_URL="http://hyperplane-service-${JOB_PREFIX}.hyperplane-pipelines.svc.cluster.local:${PORT}"
```

Only curl this after the PipelineJob has actually materialized into a running service.

---

## Phase 5: n8n Workflow Creation

Only run this phase if the spec explicitly uses n8n **and** the API key was validated in Phase 2.

If the key is unavailable, deploy the app with the workflow mocked and attach the workflow JSON or import steps for later.

---

## Phase 6: E2E Smoke Test

Smoke test every deployed service from the internal cluster URL first.

```bash
curl -s "$INTERNAL_URL/health"
curl -s "$INTERNAL_URL/[key-endpoint]"
curl -s -o /dev/null -w "%{http_code}" "$INTERNAL_URL/"
```

Report results as a table.

| Service | Check | Expected | Result |
|---|---|---|---|
| [service] | `/health` | 200 / JSON OK | |
| [service] | key business path | correct JSON / HTML | |
| [service] | UI root | 200 / 302 | |

Do not share URLs with the user until the key paths pass.

---

## Phase 7: Return Live URLs

After smoke tests pass, return:

```text
✅ [POC Name] is live

Frontend    → https://[name]-ui.dev.hyperplane.dev
API         → https://[name]-api.dev.hyperplane.dev
GitHub      → https://github.com/devsentient/demos/tree/main/[folder]
ClickUp     → https://app.clickup.com/t/[id]

Running with:
  mock systems: [list]
  real systems: [list]
  deferred components: [list]

To go live with real credentials:
  [ENV_VAR] → [what it unlocks]
```

Always mention the **mock vs real** status and any intentionally deferred components.

---

## Phase 8: Update the POC Registry

If you build or materially change a POC, update [POC Registry](./references/poc-registry.md) so future deploys can recreate it deterministically.

Capture:
- job name and subdomain
- job type
- working dir and pipeline path pairing
- deploy order
- required parameters
- smoke tests
- lite lifecycle notes

---

## Credential Upgrade / Go-Live Refresh

When the user provides real credentials later:

1. update the deployment parameters
2. if using lite-only lifecycle, delete and recreate the affected service
3. if the full microservice skill is available, use it for restart / scale workflows after config changes

Always explain whether the refresh will be destructive.

---

## Start / Stop / Restart / Delete

`shakudo-microservice-lite` is **minimal lifecycle only**. It supports create, inspect, and delete. It does **not** provide scale-to-zero or non-destructive restart operations.

### Lifecycle policy

| User intent | Preferred path | Lite-only fallback |
|---|---|---|
| Start a missing service | Create from registry config | Same |
| Start an existing but stopped service | Use full `shakudo-microservice` if scale / restart is available | Delete + recreate after confirmation |
| Stop a running service | Scale to 0 with full skill if available | Lite cannot pause; delete only after explicit confirmation |
| Restart a running service | Restart with full skill if available | Delete + recreate after explicit confirmation |
| Delete permanently | Delete by exact ID | Same |

### Search first

Always resolve the exact service ID before any destructive action:

- search by job name
- if multiple matches exist, ask the user which one
- never delete by guessed name alone

### Lite-mode stop behavior

If only lite is available, say this clearly:

> "I can recreate or delete this service with `shakudo-microservice-lite`, but I cannot scale it to zero non-destructively. Do you want me to delete it?"

### Lite-mode restart behavior

If the user says restart and only lite is available:
1. confirm delete + recreate is acceptable
2. delete by exact ID
3. recreate from the registry / build brief config
4. poll and smoke test again

### Stop all POCs

If the user asks to stop everything and only lite is available:
- list every service that would be deleted
- ask for explicit confirmation
- delete each by exact ID
- report the deleted IDs

---

## Execution Checklist

- [ ] ClickUp spec read and validated
- [ ] Architecture sanity check completed
- [ ] Build-now vs defer decision recorded
- [ ] Mock / real systems reviewed
- [ ] USER_EMAIL resolved
- [ ] Git credentials verified
- [ ] n8n key checked if needed
- [ ] User confirmed pre-flight summary
- [ ] Repo structure follows demo conventions
- [ ] Code pushed to `devsentient/demos`
- [ ] Local repo sync checked with `check-local-git-sync.sh`
- [ ] Git server resolved from remote URL
- [ ] `lastSyncedCommitHash` matches local HEAD
- [ ] `workingDir` / `pipelineYamlPath` pairing chosen correctly
- [ ] Services created in the right order
- [ ] PipelineJobs polled beyond initial pending state
- [ ] Internal smoke tests passed
- [ ] URLs returned with mock / real / deferred notes
- [ ] Registry updated if the deployment shape changed

---

## Anti-Patterns

- **Do not deploy from a dirty or unsynced repo.**
- **Do not assume the git server name.** Resolve it.
- **Do not assume `createShakudoService` means running.** Poll `pipelineJobs`.
- **Do not derive container paths from your local checkout path.** Use `/tmp/git/monorepo`.
- **Do not use lite delete as a hidden stop action.** Tell the user it is destructive.
- **Do not keep every component from the spec if they are unnecessary for the POC.** Simplify first.
- **Do not report URLs before smoke tests pass.**
- **Do not build n8n workflows with an expired key.** Mock or export instead.

---

## Related Skills

- **kaji-poc-spec-builder** — Phase 1: generates the spec and component-selection rationale
- **shakudo-microservice-lite** — Standard deployment workflow for create / inspect / delete
- **shakudo-microservice** — Optional full-lifecycle operations when scale / restart is available
- **kaji-agentic-engineering-estimator** — Price the engagement after the POC is built
