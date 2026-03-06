---
name: kaji-poc-spec-builder
description: "Build POC specifications and demo architectures from a client use case. Generates a full app spec, maps mock-to-real systems, shows which Shakudo and Kaji components are used, and produces a demo build brief an engineer can start from immediately. Use when a client describes a problem or opportunity and you need to convert it into a specced, demoable application."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.3"
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
- "Build a spec from this ClickUp task / Fireflies call / Notion page / document"
- "Rewrite this into a spec"
- "Use [source] to build the spec"
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

### Phase 0: Read Source (if a source is provided)

**This phase runs BEFORE intake when the user points to an existing document, task, or transcript.**

If the user provides any of the following, read it first before asking any questions:

| Source | How to Read It | What to Extract |
|--------|---------------|-----------------|
| ClickUp task name or ID | `get_task` tool | Task name, description, comments, subtasks, assignees |
| Fireflies meeting / transcript | `fireflies_get_summary` or `fireflies_search` | Meeting summary, action items, systems mentioned, problems discussed |
| Notion page URL or name | `notionApi_API-retrieve-a-page` + `API-get-block-children` | Page content, tables, bullet lists |
| Pasted text | Read directly | Use as-is — treat it as raw use case input |
| Any URL | Web fetch | Extract relevant content from the page |

**Extraction rules:**
- Pull out: client name, problem statement, systems mentioned, user roles, desired outcomes, any scope notes
- If the source is a meeting transcript, look for: pain points stated by the client, systems they use, what they asked for, what was agreed
- If the source is a ClickUp task, look for: task description, any attached requirements, comments with additional context
- Treat everything extracted as input to Phase 1 — skip questions you already have answers to

**After reading the source**, state what you found in 2–3 sentences:
> "I read [source]. Here's what I'm working with: [brief summary of extracted context]. Building the spec now."

Then proceed directly to Phase 2 (skip or shorten Phase 1 based on what was extracted).

---

### Phase 1: Intake

Ask the user for:
- **Client name** (optional — use "Client" if not provided)
- **Use case description** — what is the user/business trying to do?
- **Known systems** — what tools, databases, or platforms are involved? (optional)
- **Demo urgency** — when does this need to be showable?

**If client name is provided, research the company before proceeding:**
1. Fetch the client's website (homepage or About page)
2. Extract: industry, what they do, approximate size, technology environment signals
3. Pre-fill Section 0 (Company Introduction) from this research
4. Note any infrastructure posture clues relevant to the demo (air-gapped, cloud-native, regulated, etc.)
5. This research also informs Section 12 (brand colors) — do both in one fetch

If the client can't be found or the website gives no useful context, note `[ASSUMED]` and proceed.

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

Start with **Section 0 — Company Introduction**. This section comes first and sets the context for everyone who reads the spec. Fill it out before writing any capability or technical content.

Key rules for Section 0:
- Write it so a Shakudo team member picking this up for the first time immediately understands the client
- Include the technology environment — this shapes which systems are ✅ available vs 🟡 mocked
- The audience field tells the reader how much technical depth to expect in the rest of the spec

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

### Phase 4b: Design, UI & UX

**Run this phase whenever the spec includes a user-facing interface** (dashboard, web UI, branded frontend). Skip if the only interface is plain Kaji chat with no custom UI.

**If the client name or website is known:**
1. Fetch the client's website (homepage or brand page)
2. Extract brand CSS — look for Elementor global styles, CSS variables (`--e-global-color-*`), or inline style declarations
3. Extract: primary color, background/secondary color, accent/alert color, text color, font family
4. If no CSS is parseable, take a screenshot and extract colors visually

**Fill out Section 12 (Design, UI & UX) of the spec:**

| Field | How to get it |
|-------|--------------|
| Primary color | CSS `--e-global-color-primary` or dominant brand color |
| Background / secondary | CSS `--e-global-color-secondary` or site background |
| Accent / alert | CSS `--e-global-color-accent` or CTA/button color |
| Font family | CSS `font-family` or Google Fonts link in `<head>` |
| Visual tone | Infer from site — dark/light, minimal/editorial, enterprise/consumer |

**Color usage rules (apply these defaults, adjust to context):**
- Primary color → interactive elements, buttons, active states, highlights, savings counter
- Background color → page and card backgrounds, nav bar
- Accent color → critical alerts, error states, P1/P2 incidents, reject buttons
- White → text on dark backgrounds
- Font → all text, all weights

**If no website is provided or colors can't be extracted:**
- Note: `[ASSUMED — client brand colors not confirmed]`
- Default to a clean dark enterprise palette: `#1A1A2E` background, `#4A90D9` primary, `#E53935` accent

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

**SHOW-FIRST RULE: Always deliver the full spec output before offering to update or write anything. Never ask "should I update the ClickUp task?" before the spec is on screen. The user reads the spec first, then decides what to do with it.**

Deliver the output in this order:

1. **App Spec** (full or Jira-friendly based on mode)
2. **Mock/Real System Map** (table)
3. **Shakudo Platform Map** (table)
4. **Demo Build Brief**
5. **Next Steps** — only after the full output is shown, offer to:
   - Update the ClickUp task with this spec (if a task was the source)
   - Create a new ClickUp task for the demo build
   - Generate a client-facing one-pager
   - Start building (if on Shakudo platform with access)
   - Add this demo to the estimator as a use case

> Never bundle the offer to update a ticket into the spec output itself. Show everything first. Offer actions after.

---

## Practical Guidance

- **Lead with the demo, not the spec** — the spec supports the demo; always describe what the client will *see and do* before listing capabilities
- **Mock everything by default** — assume no system access until proven otherwise
- **Name the real integration clearly** — the mock/real toggle is a sales tool: "We built this with mocked Salesforce. When you give us credentials, we flip one config and it's live"
- **Use existing demos as starting points** — check [Demo Library](./references/demo-library.md) before designing from scratch
- **Kaji is always in the stack** — every demo uses Kaji as the AI layer; the question is how
- **Always include an Example Interaction** — a concrete message + response is worth more than 10 capability bullets

## Anti-Patterns

- **Don't offer to update a ticket before showing the spec** — always show the full output first; actions come after
- Don't write vague capability descriptions — "AI-powered insights" says nothing; "returns the top 3 transfer candidates ranked by days of stock" says everything
- Don't leave systems as TBD in the mock/real map — pick a mock strategy and move forward
- Don't spec features out of scope for a demo — keep it tight and demoable in 5 minutes
- Don't design a brand-new architecture if an existing demo is 80% of the way there
- Don't skip the Example Interaction — it anchors everything else
- Don't ask questions you already have answers to from the source — if you read a ClickUp task with a full description, go straight to building the spec

## Error Handling

- Use case too vague → ask 3 clarifying questions, then proceed with stated assumptions
- No systems identified → default to mocked CRM + mocked ticketing system as baseline, note assumptions
- Demo urgency < 2 days → flag which components can be built in time, push complex parts to v2
- Client already has Shakudo access → skip mocking for any system with an active MCP integration

## Execution Checklist

- [ ] Source read first (if ClickUp task / Fireflies / Notion / pasted text provided)
- [ ] Extracted context summarized before proceeding
- [ ] Client name and use case captured
- [ ] Section 0 (Company Introduction) filled — company background, size, tech environment, audience for brief
- [ ] App type classified
- [ ] Up to 3 clarifying questions asked (if needed — skip questions already answered by source)
- [ ] Full app spec generated using app-spec-template.md
- [ ] Intelligence layer section filled out (LLM usage, agent behavior, tools)
- [ ] Example Interaction written (concrete, not placeholder)
- [ ] Scope section is clear (in/out)
- [ ] Mock/Real System Map generated (all systems covered)
- [ ] Shakudo Platform Map generated
- [ ] Design/UI/UX section filled (if app has a frontend — fetch brand colors from client website)
- [ ] Demo Build Brief produced with estimated build time
- [ ] **Full spec shown to user before any ticket update is offered**
- [ ] Next steps offered after output is displayed (ClickUp update, one-pager, build)

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
