# Shakudo Platform Capabilities

Reference for mapping client use cases to Shakudo and Kaji platform components.

---

## Kaji — AI Agent Layer

**What it is:** The AI agent that understands natural language, reasons over context, uses tools, and delivers responses. Kaji is the intelligence layer in every demo.

**What it can do:**
- Understand and respond to natural language questions
- Make multi-step decisions (what to retrieve, what to do next)
- Call connected tools and skills, use the results, and compose a final response
- Maintain conversation context across a thread or session
- Be deployed in Mattermost (chat-native), a web UI, or API endpoint
- Run as an autonomous agent (given a goal, figures out the steps)

**Best for:** Any application where the user talks to the system in natural language

---

## Kaji MCP Skills (Tool Plugins)

**What they are:** Individual skills that give Kaji the ability to interact with external systems. Each skill is a defined tool with inputs, outputs, and a clear purpose.

**What they can do:**
- Query databases (SQL, NoSQL, vector)
- Call REST APIs (GET, POST, PATCH, DELETE)
- Read and write files or documents
- Trigger workflows (n8n, webhooks)
- Search internal knowledge bases (RAG)
- Perform computations or data transformations

**Existing integrations (available now):**
- **Dremio** — SQL queries over data lakes
- **Fireflies** — Meeting transcripts and summaries
- **Mattermost** — Read/write channel messages
- **ClickUp** — Task management (read/write/create)
- **Notion** — Page retrieval and updates
- **Neo4j** — Graph database queries
- **Supabase** — PostgreSQL queries
- **GitHub** — Repo browsing, file reads
- **HubSpot** — CRM records and contacts
- **Graphiti Memory** — Persistent knowledge graph
- **Gamma** — Presentation generation

**Mockable skills (build in 1–2 days):**
- Any REST API with JSON responses
- Any SQL database (seed with mock data)
- Any webhook-triggered action

**Best for:** Any application that needs to retrieve or act on data from an external system

---

## Kaji RAG (Retrieval-Augmented Generation)

**What it is:** Document ingestion, indexing, and semantic search. Kaji can retrieve relevant sections from uploaded documents and use them to answer questions.

**What it can do:**
- Ingest PDFs, Word docs, Markdown files, web pages
- Chunk and embed documents for semantic search
- Return the most relevant passages for a given question
- Summarize documents or answer specific questions from content
- Combine retrieved content with LLM reasoning for accurate answers

**Best for:** Knowledge assistants, policy Q&A, document summarization, support knowledge bases

---

## Shakudo Microservice

**What it is:** A containerized application deployed on the Shakudo platform. Can be an API backend, a web frontend, or both.

**What it can do:**
- Host a FastAPI or Flask backend with custom business logic
- Serve a React, Next.js, or plain HTML/JS frontend
- Expose REST API endpoints for skills or external tools to call
- Run background jobs, processing pipelines, or scheduled tasks
- Store and serve data (SQLite, PostgreSQL, Redis)

**Build time:** 1–3 days for a basic API + UI demo
**Best for:** Custom UI demos, API backends, standalone applications

---

## n8n on Shakudo

**What it is:** Low-code workflow automation platform hosted on Shakudo. Connects systems, triggers actions, and orchestrates multi-step processes.

**What it can do:**
- Trigger workflows on events (webhook, schedule, message received)
- Chain actions across multiple systems
- Transform and route data between systems
- Approval flows (wait for human input, then proceed)
- Error handling and retry logic
- Integrate with 400+ services via built-in connectors

**Build time:** 1–2 days per workflow
**Best for:** Approval workflows, event-triggered automations, system-to-system data pipelines

---

## Dremio (Data Layer)

**What it is:** SQL query engine that federates queries across data lakes, warehouses, and databases without moving data.

**What it can do:**
- Query Parquet, CSV, JSON files in S3 or GCS
- Connect to PostgreSQL, MySQL, SQL Server
- Join data across sources in a single query
- Expose a SQL interface for Kaji skills
- Handle large datasets with pushdown optimization

**Best for:** Analytics, NLP-to-SQL, data exploration applications

---

## Shakudo Notebook / JupyterHub

**What it is:** Managed Jupyter environment on the Shakudo platform.

**What it can do:**
- Run Python, R, or Scala notebooks
- Train and evaluate ML models
- Build and test data pipelines
- Expose analysis results to other platform components

**Best for:** ML demos, data science workflows, model evaluation

---

## Platform Integrations (Active MCPs)

These are live, working integrations available on the Shakudo platform right now:

| Integration | What Kaji Can Do |
|-------------|-----------------|
| Salesforce | Read accounts, contacts, opportunities (via custom MCP) |
| ServiceNow | Read/create incidents and requests |
| Slack | Post messages, read channels (beta) |
| Microsoft Teams | Post messages (beta) |
| Jira | Read/write issues and sprints |
| Confluence | Read pages and spaces |
| SAP | Read order and inventory data (via custom connector) |
| Snowflake | SQL queries via Dremio bridge |
| AWS S3 | File reads and writes |
| Azure Blob | File reads and writes |
| Google Workspace | Docs, Sheets, Drive (read) |

> Note: Mark as "available" only if confirmed active for the client's Shakudo instance. When in doubt, mark as 🟡 Mockable.

---

## What Kaji Demos Best

Based on demos built to date, these patterns show the strongest client response:

1. **Chat-native interface** — user asks a question in natural language, gets a complete answer with data. No dashboards, no forms.
2. **Action-with-confirmation** — user says "approve this", Kaji shows what it will do, user confirms, action executes.
3. **Multi-source synthesis** — Kaji pulls from 2–3 systems and delivers a single coherent answer ("here's what your CRM, ticketing, and monitoring all say about this account").
4. **Workflow trigger from chat** — user types a request, Kaji triggers an n8n workflow, and reports back when complete.
5. **Investigation assistant** — user describes a problem, Kaji investigates across systems and returns a root cause + recommendation.
