# Mock Patterns

Standard strategies for mocking systems in POC demos. Every mock is designed to be swappable — replacing the mock with a real system requires changing a config value or environment variable, not rewriting the code.

---

## The Mock/Real Toggle Principle

Every mocked system follows this pattern:

```python
# Environment-driven toggle
DATA_SOURCE = os.getenv("DATA_SOURCE", "mock")  # "mock" or "live"

if DATA_SOURCE == "live":
    data = live_system.query(...)
else:
    data = mock_data.get(...)
```

The demo runs on mock data. When the client is ready, they provide credentials, we set `DATA_SOURCE=live`, and the app works with their real systems. Same code, same UI, same agent behavior.

---

## Mock Patterns by System Type

### CRM (Salesforce, HubSpot, Dynamics)

**Mock strategy:** Seeded JSON or SQLite database with realistic records (accounts, contacts, opportunities, cases).

**What to include in mock data:**
- 10–20 accounts with realistic names and industries
- 3–5 contacts per account
- 5–10 open opportunities or cases with varied statuses
- Enough data to demonstrate filtering, ranking, and lookup

**File format:** `mock_crm.json` or `mock_crm.db` (SQLite)

**Real integration path:** MCP skill calling the CRM's REST API with client credentials. Existing HubSpot MCP available on platform. Salesforce via custom connector.

---

### Ticketing / ITSM (ServiceNow, Jira, Zendesk)

**Mock strategy:** In-memory list of tickets or seeded database. Include a mix of open, in-progress, and resolved tickets with realistic metadata (priority, assignee, system affected, timestamps).

**What to include in mock data:**
- 20–30 tickets across 3–5 categories
- Varied priorities (P1/P2/P3)
- Some with SLA breach dates (for demo impact)
- Realistic ticket descriptions that mirror client's actual language

**File format:** `mock_tickets.json` or `mock_tickets.db`

**Real integration path:** ServiceNow REST API or Jira REST API via MCP skill. Both available as Shakudo connectors.

---

### ERP / Operations (SAP, Oracle, NetSuite)

**Mock strategy:** Hardcoded API response JSON files that match the real system's response schema. The skill calls the mock endpoint which returns the JSON files.

**What to include:**
- Inventory records with stock levels and locations
- Order records with status and line items
- Employee/cost center data (if needed)

**File format:** `mock_erp_responses/` directory with one JSON per endpoint

**Real integration path:** Custom SAP connector or Oracle REST API. Flag for engineering estimate — these take 3–5 days to build properly.

---

### Monitoring / Observability (Datadog, Splunk, PagerDuty, CloudWatch)

**Mock strategy:** Time-series data generator that produces realistic metrics (CPU, latency, error rates) with an "incident" baked in. Serve via a simple FastAPI endpoint that mimics the real API shape.

**What to include:**
- 24–48 hours of metric data
- One clear anomaly (spike, drop, breach)
- Alert state with timestamps

**File format:** FastAPI mock service in `mock_services/monitoring_mock.py`

**Real integration path:** Datadog API or Splunk REST API via MCP skill. PagerDuty has a standard webhook integration.

---

### Document / Knowledge Base (Confluence, SharePoint, Google Drive, PDFs)

**Mock strategy:** Upload 5–10 realistic documents (policies, SOPs, reports) into the RAG index. Use real document structures but anonymize or lightly modify content.

**What to include:**
- Documents that reflect the client's domain (use public versions of similar docs if needed)
- Enough variety to demonstrate search + summarization

**File format:** Markdown files or PDFs ingested into Kaji RAG

**Real integration path:** Client grants read access to their Confluence space or SharePoint library. Kaji ingests on a schedule or on-demand.

---

### Database (PostgreSQL, MySQL, SQL Server, Snowflake)

**Mock strategy:** SQLite or PostgreSQL container seeded with realistic data using `faker` or manual seed scripts. Schema matches the client's real schema as closely as possible.

**What to include:**
- Enough rows to support meaningful queries (100–1000 rows depending on use case)
- Realistic distributions (not all records with the same values)
- FK relationships maintained

**File format:** `seed.sql` + `schema.sql` or `docker-compose.yml` with init scripts

**Real integration path:** Dremio connection to the client's real database. Kaji skill queries via Dremio SQL.

---

### Communication / Messaging (Slack, Microsoft Teams, Email)

**Mock strategy:** Pre-loaded Mattermost channel or hardcoded message history JSON. The demo runs in Mattermost (which is live), so no mocking needed for the chat interface itself.

**Real integration path:** Slack MCP (beta, 3–5 day setup) or Teams connector (beta). For email: SMTP or Gmail API integration.

---

### Workflow Systems (n8n already used as orchestrator in demo)

**Mock strategy:** n8n workflow that fires successfully but writes to a mock sink (log file, test Slack channel, mock database) instead of the real destination.

**Real integration path:** Change the n8n workflow destination node to point to the live system. No app code changes required.

---

## Quick Reference Table

| System Type | Mock Complexity | Build Time | Real Integration Path |
|-------------|----------------|------------|----------------------|
| CRM | Low | 0.5 day | MCP skill + API credentials |
| Ticketing / ITSM | Low | 0.5 day | MCP skill + API credentials |
| ERP (SAP/Oracle) | Medium | 1 day | Custom connector, 3–5 days |
| Monitoring | Medium | 1 day | MCP skill + API key |
| Document / Knowledge | Low | 0.5 day | RAG ingestion + source credentials |
| Database (SQL) | Low | 0.5 day | Dremio connection + DB credentials |
| Messaging (Slack/Teams) | Very Low | 0 days (use Mattermost) | Slack/Teams MCP beta |
| Workflow sink | Low | 0.5 day | Update n8n workflow node |
| Custom internal API | Medium | 1–2 days | Client provides API docs + credentials |

---

## Demo Data Best Practices

1. **Make it feel real** — use realistic names, amounts, dates, and language from the client's industry. Generic placeholder data kills demos.
2. **Bake in the "wow moment"** — seed one obvious problem, anomaly, or opportunity the demo will surface. The user should see something they'd actually care about.
3. **Keep it small enough to be readable** — 20–50 records is enough for any demo. More data = slower queries + harder to explain.
4. **Use the client's terminology** — field names, status values, and category labels should match what the client uses in real life.
5. **Version your mock data** — use `seed_v1.sql` etc. so you can reset the demo to a clean state.
