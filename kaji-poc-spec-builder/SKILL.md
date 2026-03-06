---
name: kaji-poc-spec-builder
description: "Build POC specifications and demo architectures from a client use case. Generates a full app spec, maps mock-to-real systems, shows which Shakudo and Kaji components are used, and produces a demo build brief an engineer can start from immediately. Use when a client describes a problem or opportunity and you need to convert it into a specced, demoable application."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.0"
  category: solutions-engineering
  tags:
    - poc
    - spec
    - demo
    - solutions-engineering
    - shakudo
    - kaji
---

# Kaji POC Spec Builder

Convert a client use case into a complete POC specification, demo architecture, and build brief — in one pass. The output shows what we're building, what's mocked vs real, which Shakudo platform components are used, and how an engineer starts building today.

**Core premise**: We can demo a working application immediately using Kaji, the Shakudo platform, and mocked system connections. When the client is ready, the mocks are replaced with real integrations — no rebuild required.

## When to Activate

- "Build me a spec for [use case]"
- "I have a client that needs [X], what would we build?"
- "Turn this use case into a POC"
- "What would this look like as a demo?"
- "Create a POC brief for [client]"
- "Spec this out for me"
- "What Shakudo components would we use for [problem]?"
- Any client use case that needs to be converted into something demoable

## Two Modes

**1. Full Spec + POC Architecture** (default)
Full app specification, mock/real system map, Shakudo platform map, and demo build brief. Use for new client opportunities, pre-sales demos, or when scoping a new engagement.

**2. Quick Spec (Jira-Friendly)**
Compact spec for alignment, ClickUp tasks, or internal handoffs. Use when you need fast written alignment without the full architecture output.

Ask: "Do you want the full POC spec and architecture, or a quick Jira-friendly spec?"

If not specified, default to **Full Spec + POC Architecture**.

---

## Execution Flow

### Phase 1: Intake

Ask the user for:
- **Client name** (optional — use "Client" if not provided)
- **Use case description** — what is the user/business trying to do?
- **Known systems** — what tools, databases, or platforms are involved? (optional)
- **Demo urgency** — when does this need to be showable?

If the use case is too vague to spec, ask **up to 3 clarifying questions**:
1. Who is the primary user of this application?
2. What are they currently doing manually or struggling with?
3. What outcome would make this demo successful to the client?

Do NOT ask more than 3 questions before proceeding with reasonable assumptions. State your assumptions clearly.

**Classify the app type** (choose the closest):
- **Operational Assistant** — agent that helps users investigate, decide, or act on real-time data
- **Data & Analytics** — NLP-to-SQL, dashboards, trend analysis, report generation
- **Workflow Automation** — event-triggered pipelines, approval flows, multi-step orchestration
- **Knowledge Assistant** — RAG over documents, policies, internal knowledge bases
- **Multi-Agent System** — coordinated agents handling complex multi-step tasks

---

### Phase 2: Generate the App Spec

Fill out the full app specification using [App Spec Template](./assets/app-spec-template.md).

Key rules for filling the spec:
- **Write in plain language** — the spec should be readable by a client, not just engineers
- **Intelligence Layer is mandatory** — always describe LLM usage, agent behavior, and tools/actions
- **Example Interaction must be concrete** — write a real message + response, not a placeholder
- **Scope section must be honest** — clearly separate what the demo includes from what it doesn't
- **Never leave a section as a placeholder** — if you don't have info, make a reasonable assumption and note it

---

### Phase 3: Mock/Real System Map

For each system mentioned or implied in the spec:

1. Identify the system (CRM, ERP, ticketing, monitoring, database, etc.)
2. Determine its **demo status**:
   - ✅ **Available Now** — Shakudo has a live integration or MCP for this
   - 🟡 **Mockable** — No live access, but realistic mock can be built in 1-2 days
   - 🔴 **Needs Custom Build** — Requires new integration or client credentials (5-10 days)
3. Define the **mock strategy** — how will we fake this for the demo?
4. Define the **real integration path** — what does "going real" look like?

Format as a table:

| System | Demo Status | Mock Strategy | Real Integration Path |
|--------|-------------|---------------|----------------------|
| [System] | ✅ / 🟡 / 🔴 | [How we mock it] | [How we connect for real] |

See [Mock Patterns](./references/mock-patterns.md) for standard mock strategies by system type.

---

### Phase 4: Shakudo Platform Map

Map the spec's capabilities to the Shakudo platform components that deliver them. See [Shakudo Platform Capabilities](./references/shakudo-platform-capabilities.md) for the full catalog.

Format as a table:

| Capability (from spec) | Shakudo Component | Notes |
|------------------------|-------------------|-------|
| [Capability] | [Component] | [Any relevant notes] |

Common mappings:
- Natural language interface → **Kaji (chat agent)**
- Tool use / system access → **Kaji MCP skills**
- Workflow automation → **n8n on Shakudo**
- API backend → **Shakudo microservice**
- UI / frontend → **Shakudo microservice (React/Next.js)**
- Data querying → **Dremio or direct DB via Kaji skill**
- Document search → **Kaji RAG skill**

---

### Phase 5: Demo Build Brief

Produce a brief that an engineer can start from immediately:

**Build Summary**
- App type and primary user
- Core demo flow (what the user does in 5 minutes)
- Closest existing demo to reference (see [Demo Library](./references/demo-library.md))

**Components Needed**
- List each Shakudo component and its role in the demo

**Mock Data Needed**
- What fake data needs to be created for the demo
- Suggested format (JSON, CSV, SQL seed, hardcoded API responses)

**Estimated Build Time**
- Use the ranges below. Round UP when unsure.

| Demo Complexity | Estimated Build Time |
|-----------------|----------------------|
| Simple (1 system, chat UI, mocked data) | **1–2 days** |
| Medium (2–3 systems, workflow + UI) | **3–5 days** |
| Complex (multi-agent, multiple mocked systems, polished UI) | **1–2 weeks** |

**Go-Live Requirements**
- What the client needs to provide to replace mocks with real systems
- Which Shakudo integrations to enable
- Any credential or access requirements

---

## Output Format

Deliver the output in this order:

1. **App Spec** (full or Jira-friendly based on mode)
2. **Mock/Real System Map** (table)
3. **Shakudo Platform Map** (table)
4. **Demo Build Brief**
5. **Next Steps** — offer to:
   - Create a ClickUp task for the demo build
   - Generate a client-facing one-pager
   - Start building (if on Shakudo platform with access)
   - Add this demo to the estimator as a use case

---

## Practical Guidance

- **Lead with the demo, not the spec** — the spec supports the demo; always describe what the client will *see and do* before listing capabilities
- **Mock everything by default** — assume no system access until proven otherwise
- **Name the real integration clearly** — the mock/real toggle is a sales tool: "We built this with mocked Salesforce. When you give us credentials, we flip one config and it's live"
- **Use existing demos as starting points** — check [Demo Library](./references/demo-library.md) before designing from scratch
- **Kaji is always in the stack** — every demo uses Kaji as the AI layer; the question is how
- **Always include an Example Interaction** — a concrete message + response is worth more than 10 capability bullets

## Anti-Patterns

- Don't write vague capability descriptions — "AI-powered insights" says nothing; "returns the top 3 transfer candidates ranked by days of stock" says everything
- Don't leave systems as TBD in the mock/real map — pick a mock strategy and move forward
- Don't spec features out of scope for a demo — keep it tight and demoable in 5 minutes
- Don't design a brand-new architecture if an existing demo is 80% of the way there
- Don't skip the Example Interaction — it anchors everything else

## Error Handling

- Use case too vague → ask 3 clarifying questions, then proceed with stated assumptions
- No systems identified → default to mocked CRM + mocked ticketing system as baseline, note assumptions
- Demo urgency < 2 days → flag which components can be built in time, push complex parts to v2
- Client already has Shakudo access → skip mocking for any system with an active MCP integration

## Execution Checklist

- [ ] Client name and use case captured
- [ ] App type classified
- [ ] Up to 3 clarifying questions asked (if needed)
- [ ] Full app spec generated using app-spec-template.md
- [ ] Intelligence layer section filled out (LLM usage, agent behavior, tools)
- [ ] Example Interaction written (concrete, not placeholder)
- [ ] Scope section is clear (in/out)
- [ ] Mock/Real System Map generated (all systems covered)
- [ ] Shakudo Platform Map generated
- [ ] Demo Build Brief produced with estimated build time
- [ ] Next steps offered (ClickUp, one-pager, build)

## Integration

- **ClickUp** — Create demo build task with subtasks from the build brief
- **Mattermost** — Share spec in client channel or #proserve for review
- **Kaji Agentic Engineering Estimator** — Add this POC as a use case for pricing
- **Kaji Client Onboarding** — After demo approval, use onboarding skill to plan deployment

## References

- [App Spec Template](./assets/app-spec-template.md) — Full 11-section spec template
- [Demo Architecture Template](./assets/demo-architecture-template.md) — Mock/real map + build brief format
- [Shakudo Platform Capabilities](./references/shakudo-platform-capabilities.md) — What Shakudo and Kaji can do
- [Mock Patterns](./references/mock-patterns.md) — Standard mock strategies by system type
- [Demo Library](./references/demo-library.md) — Existing POCs to reference and reuse
