# Shakudo Platform Capabilities

Reference for selecting the best Shakudo component stack for a client POC.

Use this file together with [Component Selection Rubric](./component-selection-rubric.md). The goal is not to list every tool the platform has — it is to choose the smallest, strongest combination that tells the story well.

---

## POC-first selection sequence

When designing a new app, make the decision in this order:
1. **Choose the primary user surface** — chat, UI, workflow trigger, or analyst workspace.
2. **Choose the core execution layer** — Kaji, microservice, workflow engine, analytics engine, or retrieval layer.
3. **Choose the data access pattern** — SQL, documents, graph, APIs, or mixed.
4. **Add orchestration only if needed** — approvals, callbacks, or event-driven steps.
5. **Add observability or memory only when they strengthen the story.**

If the architecture cannot be explained in five lines, it is probably too large for the POC.

---

## Core Shakudo Components

### Kaji — AI agent layer

**What it is:** Natural-language interface, reasoning layer, tool orchestration, and response composition.

**Best for:**
- conversational assistants
- investigation workflows
- action-with-confirmation
- executive briefing or summary generation
- human-in-the-loop agent experiences

**Choose Kaji when:**
- the user naturally asks questions in language
- the system needs to synthesize across tools or data sources
- the interaction should feel like an assistant rather than a dashboard

**Avoid making Kaji the whole app when:**
- the core workflow is mostly form entry, visual review, or bulk tabular manipulation

---

### Kaji MCP Skills

**What they are:** Tool connections that let Kaji read or act on external systems.

**Best for:**
- CRM / ticketing / document / messaging integrations
- database queries
- external system actions
- file reads and writes

**Choose MCP skills when:**
- the app needs direct system access from Kaji
- the system already exists and we want to query or trigger it
- the value comes from connecting two or more sources quickly

**Common examples:** ClickUp, Notion, Mattermost, Dremio, Neo4j, HubSpot, Supabase, GitHub, Fireflies.

---

### Shakudo Microservice

**What it is:** Deployable backend or frontend service running custom code on Shakudo.

**Best for:**
- custom APIs
- business logic that should not live in prompt instructions
- React / Next / static UI frontends
- backend state and custom endpoints
- scheduled or asynchronous jobs

**Choose a microservice when:**
- the app needs a stable API or custom runtime
- there is a non-trivial UI
- business logic needs code, not just prompt orchestration

**Avoid overusing it when:**
- a pure Kaji + MCP flow is enough for the POC story

---

### n8n on Shakudo

**What it is:** Workflow and orchestration layer for approvals, callbacks, and multi-step automations.

**Best for:**
- approval flows
- event-triggered automations
- notifications and webhooks
- waiting on human input
- stitching together multiple systems without writing all control logic in code

**Choose n8n when:**
- the workflow is step-based and externally observable
- the value comes from orchestration, not deep computation
- approvals or callbacks are a visible part of the user story

**Do not choose n8n as the main place for:**
- heavy business logic
- complex domain reasoning that belongs in app code

---

### Dremio / SQL Layer

**What it is:** Federated SQL access over structured data.

**Best for:**
- NL-to-SQL
- analytics copilots
- reporting and BI-like queries
- structured data exploration

**Choose Dremio when:**
- the primary value is over structured tables
- the user wants queries, metrics, comparisons, trends, and drill-downs

**Do not default to RAG when:**
- the source of truth is really tables, not documents

---

### Kaji RAG

**What it is:** Retrieval over documents, policies, manuals, and other unstructured content.

**Best for:**
- knowledge assistants
- policy or SOP Q&A
- document-grounded answers
- meeting or research synthesis grounded in files

**Choose RAG when:**
- the most important evidence is in documents or narrative text
- the app must cite or quote relevant content

**Do not choose RAG as the main pattern when:**
- the app is really operational analytics over structured data

---

### Graph Memory / Neo4j / Graphiti

**What it is:** Graph-structured memory, relationships, and knowledge traversal.

**Best for:**
- relationship-heavy knowledge problems
- entity linking across calls, threads, issues, and documents
- persistent memory across sessions

**Choose graph memory when:**
- relationships matter as much as the content itself
- the app needs memory beyond a single conversation

**Do not add it just because it sounds agentic.** Use it when relationship structure clearly matters.

---

### Notebook / JupyterHub

**What it is:** Analyst and data-science workspace.

**Best for:**
- model exploration
- data analysis workbenches
- evaluation workflows
- internal analyst tools

**Choose notebooks when:**
- the user is an analyst or data scientist
- interactivity and code-first exploration matter more than a polished app UI

---

### Observability — Langfuse / Arize Phoenix

**What it is:** Tracing, prompt observability, and model behavior inspection.

**Best for:**
- agentic flows that need inspection
- demos where showing traceability adds trust
- debugging multi-step LLM calls

**Choose observability when:**
- LLM behavior is central to the value proposition
- debugging or trust is important for the room

---

## Supporting surfaces

### Mattermost or chat surface

Best when the user already works in chat and does not need a rich custom UI.

### Web UI

Best when the user must:
- review lists, cards, or dashboards
- compare records visually
- fill forms
- inspect state over time

### Email / SMS / notifications

Best as outputs or side effects, rarely as the primary interface.

---

## How to choose the primary user surface

| Situation | Best default |
|---|---|
| User mainly asks questions or requests actions | Kaji chat |
| User reviews dashboards, queues, or records | Web UI + API |
| User approves or routes work across systems | Kaji + n8n or UI + n8n |
| User explores metrics and asks analytical questions | Kaji + Dremio or analytics UI + Dremio |
| User works from docs and policies | Kaji + RAG |
| User needs a code-first exploration surface | Notebook + optional Kaji |

**Best practice:** pick one primary surface, then add a secondary one only if it materially strengthens the POC.

---

## How to choose the data pattern

| Data shape | Best default components |
|---|---|
| Structured relational data | Dremio / SQL + Kaji or UI |
| Documents, PDFs, SOPs, policies | Kaji + RAG |
| Operational events and approvals | Kaji / UI + microservice + n8n |
| Mixed operational + structured data | Microservice + Dremio + Kaji |
| Relationship-heavy knowledge | Kaji + graph memory / Neo4j |

---

## How to choose the action model

| Action model | Best default |
|---|---|
| Read-only assistant | Kaji + MCP skills |
| Human-in-the-loop action | Kaji or UI + microservice + optional n8n |
| Background automation | n8n + microservice |
| Investigation and synthesis | Kaji + MCP skills + optional observability |
| Executive briefing | Kaji + data/doc layer + optional UI export |

---

## Default build stacks by app type

### Operational Assistant

**Recommended default stack:**
- Kaji
- MCP skills
- microservice for business logic and state
- optional UI if users must review queues or status
- optional n8n for approval or callback flows

**Strong examples:** Gallo, Campbell

**Usually mock first:** downstream writebacks, approvals, or niche APIs.

---

### Data & Analytics Copilot

**Recommended default stack:**
- Kaji or lightweight analytics UI
- Dremio / SQL layer
- microservice only if custom logic or formatting is needed
- optional observability if LLM query generation is central

**Strong examples:** Reagan, Dynex

**Usually mock first:** warehouse credentials or downstream exports.

---

### Workflow Automation POC

**Recommended default stack:**
- Kaji or UI as initiation surface
- n8n for orchestration
- microservice for state, APIs, and callback handling
- mocked external systems where possible

**Strong examples:** HR Resume, Gallo approvals

**Usually mock first:** final side effects in external systems.

---

### Knowledge Assistant

**Recommended default stack:**
- Kaji
- RAG
- optional microservice if custom retrieval or auth is needed
- optional UI only if document browsing materially improves the story

**Usually mock first:** private corpora and auth edge cases.

---

### Multi-Agent System

**Recommended default stack:**
- Kaji
- distinct tool / role boundaries
- microservice for orchestration or shared state
- observability strongly recommended

**Use only when:**
- there are truly separate roles (planner, analyst, executor, reviewer)
- the workflow clearly benefits from separation

**Do not use when:**
- a single agent with tools can do the job just as well

---

## Common combinations that work well

| Pattern | Good combination | Why it works |
|---|---|---|
| Chat-first ops copilot | Kaji + MCP skills + microservice | Natural language + tool access + stable business logic |
| Analytics assistant | Kaji + Dremio + optional microservice | Structured insight without overbuilding |
| Approval workflow | Kaji or UI + microservice + n8n | Clear separation of reasoning, state, and orchestration |
| Knowledge assistant | Kaji + RAG + optional UI | Strong grounding without forcing a data warehouse |
| Agentic workflow with trust needs | Kaji + tools + observability | Easier debugging and better demo trust |

---

## What to mock first for a POC

Prefer mocking in this order:
1. downstream side effects (ticket creation, CRM updates, emails)
2. hard-to-get credentials
3. long-tail integrations that do not change the core story
4. advanced approval routing rules

Avoid mocking the core user experience or the primary reasoning path unless there is no other option.

---

## Best practices

- Prefer **reuse** of an existing demo pattern over a fresh architecture.
- Prefer **one primary surface**.
- Prefer **explicit mock / real separation**.
- Prefer **fewer moving parts** for the first build.
- Prefer **stable app code** for business logic and **n8n** for orchestration.
- Prefer **Dremio / SQL** for structured analytics and **RAG** for documents.
- Add **observability** when LLM behavior is a core part of the story.
- Add **graph memory** only when relationship structure is core to the problem.

---

## Anti-patterns

- choosing multi-agent because it sounds advanced
- adding both a full UI and a deep chat assistant without a clear reason
- using RAG for a use case that is really just SQL analytics
- using n8n for all core logic
- adding too many integrations to the POC when mocks prove the value just as well
- listing components without a reason tied to user value
- adding memory or observability with no concrete role in the demo
