# POC Architecture & Build Plan — [App Name]

> Output this alongside the app spec for every Full Spec request. This is the architecture decision package that `kaji-poc-build-deploy` should be able to consume.

---

## Architecture Options Considered

Compare at least 2 options. For non-trivial use cases, compare 3.

| Option | Interaction Model | Core Components | Why It Could Work | Why It Might Be Wrong | Verdict |
|---|---|---|---|---|---|
| Option A | [chat-first / UI-first / workflow-first / hybrid] | [Kaji, microservice, n8n, Dremio, RAG, etc.] | | | |
| Option B | | | | | |
| Option C *(optional)* | | | | | |

---

## Recommended Build Strategy

**App Type:** [Operational Assistant / Data & Analytics / Workflow Automation / Knowledge Assistant / Multi-Agent / Hybrid]

**Primary Interaction Model:** [chat-first / UI-first / workflow-first / analytics-first / hybrid]

**Recommended Architecture:**
[One paragraph describing the chosen architecture in plain English. Example: “Kaji handles the natural-language workflow, a FastAPI microservice owns business logic and state, and n8n handles the approval callback.”]

**Best Component Stack:**
- [Component]
- [Component]
- [Component]

**Closest Existing Demo to Reuse:**
[Name of existing demo] — [why it is the best reference]

**Components to Build Now:**
- [Component / service]
- [Component / service]

**Components to Mock Now:**
- [System / workflow / dependency]
- [System / workflow / dependency]

**Components to Defer to Year 2:**
- [Enhancement]
- [Enhancement]

**Why this is the best POC choice:**
[2–4 sentences. Focus on credibility, speed to value, and why it is easier to demo than the rejected alternatives.]

---

## Component Selection Rationale

| Need / Function | Recommended Component | Why This Component Fits | Build Now or Later |
|---|---|---|---|
| Natural language interface | Kaji | [why] | Now |
| [Need] | [Component] | [why] | [Now / Later] |
| [Need] | [Component] | [why] | [Now / Later] |

Add a short note on rejected choices:

**Alternatives rejected:**
- [Rejected component or pattern] — [why it was not chosen]
- [Rejected component or pattern] — [why it was not chosen]

---

## Mock / Real System Map

| System | Type | Status | Mock Strategy | Real Integration Path | What Changes on Go-Live | Owner |
|---|---|---|---|---|---|---|
| [System] | [CRM / ERP / DB / API] | ✅ / 🟡 / 🔴 | | | | |
| [System] | | | | | | |

**Status key:**
- ✅ **Available now** — live integration or public data is enough
- 🟡 **Mocked** — realistic fake for the POC, swappable later
- 🔴 **Needs build** — custom connector or extra work required

### Environment Table

| Phase | Environment |
|---|---|
| Build & POC | `dev.hyperplane.dev` (Shakudo GCP) |
| Go-Live | [Client cloud / on-prem / TBD] |

---

## Shakudo Build Stack (Platform Map)

| App Function | Shakudo Component | Role in the POC | Why It Is Included Now |
|---|---|---|---|
| [Function] | [Kaji / MCP Skill / Microservice / n8n / Dremio / RAG / Notebook / Phoenix / etc.] | | |
| [Function] | [Component] | | |
| [Function] | [Component] | | |

---

## Service Deployment Specs

Provide one block per service. These should be explicit enough for `shakudo-microservice-lite`.

```yaml
- jobName: [service-name]
  subdomain: [service-subdomain]
  jobType: [basic-ai-tools-small / basic-medium / etc.]
  gitServerName: [resolve from repo remote, often demos]
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: [app-folder]/[service]/run.sh
  port: "8787"
  deployOrder: 1
  parameters:
    - key: [ENV_VAR]
      default: [mock-value-or-empty]
  smokeTests:
    - path: /health
      expected: 200
```

If using the app-directory pattern instead, make that explicit:

```yaml
workingDir: /tmp/git/monorepo/[app-folder]/
pipelineYamlPath: run.sh
```

---

## What the POC Shows

[2–3 sentences describing the user story, wow moment, and what gets demonstrated in the room.]

---

## 5-Minute Demo Script

| Step | What the User Does | What the App Does | Talking Point |
|---|---|---|---|
| 1 | Opens the app / Kaji chat | — | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

---

## Components to Build

| Component | Type | Build Time | Notes |
|---|---|---|---|
| [Component] | [Kaji skill / microservice / UI / workflow] | [Xh / Xd] | |
| [Component] | | | |

**Total Estimated Build Time:** [X days]

---

## Mock Data Required

| Data Set | Format | Volume | Who Creates It |
|---|---|---|---|
| [Dataset] | [JSON / CSV / SQL seed] | [volume] | [owner] |
| [Dataset] | | | |

---

## Go-Live Requirements

| Requirement | Type | Notes |
|---|---|---|
| [Credential / access / infra item] | [Credentials / Data access / Approval / Security] | |
| [Requirement] | | |

---

## Architecture Diagram (Text)

```text
User (Mattermost / Web UI / Analyst Screen)
        ↓
Primary Interface Layer
        ↓
   Kaji / UI / Workflow Controller
        ↓
┌──────────────────────────────────┐
│ Core Components                  │
│  • [Component 1]                 │
│  • [Component 2]                 │
│  • [Component 3]                 │
└──────────────────────────────────┘
        ↓
[Mock Systems / Real Systems / Data Layer]
```

**POC simplification note:**
[Explain what is intentionally mocked or deferred for speed.]
