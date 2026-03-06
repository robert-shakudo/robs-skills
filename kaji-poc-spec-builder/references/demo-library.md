# Demo Library

Reference of existing POCs built on the Shakudo platform. Check here before designing a new demo from scratch — reusing an existing demo saves 50–80% of build time.

---

## How to Use This Library

1. Match the client's use case to the closest demo below
2. Note which components to reuse and what needs to be customized
3. Reference the demo in the POC build brief as the starting point

---

## Demo: Gallo Freight Exception Hub

**Client:** E.&J. Gallo Winery
**Category:** Operational Assistant / Workflow Automation
**Status:** Built and deployed

**What it does:**
Supply chain operations team queries open freight exceptions (inventory stockouts, distribution delays), reviews AI-analyzed recommendations, and approves or rejects transfer orders — all from a Mattermost thread.

**Stack:**
- Kaji (chat-native interface via Mattermost)
- FastAPI backend (Shakudo microservice)
- n8n (approval workflow → SAP order + TMS booking)
- Mocked SAP ERP + mocked TMS (replaceable)
- React UI (secondary interface)

**Core demo flow:**
1. User asks "show open freight exceptions"
2. Kaji returns ranked list with severity + savings potential
3. User asks "approve exception 1"
4. Kaji confirms action, n8n fires SAP + TMS booking
5. Summary: "$34,800 saved today"

**Best reused for:**
- Any supply chain / inventory / logistics use case
- Any approval workflow (human-in-the-loop pattern)
- Any use case where actions trigger downstream ERP/system writes
- Operations teams that triage queues of exceptions or alerts

**What to customize:** Mock data (SKUs, DCs, amounts), domain language (freight → any operational exception type), connected systems.

---

## Demo: Reagan Foundation NLP-to-SQL

**Client:** Reagan Foundation
**Category:** Data & Analytics / Knowledge Assistant
**Status:** Built and deployed (live Salesforce data)

**What it does:**
Development and fundraising team asks natural language questions about donor data — "who are the top 10 donors?", "which campaigns raised the most last year?" — and gets formatted answers with underlying SQL.

**Stack:**
- Kaji + Vanna AI (NLP-to-SQL)
- FastAPI backend (Shakudo microservice)
- PostgreSQL (mock CRM → live Salesforce via sync)
- Claude Sonnet (SQL generation + response formatting)

**Core demo flow:**
1. User asks any donor/campaign question in natural language
2. Vanna generates SQL, executes against PostgreSQL
3. Response includes: natural language answer + data table + SQL shown
4. Streaming SSE or REST JSON response

**Best reused for:**
- Any database with a known schema (HR, finance, operations, sales)
- Any client that wants to query their data without writing SQL
- Analytics assistants, reporting copilots, data exploration tools

**What to customize:** Schema (swap donor tables for whatever the client has), mock seed data, question examples, Vanna training questions.

---

## Demo: HR Resume Processor

**Client:** Internal / Demo
**Category:** Workflow Automation / Operational Assistant
**Status:** Built and deployed

**What it does:**
HR team uploads resumes or receives applications, Kaji scores candidates against job requirements, sends personalized interview invites, and manages applicant state — all from a Mattermost thread.

**Stack:**
- Kaji (chat-native, Mattermost)
- FastAPI backend with document processing
- Custom HR Resume Processor MCP skill
- Mattermost (interface + notifications)
- Mock ATS (replaceable with Greenhouse, Lever, Workday)

**Core demo flow:**
1. User asks "show me candidates for the [role]"
2. Kaji returns scored list with fit summary
3. User says "send interview invite to [candidate]"
4. Kaji confirms, sends invite, updates applicant state
5. User can book interviews and track pipeline from chat

**Best reused for:**
- Any HR or recruiting workflow
- Any document processing + scoring use case
- Any workflow where humans review AI-ranked items and take action

**What to customize:** Job descriptions, scoring criteria, applicant data, downstream ATS integration.

---

## Demo: Campbell / Martinrea Ops Demo

**Client:** Campbell / Martinrea
**Category:** Operational Assistant / Multi-source Investigation
**Status:** Built (demo-ready)

**What it does:**
Operations and plant management ask questions about production issues, equipment status, and supply chain delays. Kaji investigates across mocked ERP, ticketing, and monitoring systems and returns a synthesized answer.

**Stack:**
- Kaji (agent with multi-tool calling)
- FastAPI backend
- Mocked ERP + ticketing + monitoring (seeded JSON)
- Shakudo microservice (UI)

**Best reused for:**
- Manufacturing, industrial, or plant operations use cases
- Any "investigate a problem" pattern (root cause, status, what happened)
- Multi-source synthesis demos (Kaji queries 3 systems and gives one answer)

**What to customize:** Domain (manufacturing → logistics, healthcare, utilities), mock data, system names.

---

## Pattern Library (Without Named Demos)

Use these as starting points when no existing demo matches closely enough.

### Pattern: Chat-Native Operational Assistant

**Use when:** Client has a team that manages a queue of something (exceptions, tickets, alerts, requests) and makes decisions.

**Core components:**
- Kaji in Mattermost or web chat
- MCP skill(s) querying the queue
- Action skill for approving/rejecting/escalating
- n8n for downstream system writes

**Build time:** 2–3 days (mocked)

---

### Pattern: NLP-to-SQL Analytics

**Use when:** Client has structured data and wants natural language querying.

**Core components:**
- Kaji + Vanna AI or Claude NL→SQL
- Dremio or PostgreSQL
- FastAPI backend
- Optional: React table/chart UI

**Build time:** 2–3 days (mocked schema + seed data)

---

### Pattern: Document Intelligence / RAG

**Use when:** Client has documents (policies, manuals, SOPs, reports) that users need to search or query.

**Core components:**
- Kaji RAG with document ingestion
- 5–10 sample documents
- Optional: web UI for document browsing

**Build time:** 1–2 days (with sample docs)

---

### Pattern: Workflow Automation with Human-in-the-Loop

**Use when:** Client has a multi-step process with a human decision point before a system action.

**Core components:**
- Kaji (presents options, receives decision)
- n8n (orchestrates the workflow)
- Notification via Mattermost or email
- Mock system write (becomes real write on go-live)

**Build time:** 2–4 days depending on workflow complexity

---

### Pattern: Multi-Agent Investigation

**Use when:** Client needs to diagnose complex problems that span multiple systems or require multi-step reasoning.

**Core components:**
- Kaji as orchestrator agent
- 2–4 specialist sub-agents or MCP skills
- Each skill queries one system
- Kaji synthesizes all responses into final answer

**Build time:** 3–5 days (mocked systems)

---

## Adding New Demos

When a new POC is built and deployed, add an entry here with:
- Client name and date
- Category (operational assistant / analytics / workflow automation / knowledge assistant)
- What it does (3–4 sentences)
- Stack (components used)
- Core demo flow (numbered steps)
- Best reused for (when to reference this demo)
- What to customize
