---
name: kaji-poc-build-deploy
description: "One-command POC build and deploy. Reads a ClickUp spec (from kaji-poc-spec-builder), builds and deploys all components to Shakudo using the demos git server, uses mocks for all unconfigured credentials, returns live URLs. Companion to kaji-poc-spec-builder. Use when asked to build, deploy, launch, go live, or spin up a POC."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.1"
  category: solutions-engineering
  tags:
    - poc
    - build
    - deploy
    - shakudo
    - microservice
    - demos
    - build
---

# Kaji POC Build & Deploy

**Phase 2 of the POC workflow** — companion to `kaji-poc-spec-builder`.

Takes a ClickUp task ID or POC name. Validates the spec. Checks ALL credentials upfront. Builds code, commits to git, deploys to `dev.hyperplane.dev`. Returns live URLs.

**Core principle**: Ask for everything you need before you write a single line of code. Never discover a missing credential mid-flight.

---

## When to Activate

- "build and deploy the [name] POC"
- "go live with [ClickUp task URL]"
- "build and deploy this spec"
- "spin up the demo for [client]"
- "redeploy / restart [service name]"
- "take the [name] POC live — here are the credentials"
- "build and deploy all demos"

---

## Two Paths

### Path A — Library POC (Gallo, HR Resume, Reagan, Campbell, Dynex)

Pre-built — code already exists. Skip Phase 2 (Build), go straight to Phase 3 (Deploy).

→ See [POC Registry](./references/poc-registry.md) for exact configs.

### Path B — New POC from ClickUp Spec

Code doesn't exist yet. Run all phases in order: validate → credential check → build → deploy → test.

---

## Phase 0: Spec Validation

**STOP if the spec is missing required sections. Never build from an incomplete spec.**

Read the ClickUp task. Confirm ALL of the following exist:

| Required | Where in spec | What to check |
|---|---|---|
| Service list | POC Build Brief | At least 1 service defined (backend / frontend / worker) |
| Entry scripts | POC Build Brief | `run.sh` path named for each service |
| Environment vars | POC Build Brief — "POC (dev)" block | Which LLM, which external systems |
| Port numbers | POC Build Brief or Section 2 | Explicit or assumed 8787 |
| External systems | Section 6 (Systems Connected) | Which are mocked vs real |
| Mock/Real map | Mock/Real System Map table | Each system has a mock strategy |

If ANY section is missing → stop and tell the user:
> "The spec is missing [X]. Update the spec first with `kaji-poc-spec-builder`, or tell me [X] now."

Do not build. Do not deploy. Wait.

---

## Phase 1: Credential Pre-flight

**Run before writing any code. Ask for every key upfront. Never discover a missing credential mid-flight.**

### 1a — Scan spec for external systems

Read Section 6 (Systems Connected) and Mock/Real System Map. For every system NOT marked 🟡 Mocked, and every platform component mentioned (n8n, LLM, etc.) — check if the required credential exists.

### 1b — Check all credentials

```bash
# GitHub token (required for all builds)
git remote get-url origin   # check if token is embedded

# n8n API key (if spec mentions n8n)
cat /root/n8n-v2-api-key 2>/dev/null

# LLM keys
echo $OPENAI_API_KEY
echo $ANTHROPIC_API_KEY
```

### 1c — Validate n8n key if spec uses n8n

```bash
N8N_KEY=$(cat /root/n8n-v2-api-key 2>/dev/null)
# Test it — 200 = valid, 401 = expired
curl -s -o /dev/null -w "%{http_code}" \
  -H "X-N8N-API-KEY: $N8N_KEY" \
  "https://n8n-v2.dev.hyperplane.dev/api/v1/workflows?limit=1"
```

If expired or missing — **STOP**:

> "⚠️ n8n API key is [missing / expired].
>
> **What won't work without it:** [list specific workflows from spec — e.g., 'briefing approval flow', 'exception detection webhook']
>
> Get a fresh key: https://n8n-v2.dev.hyperplane.dev → Settings → API Keys → Create → save to `/root/n8n-v2-api-key`
>
> Reply with the key, drop it in the file, or say 'skip n8n' to deploy without automation workflows."

Wait. Do not proceed until: valid key confirmed OR user says "skip n8n".

### 1d — Show pre-flight summary and get confirmation

```
📋 Pre-flight — [POC Name]

Services to build: [list]
Repos: robert-shakudo/[name] + devsentient/demos/[folder]

Credentials:
  ✅ GitHub token — valid
  ✅ USER_EMAIL — robert@shakudo.io
  ✅ OPENAI_API_KEY — set (mock mode)
  ⚠️ n8n API key — EXPIRED → [waiting for new key]

What works in mock mode:
  ✅ [service] — fully functional with mock data

What requires a real key:
  ⚠️ n8n approval workflow — blocked until key is updated

ETA: ~10–15 min

Proceed? (or provide missing keys first)
```

**Do not start building until confirmed.**

---

## Phase 2: Build — Git Structure Rules

Every new POC MUST match the exact structure of existing demos. No exceptions.

### Repository Convention

| Artifact | Pattern | Example |
|---|---|---|
| Client GitHub repo | `robert-shakudo/{client}-{type}-demo` | `robert-shakudo/dynex-mbs-demo` |
| Demos monorepo folder | same name without `-demo` | `devsentient/demos/dynex-mbs` |
| Shakudo service names | `{client}-{type}-api`, `{client}-{type}-ui` | `dynex-mbs-api`, `dynex-mbs-ui` |
| Live URLs | `{service-name}.dev.hyperplane.dev` | `dynex-mbs-ui.dev.hyperplane.dev` |

### Required File Structure (every demo must have ALL of these)

```
{client}-{type}/                    ← folder in devsentient/demos
├── README.md                       ← architecture + usage + KPIs + local dev
├── APP_INFO_AND_ARCHITECTURE.md    ← technical topology diagram + env vars table
├── api/ or backend/
│   ├── main.py (or app.py)
│   ├── requirements.txt
│   └── run.sh
├── ui/ or frontend/                ← only if spec has a frontend
│   ├── package.json
│   ├── src/
│   └── run.sh
└── skill/
    ├── SKILL.md                    ← Kaji chat skill (YAML frontmatter + commands)
    └── README.md                   ← skill install guide
```

### run.sh — always follow this exact format

**Backend:**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
cd "$SCRIPT_DIR"
pip install -q -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8787 --workers 1
```

**Frontend:**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
cd "$SCRIPT_DIR"
npm install --legacy-peer-deps
VITE_API_URL=https://{client}-{type}-api.dev.hyperplane.dev npm run build
npx serve -s dist -l 8787
```

### Git commit order

```bash
# 1. Push to client repo first
git init && git add -A && git commit -m "feat: initial build — [POC name]"
git remote add origin https://${GH_TOKEN}@github.com/robert-shakudo/{name}.git
git push -u origin main

# 2. Mirror to devsentient/demos
git clone --depth 1 https://${GH_TOKEN}@github.com/devsentient/demos.git /tmp/demos-build
cp -r /tmp/{build-dir} /tmp/demos-build/{folder-name}/
cd /tmp/demos-build

# ⚠️ devsentient/demos .gitignore blocks *.csv — force-add any data files
git add -f {folder}/api/data/*.csv 2>/dev/null || true
git add {folder-name}/
git commit -m "feat({folder}): add [POC name] — [client]"
git push origin main
```

### skill/SKILL.md format (required in every demo)

```yaml
---
name: {client}-{type}
description: "Chat-native interface to [POC name]. [2-sentence description]. Use when [trigger]."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.0"
  category: [ops|analytics|hr|supply-chain|etc]
  tags: [relevant-tags]
---
```

Content sections: When to Activate, API endpoints used by skill, Behavior Guidelines (what to do for each trigger), Example Responses, Credentials Required.

---

## Phase 3: Deploy

After code is in demos repo — wait for git server sync:

```javascript
shakudo-platform_checkGitServerSync()  // wait until IN_SYNC
```

Then deploy each service:

```javascript
shakudo-platform_createMicroservice({
  name: "[service-name]",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",  // "basic-medium" if run.sh uses apt-get
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "[folder]/[api-or-ui]/run.sh",
  parameters: [
    { key: "OPENAI_API_KEY", value: "[mock-or-real]" }
  ]
})
```

For multi-service: create backend first, then frontend.

---

## Phase 4: n8n Workflow Creation

**Only if spec mentions n8n AND valid key was confirmed in Phase 1.**

```bash
N8N_KEY=$(cat /root/n8n-v2-api-key)

# Create workflow
curl -s -X POST "https://n8n-v2.dev.hyperplane.dev/api/v1/workflows" \
  -H "X-N8N-API-KEY: $N8N_KEY" \
  -H "Content-Type: application/json" \
  -d '{ ...workflow JSON from spec build brief... }'

# Activate it
curl -s -X POST "https://n8n-v2.dev.hyperplane.dev/api/v1/workflows/{id}/activate" \
  -H "X-N8N-API-KEY: $N8N_KEY"
```

If key was skipped → attach workflow JSON in thread:
> "n8n workflow not created (key skipped). Import manually:
> n8n v2 → New Workflow → ⋮ → Import from file → Activate"

---

## Phase 5: Poll & Verify

Poll each service until `running` (5 min timeout):

```javascript
shakudo-platform_searchMicroservice({ searchTerm: "[service-name]" })
```

Common failures:

| Log pattern | Root cause | Fix |
|---|---|---|
| `No such file or directory: run.sh` | Wrong `script` path / folder name mismatch | Check demos repo sync, verify folder |
| `*.csv not found` | .gitignore blocked data file | Re-add with `git add -f`, push, restart |
| `ModuleNotFoundError` | Missing requirement | Fix requirements.txt, push, restart |
| `Address already in use` | Old service still running | Scale down first |

---

## Phase 6: E2E Smoke Test

After all services reach `running`, test every API endpoint from the spec using the internal cluster URL (bypasses Istio auth):

```bash
BASE="http://hyperplane-service-[id].hyperplane-pipelines.svc.cluster.local:8787"
curl -s "$BASE/health"                    # health check
curl -s "$BASE/[key-endpoint-1]"          # business endpoints
curl -s -o /dev/null -w "%{http_code}" "$BASE/"  # frontend loads
```

Report as a table. Fix any `500` errors before reporting URLs — check logs, push fix, restart, retest.

---

## Phase 7: Return Live URLs

Only after smoke tests pass:

```
✅ [POC Name] is live

Frontend    → https://[name]-ui.dev.hyperplane.dev
API         → https://[name]-api.dev.hyperplane.dev
GitHub      → https://github.com/robert-shakudo/[name]
ClickUp     → https://app.clickup.com/t/[id]

Running with: mock [systems list]
n8n:        ✅ [workflow] active  /  ⚠️ skipped (key not provided)

To go live with real credentials:
  [ENV_VAR]  →  [description]

Kaji skill: [skill-name]
Install:    openskills install robert-shakudo/[repo]/skill
```

---

## Phase 8: Update POC Registry

Add the new POC to `poc-registry.md` in this skill repo and commit:

```bash
# Edit /root/.claude/skills/kaji-poc-build-deploy/references/poc-registry.md
# Follow the exact format of existing entries
# Commit and push to robert-shakudo/robs-skills
```

---

## Credential Upgrade (Go-Live)

When user provides real credentials after initial mock deploy:

```javascript
// Recreate with real params
shakudo-platform_deleteMicroservice({ id: "[id]", confirm: true })
shakudo-platform_createMicroservice({
  ...same config...,
  parameters: [{ key: "OPENAI_API_KEY", value: "sk-real-key" }]
})
```

---

## Stop / Teardown

### Trigger Phrases

- "stop the [name] POC"
- "shut down the [name] services"
- "scale down [name]"
- "pause the [name] POC"
- "take down everything"
- "stop all running POCs"
- "delete the [name] POC" (only for full delete — confirm first)

### Step 1 — Find the service IDs

```javascript
// Search by POC name
shakudo-platform_searchMicroservice({ searchTerm: "[poc-name]" })
// e.g. "dynex-mbs" returns dynex-mbs-api + dynex-mbs-ui
```

### Step 2 — Scale to zero (default)

Scaling to zero stops the pods but **preserves the service config**. The POC can be restarted instantly without rebuilding.

```javascript
// Stop one service
shakudo-platform_scaleService({ id: "[service-id]", newReplicas: 0 })

// For multi-service POCs — stop all services found in Step 1
// Run for each service ID returned by the search
```

After scaling to zero, confirm:
```
⏹️ [POC name] stopped.
  [service-1] — scaled to 0 (config preserved)
  [service-2] — scaled to 0 (config preserved)

To restart: @kaji restart the [name] POC
To delete permanently: @kaji delete the [name] POC
```

### Step 3 (optional) — Full delete

Only do this if the user says **"delete"**, **"remove"**, or **"permanently remove"**. Always confirm first:

> "⚠️ This will permanently delete [service-name] and all its config. Are you sure? (yes/no)"

```javascript
// Only after explicit confirmation
shakudo-platform_deleteMicroservice({ id: "[service-id]", confirm: true })
```

### Restart after scale-to-zero

If the service config still exists (was scaled to zero, not deleted):

```javascript
shakudo-platform_scaleService({ id: "[service-id]", newReplicas: 1 })
// or
shakudo-platform_restartService({ id: "[service-id]" })
```

### Stop all running POCs

When user says "stop everything" or "scale down all POCs":

```javascript
// Search for each known POC name and scale all to zero
const pocs = ["dynex-mbs", "gallo", "hr-resume", "reagan", "campbell"]
// For each: searchMicroservice → get IDs → scaleService to 0
```

Always list what was stopped and confirm each.

---

## Execution Checklist

- [ ] ClickUp spec read — all required sections present (Phase 0)
- [ ] All external systems identified from spec
- [ ] n8n key checked and validated if spec uses n8n (Phase 1c)
- [ ] GitHub token confirmed valid
- [ ] Pre-flight summary shown — user confirmed before any code written
- [ ] Git structure follows standard: README, APP_INFO, run.sh, api/, ui/, skill/ (Phase 2)
- [ ] Code pushed to `robert-shakudo/{name}` first
- [ ] Mirrored to `devsentient/demos/{folder}/`
- [ ] Data files force-added if blocked by .gitignore (`git add -f *.csv`)
- [ ] `demos` git server confirmed IN_SYNC before deploy
- [ ] All services created with correct `script` paths
- [ ] n8n workflows created via API (or JSON exported if key skipped)
- [ ] All services polled to `running`
- [ ] E2E smoke test run — all 500s fixed before reporting URLs
- [ ] Live URLs returned with mock/real status + go-live credential path
- [ ] POC registry updated

---

## Anti-Patterns (learned from Dynex)

- **Never start building if spec is missing Build Brief** — validate Phase 0 before any code
- **Never discover a missing credential mid-flight** — Phase 1 runs before Phase 2, always
- **Never assume n8n key is valid** — always validate with a live `401/200` check
- **Never ignore `.gitignore` for data files** — demos repo blocks `*.csv`, force-add explicitly
- **Never use a different folder structure than existing demos** — README, APP_INFO, run.sh, skill/ are required
- **Never report live URLs before smoke tests pass** — fix 500s first
- **Never create n8n workflows with an expired key** — export JSON as fallback, never guess

---

## Related Skills

- **kaji-poc-spec-builder** — Phase 1: generate the spec this skill builds and deploys
- **shakudo-microservice** — Low-level platform reference
- **kaji-agentic-engineering-estimator** — Price the engagement after the POC is built
