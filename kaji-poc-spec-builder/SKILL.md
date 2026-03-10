---
name: kaji-poc-spec-builder
description: "Build POC specifications and demo architectures from a client use case. Generates a full app spec, maps mock-to-real systems, shows which Shakudo and Kaji components are used, and produces a demo build brief an engineer can start from immediately. Use when a client describes a problem or opportunity and you need to convert it into a specced, demoable application."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.4"
  category: solutions-engineering
  tags:
    - poc
    - spec
    - solutions-engineering
    - shakudo
    - kaji
---

# Kaji POC Spec Builder

Convert a client use case into a complete POC specification that an engineer or agent can build from immediately. Output: company background, full app spec (sections 0–12), mock/real system map, Shakudo platform map, and POC build brief — all in one ClickUp task, no subtasks.

**Core principle**: The POC is always pre-built before any client meeting. It runs in Shakudo's dev environment (GCP). On go-live, the same app deploys to the client's environment. The spec must be complete enough to build from — not just describe.

**Environment rule**: POC built on `dev.hyperplane.dev` (Shakudo GCP). Go-live = deploy same app into client's infrastructure (Azure, AWS, on-prem). Always note both in the spec.

---

## When to Activate

- "Build me a spec for [use case]"
- "Turn this use case into a POC"
- "Spec this out for [client]"
- "What would we build for [client/problem]?"
- "Build a spec from this ClickUp task / Fireflies call / Notion page / Mattermost thread"
- "Rewrite this into a spec"
- Any client use case that needs to be converted into something buildable

---

## Two Modes

**Full Spec** (default) — Complete spec (sections 0–12) + mock/real map + Shakudo platform map + POC build brief. Use for any new client opportunity or engagement.

**Quick Spec** — Compact version (sections 0, 1, 4, 10 only) for fast internal alignment. Use when speed matters more than completeness.

Ask if not specified. Default to **Full Spec**.

---

## Execution Flow

### Phase 0: Read All Sources First

**Before asking any questions, read everything available.**

Check in this order:
1. **ClickUp task** (if URL/ID provided) — `get_task`: description, comments, subtasks
2. **Mattermost channel/thread** (if referenced) — `get_thread` or `get_channel_messages`: context, decisions, explicit corrections
3. **Notion page** (if referenced) — `retrieve-a-page` + `get-block-children`: architecture docs, client overviews, security/infra details
4. **Fireflies meeting** (if referenced) — `fireflies_get_summary`: pain points, systems, agreed scope
5. **Pasted text** — use as-is

Extract: client name, problem, tech stack, user roles, scope decisions, and any explicit corrections (e.g., "not GCP, it's Azure", "pre-built not live build").

After reading, summarize what you found in 2–3 sentences. Then proceed — skip questions already answered by the sources.

---

### Phase 1: Intake

If critical info is still missing after reading all sources, ask **at most 3 questions**:
1. Who is the primary user?
2. What are they doing manually today that this replaces?
3. What would make this POC successful for the client?

Make reasonable assumptions for anything still unknown. State them clearly.

**Infrastructure question** — always ask this if client infra is unknown:

> "Do you have the client's infrastructure details (cloud, auth, LLM setup)? Or should I default to Shakudo dev (`dev.hyperplane.dev`) for the POC and mark go-live environment as TBD?"

- If infra details are provided → use them in Section 0 (tech environment), Section 6 (systems), and the environment table
- If not → set POC env as `dev.hyperplane.dev` (Shakudo GCP), mark go-live as `[TBD — client to provide]` in the environment table, and note in Section 0 that infra will be confirmed before go-live

**Research the client** (if name is known):
1. Fetch client website (homepage + About page)
2. Extract for **Section 0**: industry, what they do, size, tech environment
3. Extract for **Section 12**: brand colors (CSS vars, Elementor globals), font, logo URL
4. Do both in one pass — single fetch

**Classify the app type:**
- **Operational Assistant** — agent helps users investigate, decide, or act on real-time data
- **Data & Analytics** — NLP-to-SQL, report generation, trend analysis
- **Workflow Automation** — event-triggered pipelines, approval flows, multi-step orchestration
- **Knowledge Assistant** — RAG over documents, policies, internal knowledge bases
- **Multi-Agent System** — coordinated agents for complex multi-step tasks

---

### Phase 2: Build the Spec

Fill out all 13 sections using [App Spec Template](./assets/app-spec-template.md).

Key rules:
- **Section 0 first** — establishes who the client is before any technical content
- **No placeholders** — if info is missing, state your assumption and mark `[ASSUMED]`
- **Section 8 (Example Interaction) is mandatory** — write a real message and real response, no templates
- **Section 10 (Scope) must be honest** — what's in this POC, what's not, what's Year 2
- **App Functions** — name capabilities as what the app does, not as presentation moments

---

### Phase 3: Mock/Real System Map

For every system mentioned or implied, produce this table:

| System | Status | Mock Strategy | Real Integration Path | What Changes on Go-Live |
|--------|--------|---------------|----------------------|--------------------------|
| [System] | ✅ / 🟡 / 🔴 | [How we fake it] | [Real connection] | [Env var / config field] |

Status:
- ✅ **Available** — live integration or public data, no mock needed
- 🟡 **Mocked** — realistic fake, swappable with one config change
- 🔴 **Needs Build** — new connector required, estimate separately

Always include the environment table:

| Phase | Environment |
|-------|-------------|
| Build & POC | `dev.hyperplane.dev` (Shakudo GCP) |
| Go-Live | [Client's environment — cloud/auth/infra specifics] |

---

### Phase 4: Shakudo Platform Map

| App Function | Shakudo Component | Role |
|---|---|---|
| [Function] | [Kaji / n8n / microservice / Chroma / Arize Phoenix / etc.] | [What it does] |

---

### Phase 4b: Design, UI & UX

Run whenever the POC has a user-facing frontend.

1. Fetch client website → extract CSS brand colors (font, logo, primary/bg/accent)
2. If no CSS found, infer from visual inspection and note `[INFERRED]`
3. Fill Section 12: color palette, usage guide (what each color is used for), page layout, component design

---

### Phase 5: POC Build Brief

Must be complete enough for an engineer or agent to start building immediately. Include:

**Environment vars** — two blocks:
```bash
# POC (Shakudo dev.hyperplane.dev)
LLM_ENDPOINT=...

# Go-Live (Client env)
LLM_ENDPOINT=...
```

**Build checklist** (checkboxes):
- [ ] [Atomic build task]

**Acceptance criteria** (checkboxes):
- [ ] [Specific, verifiable condition]

**Go-live requirements from client** (checkboxes):
- [ ] [What the client must provide]

---

## Output Format

**SHOW-FIRST RULE**: Display the full spec before offering any ClickUp update. Never ask "should I update the ticket?" before the spec is on screen.

Deliver in this order:
1. Full spec (sections 0–12)
2. Mock/Real System Map + environment table
3. Shakudo Platform Map
4. POC Build Brief (without walk-through)

Then ask:
> "Do you want a POC walk-through added? (step-by-step presenter flow)"

- If **yes** → generate the walk-through table and append it to the spec
- If **no** → skip it

Then offer:
- Update the ClickUp task with this spec (one task, no subtasks — description IS the full spec)
- Create a new ClickUp task if none exists
- Generate a client-facing one-pager

---

## Walk-Through (On Demand)

The walk-through can be generated at any time — not just at spec creation. Trigger phrases:

- "show me the walk-through"
- "print the walk-through"
- "walk-through for this spec"
- "how do we present this POC?"

When triggered, generate:

| Step | User Does | App Does | Talking Point |
|------|-----------|----------|---------------|
| 1 | [What user does] | [What app does] | [Value statement for the room] |
| ... | | | |

5–10 steps. Each talking point should be one sentence, written for the person presenting — not technical. The last step should always close toward the renewal or next action: *"This is what Year 2 looks like. Three use cases in 90 days."*

The walk-through is never part of the ClickUp task description — it's a separate output for the presenter.

---

## Anti-Patterns

- **Never offer to update ClickUp before showing the spec**
- **Never assume the tech stack** — check Notion and Mattermost first; client infra is often different from what's in the original task
- **Never use "demo" language** — it's a POC, an application, a build
- **Never describe a live build** — the POC is always pre-built; the client sees a working app
- **Never create subtasks** — one task, all content in the description
- **Never leave Section 8 as a placeholder** — write real message + real response
- **Never ask questions already answered by the sources**
- **Never auto-generate the walk-through** — always ask first
- **Never include the walk-through in the ClickUp task description** — it's a separate presenter output

---

## Execution Checklist

- [ ] All sources read (ClickUp / Mattermost / Notion / Fireflies / pasted text)
- [ ] Source summary stated before proceeding
- [ ] Client website fetched (Section 0 + Section 12 in one pass)
- [ ] App type classified
- [ ] Infrastructure question asked — client infra details provided or go-live marked TBD
- [ ] At most 3 questions asked if critical info still missing
- [ ] Section 0 filled — company, industry, size, tech environment, audience, relationship context
- [ ] Sections 1–11 complete, no placeholders
- [ ] Section 8 — real message + real response written
- [ ] Section 12 — brand colors, font, logo, component design
- [ ] Mock/Real System Map — all systems, "What Changes on Go-Live" column filled
- [ ] Environment table — dev (Shakudo GCP) + go-live (client env)
- [ ] Shakudo Platform Map complete
- [ ] POC Build Brief — env vars, build checklist, acceptance criteria, go-live requirements
- [ ] Full spec shown before any ClickUp update offered
- [ ] Walk-through offered as optional (not auto-generated)
- [ ] Walk-through NOT included in ClickUp task description (separate output only)
- [ ] Output written into ONE ClickUp task, no subtasks

---

## References

- [App Spec Template](./assets/app-spec-template.md) — 13-section spec (sections 0–12)
- [POC Architecture Template](./assets/demo-architecture-template.md) — Mock/real map + build brief format
- [Shakudo Platform Capabilities](./references/shakudo-platform-capabilities.md) — Kaji, n8n, microservice, Dremio, Arize Phoenix catalog
- [Mock Patterns](./references/mock-patterns.md) — Standard mock strategies by system type
- [POC Library](./references/demo-library.md) — Existing POCs to fork (Gallo, Reagan, HR Resume, Campbell, Dynex)
