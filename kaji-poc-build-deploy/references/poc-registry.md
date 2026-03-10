# POC Registry

Complete deployment configuration for all built POCs. Agents read this file to get exact service names, paths, ports, and env vars before deploying.

**Platform defaults (apply to all unless overridden):**
- Git server: `demos` (devsentient/demos, clones to `/tmp/git/monorepo/`)
- Environment: `basic-ai-tools-small`
- Branch: `main`
- Working dir: `/tmp/git/monorepo/`

---

## HR Resume Processor

**ClickUp Spec:** `86afrhqxy` → https://app.clickup.com/t/86afrhqxy  
**Client context:** Mountain Capital Partners (internal sales demo)  
**Category:** Workflow Automation / HR  
**Notion:** https://www.notion.so/shakudo/Kaji-Demos-318d36b5509080cd84edfa3e5fa4af98  
**GitHub (source):** https://github.com/robert-shakudo/hr-resume-demo  

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| Backend + Frontend (monolith) | `hr-resume-demo` | `hr-resume-demo/run.sh` | `8787` | https://hr-resume-demo.dev.hyperplane.dev |

### Deployment Call

```javascript
shakudo-platform_createMicroservice({
  name: "hr-resume-demo",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "hr-resume-demo/run.sh",
  parameters: []  // no external credentials needed for mock mode
})
```

### Mock vs Real Env Vars

| Variable | Mock (default) | Real (go-live) | Where to get it |
|---|---|---|---|
| `PAYCOM_API_KEY` | Not needed — 30 applicants are hardcoded JSON | Real Paycom OAuth token | Paycom Admin → API settings |
| `GOOGLE_CLIENT_ID` | Emails printed to console only | Google OAuth 2.0 client ID | GCP Console → OAuth consent screen |
| `GOOGLE_CLIENT_SECRET` | — | Google OAuth secret | GCP Console |
| `OPENAI_API_KEY` | Not needed — mock scoring engine | Real key for AI scoring | platform.openai.com |

> **POC default**: No env vars needed. App runs with 30 hardcoded Ski Lift Operator applicants, mock email sends, and rule-based scoring. Fully functional for a demo.

### Kaji Skill

- Skill name: `hr-resume-processor`
- Installed at: `/root/.claude/skills/hr-resume-processor/`
- Trigger: `@kaji show candidates`, `@kaji score all`, `@kaji email the top 5`
- Config var: `HR_APP_URL=https://hr-resume-demo.dev.hyperplane.dev`

### What the Demo Shows

1. Dashboard opens → 30 Ski Lift Operator applicants in "New" column
2. Click **Run AI Scoring** → candidates scored in real-time
3. Select Top 10 (score ≥ 75) → **Send Invites** bulk action
4. Side panel shows full resume + AI score breakdown
5. Candidate reply → **AI Reply** drafts the response

---

## Gallo Freight Exception Hub

**ClickUp Spec:** `86afvjudd` → https://app.clickup.com/t/86afvjudd  
**Client context:** E.J. Gallo Winery (supply chain demo)  
**Category:** Operational Assistant / Workflow Automation  
**GitHub (source):** https://github.com/robert-shakudo/gallo-freight-exception-hub  

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| FastAPI backend | `gallo-api` | `gallo-freight-exception-hub/api/run.sh` | `8787` | https://gallo-api.dev.hyperplane.dev |
| React UI | `gallo-ui` | `gallo-freight-exception-hub/ui/run.sh` | `8787` | https://gallo-ui.dev.hyperplane.dev |

> **Deploy order**: backend first, then UI. The UI bakes in the API URL at build time.

### Deployment Calls

```javascript
// 1. Backend first
shakudo-platform_createMicroservice({
  name: "gallo-api",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "gallo-freight-exception-hub/api/run.sh",
  parameters: [
    { key: "OPENAI_API_KEY", value: "sk-mock" }  // replace with real key for /analyze
  ]
})

// 2. Frontend (bakes in API URL)
// The UI's run.sh hardcodes: VITE_API_URL=https://gallo-api.dev.hyperplane.dev
// If deploying to a different subdomain, override via parameter:
shakudo-platform_createMicroservice({
  name: "gallo-ui",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "gallo-freight-exception-hub/ui/run.sh",
  parameters: []  // VITE_API_URL is hardcoded in run.sh to gallo-api.dev.hyperplane.dev
})
```

### Mock vs Real Env Vars

| Variable | Service | Mock (default) | Real (go-live) | Where to get it |
|---|---|---|---|---|
| `OPENAI_API_KEY` | gallo-api | App works without it (pre-seeded exceptions load; `/analyze` returns static mock) | Real key → `/analyze` generates live AI recommendations | platform.openai.com |
| `N8N_WEBHOOK_URL` | gallo-api | Falls back to direct SAP mock resolve | Real n8n webhook URL on Shakudo n8n instance | n8n admin → Workflow 2 webhook URL |
| `GALLO_API_KEY` | gallo-api | Not set → no auth required | Set to any secret string → skill must send `X-API-Key` header | Generate any secure string |
| `VITE_API_URL` | gallo-ui | `https://gallo-api.dev.hyperplane.dev` (hardcoded in run.sh) | Change run.sh if deploying to different URL | Edit `gallo-freight-exception-hub/ui/run.sh` |

> **POC default**: 3 pre-seeded exceptions (Chardonnay, Cab, Moscato) appear on startup. Approve flow works end-to-end with mock SAP/TMS resolution. No API key required.

### n8n Workflows (optional for POC)

Pre-built workflows already imported to n8n v2 at `https://n8n-v2.dev.hyperplane.dev`:
- **Workflow 1** (ID: `QTCqVPxK6JCyi5ef`) — Scheduled scan every 6h
- **Workflow 2** (ID: `5WKFCpCDo0mKzT2n`) — Approval fulfillment webhook

> Not required to activate for POC — exception approval still works via direct API fallback.

### Kaji Skill

- Skill name: `gallo-freight-exception-hub`
- Located: `gallo-freight-exception-hub/skill/SKILL.md` in demos repo
- Trigger: `@kaji show freight exceptions`, `@kaji approve exception 1`
- Config vars:
  ```
  GALLO_API_URL=http://hyperplane-service-[id].hyperplane-pipelines.svc.cluster.local:8787
  GALLO_API_KEY=  (leave blank unless set on API service)
  ```
  > Use internal cluster URL (bypasses Istio auth). Get `[id]` from `searchMicroservice`.

### What the Demo Shows

1. Dashboard: 3 open exceptions with severity (🔴🟡🟢), savings, AI recommendation preview
2. Click any card → detail: root cause, transfer route, cost table, confidence %, deadline
3. Approve → status moves through: Executing → Confirming → ✅ Resolved
4. Daily summary bar: exceptions processed, total savings captured
5. From Mattermost: `@kaji approve exception 1` → same flow, no UI needed

---

## Reagan NLP-to-SQL

**ClickUp Spec:** `86afrnky0` → https://app.clickup.com/t/86afrnky0  
**Client context:** Reagan Foundation (nonprofit fundraising demo)  
**Category:** Data & Analytics  
**GitHub (source):** https://github.com/robert-shakudo/reagan-nlp-sql-demo  

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| App (backend + Vanna UI) | `reagan-crm-chat` | `reagan-nlp-sql-demo/run.sh` | `8787` | https://reagan-crm-chat.dev.hyperplane.dev |

> Also deployed as `reagan-nlp-sql-backend` at `https://reagan-nlp-sql-backend.dev.hyperplane.dev` — same app, different subdomain.

### Deployment Call

```javascript
shakudo-platform_createMicroservice({
  name: "reagan-crm-chat",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-medium",      // needs libpq-dev + heavier dependencies
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "reagan-nlp-sql-demo/run.sh",
  parameters: [
    { key: "ANTHROPIC_API_KEY", value: "sk-ant-mock" }  // replace with real key
  ]
})
```

> ⚠️ Use `basic-medium` (not `basic-ai-tools-small`) — run.sh installs `libpq-dev` via apt-get, which requires more resources.

### Mock vs Real Env Vars

| Variable | Mock (default) | Real (go-live) | Where to get it |
|---|---|---|---|
| `ANTHROPIC_API_KEY` | App starts but NL→SQL calls return error | Real Claude key → full NL→SQL + response formatting | console.anthropic.com |
| `DATABASE_URL` | Mock PostgreSQL schema seeded with donor data (`reagan_crm` schema on internal Supabase) | Real Salesforce sync → live donor data | Client's Salesforce → sync script |

> **POC default**: App uses pre-seeded PostgreSQL donor data (300 mock donors, 5 campaigns, donation history). Vanna's built-in web UI handles chat. Natural language queries work end-to-end with mock data.

### Kaji Skill

- Skill name: `reagan-nlp-sql`
- Trigger: `@kaji ask reagan: [question]`, `@kaji top 5 donors`
- Config var: `REAGAN_API_URL=https://reagan-crm-chat.dev.hyperplane.dev`

### What the Demo Shows

1. Ask: "who are the top 10 donors by total amount?"
2. App generates SQL, executes, returns: natural language answer + data table + SQL shown
3. Follow-up: "which of those gave in 2025?" → contextual query
4. Ask: "which campaigns raised the most last year?" → campaign analytics
5. From Kaji: same queries in Mattermost, formatted as table

---

## Campbell & Company — Ops Co-Pilot

**ClickUp Spec:** `86afxwcn9` → https://app.clickup.com/t/86afxwcn9  
**Client context:** Campbell & Company (trading infrastructure demo)  
**Category:** Operational Assistant / Multi-Agent  
**GitHub (source):** https://github.com/robert-shakudo/campbell-ops-demo  

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| FastAPI + Alpine.js UI | `campbell-ops-demo` | `campbell-ops-demo/run.sh` | `8787` | https://campbell-ops-demo.dev.hyperplane.dev |

### Deployment Call

```javascript
shakudo-platform_createMicroservice({
  name: "campbell-ops-demo",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "campbell-ops-demo/run.sh",
  parameters: [
    { key: "ANTHROPIC_API_KEY", value: "sk-ant-mock" },
    { key: "CLICKUP_API_KEY", value: "mock" },
    { key: "MATTERMOST_WEBHOOK_URL", value: "" }
  ]
})
```

### Mock vs Real Env Vars

| Variable | Mock (default) | Real (go-live) | Where to get it |
|---|---|---|---|
| `ANTHROPIC_API_KEY` | Diagnosis returns static mock responses | Real Claude key → live AI root cause analysis | console.anthropic.com |
| `CLICKUP_API_KEY` | Ticket creation logs locally (no real task created) | Real key → ClickUp tasks created in Campbell list | ClickUp → Settings → API token |
| `MATTERMOST_WEBHOOK_URL` | Alerts post to console | Real webhook → #campbell-ops-alerts channel | Mattermost → Integrations → Incoming webhooks |
| `SUPABASE_URL` | SQLite fallback for incident storage | Real Supabase → `campbell_ops` schema | Supabase admin |
| `SUPABASE_KEY` | — | Supabase anon/service key | Supabase admin |

> **POC default**: 5 pre-defined failure scenarios inject on demand (Airflow, Bloomberg, TeamCity, risk-calculator, price-feed). Diagnosis and fix flow works end-to-end with static mock responses. Mattermost notifications require real webhook URL.

### Kaji Skill

- Skill name: `campbell-ops`
- Trigger: `@kaji what failed in Airflow`, `@kaji inject a Bloomberg failure`, `@kaji diagnose the risk calculator`
- Internal API: `http://hyperplane-service-[id].hyperplane-pipelines.svc.cluster.local:8787`

### What the Demo Shows

1. `@kaji inject a Bloomberg failure` → failure card appears in dashboard + Mattermost alert
2. `@kaji diagnose the Bloomberg incident` → AI synthesizes logs + Confluence runbook → returns root cause + diff
3. `@kaji apply the fix` → patch applied, build simulation runs, card moves to Resolved
4. Dashboard: live ops feed, 5 service status cards, savings counter ($310K total)
5. `@kaji create a ClickUp ticket for the price feed issue — assign to Jimmy`

---

## Dynex Capital — MBS Intelligence Platform

**ClickUp Spec:** `86ag1jmra` → https://app.clickup.com/t/86ag1jmra
**Client context:** Dynex Capital — fixed income / MBS portfolio analytics
**Category:** Data & Analytics / Operational Assistant
**GitHub (source):** https://github.com/robert-shakudo/dynex-mbs-demo

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| FastAPI backend | `dynex-mbs-api` | `dynex-mbs/api/run.sh` | `8787` | https://dynex-mbs-api.dev.hyperplane.dev |
| React frontend | `dynex-mbs-ui` | `dynex-mbs/ui/run.sh` | `8787` | https://dynex-mbs-ui.dev.hyperplane.dev |

### Deployment Calls

```javascript
// 1. Backend first
shakudo-platform_createMicroservice({
  name: "dynex-mbs-api",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "dynex-mbs/api/run.sh",
  parameters: [
    { key: "OPENAI_API_KEY", value: "sk-mock" },
    { key: "OTEL_EXPORTER_OTLP_ENDPOINT", value: "https://arize-phoenix.dev.hyperplane.dev" }
  ]
})

// 2. Frontend
shakudo-platform_createMicroservice({
  name: "dynex-mbs-ui",
  userEmail: process.env.USER_EMAIL,
  environment: "basic-ai-tools-small",
  gitServer: "demos",
  branch: "main",
  port: 8787,
  script: "dynex-mbs/ui/run.sh",
  parameters: []  // VITE_API_URL hardcoded to dynex-mbs-api.dev.hyperplane.dev
})
```

### Mock vs Real Env Vars

| Variable | Service | Mock (default) | Real (go-live) |
|---|---|---|---|
| `OPENAI_API_KEY` | dynex-mbs-api | Mock GPT-4o via LiteLLM — returns structured mock responses | Dynex Azure OpenAI key |
| `OPENAI_BASE_URL` | dynex-mbs-api | Not set (uses LiteLLM mock) | `https://[dynex-azure].openai.azure.com/` |
| `OPENAI_API_VERSION` | dynex-mbs-api | Not set | `2024-02-01` |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | dynex-mbs-api | Arize Phoenix on Shakudo dev | Client's Arize Phoenix instance |
| `N8N_WEBHOOK_URL` | dynex-mbs-api | `https://n8n-v2.dev.hyperplane.dev/webhook/briefing-approval` | Same (or client's n8n) |

> **POC default**: 20 MBS positions pre-seeded (`dynex_portfolio_q1_2026.csv`). Portfolio Intelligence, Briefing View, 10-Q Extraction, and Audit Trail all work with mock LLM responses. n8n approval flow requires valid n8n API key.

### n8n Workflows

- **Workflow:** Dynex MBS — Briefing Approval
- **Trigger:** `POST /webhook/briefing-approval`
- **Flow:** Webhook → Respond immediately → Wait 5s → POST approval back to API
- **CFO name in workflow:** Mike Sartori (CFO)

> ⚠️ Requires valid n8n API key at `/root/n8n-v2-api-key`. Key expires — validate before deploying.

### Known Issues (fixed)

- `*.csv` blocked by devsentient/demos `.gitignore` — force-add with `git add -f dynex-mbs/api/data/*.csv`
- `ReferenceError: BarChart3 is not defined` — fixed in current build (import added to PortfolioIntelligence.jsx)

### Kaji Skill

- Skill name: `dynex-mbs`
- Trigger: `@kaji show my MBS portfolio`, `@kaji analyze exposure to agency debt`, `@kaji generate briefing`
- Config var: `DYNEX_API_URL=https://dynex-mbs-api.dev.hyperplane.dev`

### What the Demo Shows

1. Portfolio Intelligence — analyst asks "show my top 5 MBS exposures" → ranked positions with dollar impact, ~3s
2. Briefing View — generate full daily briefing → "Submit for CFO Approval" triggers n8n approval flow
3. 10-Q Extraction — upload FNMA/FHLMC/GNMA PDF → structured pool table + CSV export
4. Audit Trail — Arize Phoenix iframe showing every LLM call logged

---

## Adding New POCs

When a new POC is built from a spec, add an entry to this file:

```markdown
## [Client] — [Use Case Name]

**ClickUp Spec:** [task ID] → [URL]
**Client context:** [1 line]
**Category:** [Operational Assistant / Analytics / Workflow Automation / Knowledge Assistant]
**GitHub (source):** [repo URL]

### Services

| Service | Name | Script | Port | Live URL |
|---|---|---|---|---|
| [role] | [name] | [folder/run.sh] | [port] | https://[name].dev.hyperplane.dev |

### Deployment Call
[javascript block]

### Mock vs Real Env Vars
[table]

### What the Demo Shows
[numbered steps]
```
