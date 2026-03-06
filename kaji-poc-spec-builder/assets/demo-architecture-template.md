# Demo Architecture — [App Name]

> Output this alongside the App Spec for every Full Spec + POC Architecture request.

---

## Mock / Real System Map

For each system in the spec, define the mock strategy and real integration path.

| System | Type | Demo Status | Mock Strategy | Real Integration Path | Owner |
|--------|------|-------------|--------------|----------------------|-------|
| [System] | [CRM/ERP/DB/API] | ✅ Available | Live integration | — | Shakudo |
| [System] | [Type] | 🟡 Mocked | [How we fake it] | [What real looks like] | [Client / Shakudo] |
| [System] | [Type] | 🔴 Custom Build | N/A — needs build | [What real looks like] | [Eng estimate needed] |

**Demo Status Key:**
- ✅ **Available Now** — Shakudo has a live MCP or integration for this system
- 🟡 **Mockable** — Realistic mock can be built in 1–2 days (seeded DB, hardcoded API, sample JSON)
- 🔴 **Needs Custom Build** — New integration required; needs client credentials or custom connector (5–10 days)

---

## Shakudo Platform Map

Map each capability from the spec to the Shakudo component that delivers it.

| Capability | Shakudo Component | Role in Demo |
|------------|-------------------|--------------|
| Natural language interface | Kaji (chat agent) | User asks questions, Kaji responds |
| [Capability] | [Component] | [What it does in this demo] |
| [Capability] | [Component] | [What it does in this demo] |
| [Capability] | [Component] | [What it does in this demo] |

**Component Reference:**
- **Kaji** — AI agent layer (chat interface, reasoning, tool calling)
- **Kaji MCP Skills** — Tool plugins that give Kaji access to systems
- **Kaji RAG** — Document/knowledge search and retrieval
- **Shakudo Microservice** — API backend or web frontend deployed on Shakudo
- **n8n on Shakudo** — Workflow automation, event triggers, multi-step pipelines
- **Dremio** — SQL query layer over data lakes and databases
- **Shakudo Notebook** — Data exploration, ML model, analysis environment

---

## Demo Build Brief

### What the Demo Shows

[2–3 sentences describing the demo scenario. What does the user do? What does the app do? What's the "wow moment"?]

### 5-Minute Demo Script

| Step | What the User Does | What the App Does | Talking Point |
|------|-------------------|-------------------|---------------|
| 1 | Opens the app / Kaji chat | — | "Here's what we built in [X days]" |
| 2 | [User action] | [App response] | [Value statement] |
| 3 | [User action] | [App response] | [Value statement] |
| 4 | [User action] | [App response] | "In production, this connects to your real [system]" |
| 5 | [User action] | [App response] | "To go live, you give us [what client needs to provide]" |

### Components to Build

| Component | Type | Build Time | Notes |
|-----------|------|------------|-------|
| [Component name] | Kaji skill / Microservice / n8n workflow / UI | [Xh / Xd] | [Any notes] |
| [Component name] | [Type] | [Time] | |
| [Component name] | [Type] | [Time] | |

**Total Estimated Build Time:** [X days]

### Mock Data Required

| Data Set | Format | Volume | Who Creates It |
|----------|--------|--------|---------------|
| [e.g., Sample CRM records] | [JSON / CSV / SQL seed] | [e.g., 50 records] | [Shakudo eng] |
| [e.g., Mock API responses] | [Hardcoded JSON] | [e.g., 10 scenarios] | [Shakudo eng] |

### Go-Live Requirements

What the client needs to provide to replace mocks with real systems:

| Requirement | Type | Notes |
|-------------|------|-------|
| [e.g., Salesforce API credentials] | Credentials | [Any access level or permission notes] |
| [e.g., ERP read access for [schema]] | Data access | [What queries we need to run] |
| [e.g., Slack app approval] | Integration setup | [Steps to install Slack app] |

### Closest Existing Demo

[Name of the existing demo from the Demo Library that most closely matches this POC]

**What to reuse:** [Which components, patterns, or code to copy from that demo]

**What's different:** [What needs to be customized or built fresh]

---

## Architecture Diagram (Text)

```
User (Mattermost / Web UI)
        ↓
   Kaji Agent
        ↓ (tool calls)
┌────────────────────────────────┐
│ Skills / MCP Tools             │
│  • [Skill 1] → [System/Mock]   │
│  • [Skill 2] → [System/Mock]   │
│  • [Skill 3] → [System/Mock]   │
└────────────────────────────────┘
        ↓ (optional)
  n8n Workflow / Microservice
        ↓
  [Backend System / Mock DB]
```

**Mock → Real Swap:**
When ready, replace `[System/Mock]` with the live system integration. No other changes required.
