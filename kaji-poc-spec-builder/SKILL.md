---
name: kaji-poc-spec-builder
description: "Turn a client use case into a buildable POC spec and architecture decision package. Produces the full app spec, architecture options considered, recommended build strategy, component selection rationale, mock/real system map, Shakudo build stack, service deployment specs, and POC build brief."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "1.5"
  category: solutions-engineering
  tags:
    - poc
    - spec
    - architecture
    - shakudo
    - kaji
    - agentic
---

# Kaji POC Spec Builder

Convert a client use case into a complete, buildable POC specification that an engineer or agent can execute immediately.

## Output package

Every full spec should produce:
1. company background + app spec (sections 0–12)
2. architecture options considered
3. recommended build strategy
4. component selection rationale
5. mock / real system map + environment table
6. Shakudo build stack (platform map)
7. service deployment specs
8. POC build brief

## Core principles

- **Research first, ask second.** Read all available context before asking questions.
- **Simplest credible POC wins.** Always choose the smallest architecture that proves the value.
- **Architecture choice must be explicit.** Consider alternatives and explain why the chosen stack is best.
- **Reuse existing demos whenever possible.** Identify the closest prior build and note what to reuse.
- **POC first, go-live second.** The spec should separate what we build now from what comes later.
- **Shakudo component choices should be intentional.** Do not list tools casually; justify them.

**Environment rule**: The POC is built and demonstrated on `dev.hyperplane.dev` (Shakudo GCP). Go-live means deploying the same application into the client's environment. Always name both.

---

## When to Activate

- "build me a spec for [use case]"
- "turn this into a POC"
- "what would we build for [client / problem]?"
- "spec this out from this ClickUp task / Notion page / Fireflies call / thread"
- "rewrite this into a buildable POC spec"
- any client use case that needs a concrete app design and build plan

---

## Two Modes

### Full Spec (default)

Use for new client opportunities, internal POCs, or anything that may be built.

Includes:
- full app spec (sections 0–12)
- architecture options considered
- recommended build strategy
- component selection rationale
- mock / real system map
- Shakudo build stack
- service deployment specs
- POC build brief

### Quick Spec

Use for fast internal alignment.

Includes a compressed version of:
- sections 0, 1, 4, 10
- recommended build strategy
- minimal mock / real system map
- minimal build brief

Ask if the user wants Quick Spec. Otherwise default to **Full Spec**.

---

## Execution Flow

### Phase 0: Read All Sources First

Before asking anything, read everything available.

Check in this order:
1. **ClickUp task** — description, comments, subtasks
2. **Mattermost thread / channel** — context, decisions, corrections
3. **Notion page** — architecture notes, security / infra details, process docs
4. **Fireflies meeting** — pain points, system mentions, agreed scope
5. **Pasted text or screenshots** — direct constraints and examples

Extract:
- client name and industry
- primary user and workflow
- systems involved
- constraints (security, infra, timeline, compliance)
- explicit corrections (for example: Azure not GCP, pre-built not live build, no Slack, use Mattermost)

After reading, summarize what you found in 2–3 sentences. Then continue. Do **not** ask questions that were already answered.

---

### Phase 1: Intake and Assumptions

If critical information is still missing, ask **at most 3 questions**:
1. Who is the primary user?
2. What are they doing manually today that this replaces?
3. What would make this POC successful for the client?

### Infrastructure question (always ask if unknown)

> "Do you have the client's infrastructure details (cloud, auth, LLM setup, security constraints)? Or should I default the POC to Shakudo dev and mark go-live environment as TBD?"

Rules:
- If infra details are provided, use them in Section 0, Section 6, the environment table, and go-live requirements.
- If not, set:
  - **Build & POC** = `dev.hyperplane.dev` (Shakudo GCP)
  - **Go-Live** = `[TBD — client to provide]`

### Client research

If the client is known, fetch the website once and extract:
- **Section 0**: industry, business, size, footprint, technology environment
- **Section 12**: brand colors, font, logo source

### Classify the app shape

Choose one primary app type:
- **Operational Assistant** — investigate, decide, or act on business situations
- **Data & Analytics** — NL-to-SQL, analysis, dashboards, executive insight
- **Workflow Automation** — multi-step orchestration, approvals, event-driven flows
- **Knowledge Assistant** — documents, policies, SOPs, research over content
- **Multi-Agent System** — multiple distinct agents with separate roles
- **Hybrid** — two of the above are truly central

---

### Phase 2: Architecture Option Analysis

Use:
- [POC Architecture Template](./assets/demo-architecture-template.md)
- [Shakudo Platform Capabilities](./references/shakudo-platform-capabilities.md)
- [Component Selection Rubric](./references/component-selection-rubric.md)
- [POC Library](./references/demo-library.md)

For every Full Spec, consider at least **2 options**. For non-trivial use cases, consider **3 options**.

Common options to evaluate:
- chat-native Kaji agent
- UI-first app + API backend
- workflow-first automation with n8n
- analytics copilot over structured data
- knowledge assistant with RAG
- multi-agent orchestration
- hybrid: chat + UI + API + workflow

### Architecture analysis rules

For each option, evaluate:
- primary user surface
- main Shakudo components
- closest reuse pattern or demo
- what the user gets in the room
- what is easy to mock
- what will be hardest to build
- why it is or is not the best POC choice

### Required comparison artifacts

For every Full Spec, produce both:
1. an **Architecture Options Considered** table
2. an **Architecture Scorecard** using the rubric dimensions:
   - credibility
   - simplicity
   - reuse
   - mockability
   - extensibility
   - fit

### Default best-practice filters

- Prefer **one primary interface** unless two are necessary for the story.
- Prefer **chat + tools** for conversational / action-heavy workflows.
- Prefer **UI + API** when visual state, forms, dashboards, or review workflows matter.
- Prefer **Dremio / SQL** for structured analytics before reaching for RAG.
- Prefer **RAG** when the value comes from documents, policies, or unstructured knowledge.
- Prefer **n8n** for orchestration and approval flows, not for all business logic.
- Prefer **single-agent** unless multiple agents clearly map to different responsibilities.
- Prefer **mock integrations** over real credentials when the POC story is unaffected.

### Required architecture output

Produce a table like this:

| Option | Interaction Model | Core Components | Closest Reuse Pattern | Why It Could Work | Why It Might Be Wrong | Verdict |
|---|---|---|---|---|---|---|
| Option A | | | | | | |
| Option B | | | | | | |
| Option C | | | | | | |

Then add an Architecture Scorecard table with High / Medium / Low ratings.

### Phase 3: Recommended Build Strategy

After comparing options, produce a concise recommendation.

Required fields:

```md
## Recommended Build Strategy
- App type:
- Primary interaction model:
- Recommended architecture:
- Best component stack:
- Closest existing demo to reuse:
- Components to build now:
- Components to mock now:
- Components to defer to Year 2:
- Why this is the best POC choice:
```

Also include a short **POC Simplification Decisions** table that explicitly says what is being kept, removed, mocked, or deferred.

This section is mandatory. `kaji-poc-build-deploy` depends on it.

### Phase 4: Build the App Spec (Sections 0–12)

Fill out all sections in [App Spec Template](./assets/app-spec-template.md).

Key rules:
- **Section 0 first** — it establishes who the client is
- **No placeholders** — if unknown, state the assumption and mark `[ASSUMED]`
- **Section 8 is mandatory** — write a real user message and a real response
- **Section 10 must be honest** — clearly separate POC scope from later phases
- **Capability names should describe user value**, not technology

---

### Phase 5: Mock / Real System Map and Environment Table

For every system mentioned or implied, produce:

| System | Status | Mock Strategy | Real Integration Path | What Changes on Go-Live |
|---|---|---|---|---|
| [System] | ✅ / 🟡 / 🔴 | | | |

Status meanings:
- ✅ **Available** — live integration already exists or public data is enough
- 🟡 **Mocked** — realistic fake for the POC, swappable later
- 🔴 **Needs Build** — custom connector or custom work required

Always include:

| Phase | Environment |
|---|---|
| Build & POC | `dev.hyperplane.dev` (Shakudo GCP) |
| Go-Live | [Client environment or TBD] |

---

### Phase 6: Shakudo Build Stack and Component Selection Rationale

Build the Shakudo stack explicitly.

Required table:

| App Function | Recommended Component | Why It Is Used | Build Now or Later |
|---|---|---|---|
| [Function] | [Kaji / microservice / n8n / Dremio / RAG / etc.] | | |

This replaces the older shallow "platform map". The goal is not just to name components, but to justify them.

Also produce a short **Component Selection Rationale** block that explains:
- why the chosen components are the best fit
- what alternatives were rejected
- where the app is intentionally simplified for the POC

---

### Phase 7: Service Deployment Specs and POC Build Brief

The build brief must be good enough for `kaji-poc-build-deploy` to act on.

Include:
- environment vars for **POC** and **go-live**
- build checklist
- acceptance criteria
- client go-live requirements
- **service deployment specs** for every deployable service

Each service deployment spec should include:
- `jobName`
- `subdomain`
- `jobType`
- `gitServerName` (or note that it must be resolved from repo remote)
- `branchName`
- `workingDir`
- `pipelineYamlPath`
- `port`
- `deployOrder`
- `lifecycleMode`
- `parameters`
- `smokeTests`
- `recreateNotes`

Prefer the Shakudo repo-root pattern unless there is a strong reason to use app-dir working directories:

```yaml
workingDir: /tmp/git/monorepo/
pipelineYamlPath: my-app/api/run.sh
```

Parameter defaults should show whether they come from a real secret, a mounted env var, or a mock fallback.

If the design depends on lite-only lifecycle behavior, say so explicitly in the build brief so the deploy skill knows stop / restart may require delete + recreate.

## Output Format

**SHOW-FIRST RULE**: display the actual output before offering to update ClickUp.

Deliver in this order:
1. full app spec (sections 0–12)
2. architecture options considered
3. recommended build strategy
4. component selection rationale + Shakudo build stack
5. mock / real system map + environment table
6. service deployment specs
7. POC build brief

Then ask:

> "Do you want a POC walk-through added? (step-by-step presenter flow)"

If yes, generate the walk-through. If no, skip it.

Then offer:
- update the ClickUp task with the spec
- create a new ClickUp task if needed
- generate a client-facing one-pager

---

## Walk-Through (On Demand)

Generate only when asked.

Trigger phrases:
- "show me the walk-through"
- "print the walk-through"
- "walk-through for this spec"
- "how do we present this POC?"

Format:

| Step | User Does | App Does | Talking Point |
|---|---|---|---|
| 1 | | | |

5–10 steps. The final step should point to the next phase or Year 2 value.

The walk-through is **not** part of the ClickUp task description.

---

## Anti-Patterns

- **Do not offer to update ClickUp before showing the spec.**
- **Do not skip architecture comparison.** Always evaluate alternatives.
- **Do not choose multi-agent by default.** Use it only when roles are truly distinct.
- **Do not put both a full UI and a rich chat layer into scope unless both are necessary.**
- **Do not assume RAG is needed just because there are documents.**
- **Do not assume SQL is enough when the key value is document reasoning.**
- **Do not list Shakudo components without explaining why they were chosen.**
- **Do not write vague deployment guidance.** Include `workingDir`, `pipelineYamlPath`, `jobType`, and deploy order.
- **Do not auto-generate the walk-through.** Ask first.
- **Do not put the walk-through into the ClickUp task description.**

---

## Execution Checklist

- [ ] All sources read first
- [ ] Source summary stated before questions
- [ ] Client website reviewed if client is known
- [ ] App type classified
- [ ] Infrastructure question asked if needed
- [ ] At most 3 clarifying questions asked
- [ ] Architecture options considered written out
- [ ] Recommended build strategy selected and justified
- [ ] Closest existing demo identified
- [ ] Sections 0–12 completed with no placeholders
- [ ] Section 8 contains a real interaction
- [ ] Mock / real system map completed
- [ ] Environment table included
- [ ] Shakudo build stack completed
- [ ] Component selection rationale completed
- [ ] Service deployment specs completed
- [ ] POC build brief completed
- [ ] Full spec shown before any ClickUp update offer
- [ ] Walk-through offered only after spec is shown

---

## Integration

- **ClickUp** — write the full spec into one task description
- **Mattermost** — share the spec for internal review
- **kaji-poc-build-deploy** — builds and deploys the spec after architecture decisions are made
- **kaji-agentic-engineering-estimator** — uses the spec to scope effort and pricing
- **kaji-client-onboarding** — turns the approved POC into deployment planning

---

## References

- [App Spec Template](./assets/app-spec-template.md)
- [POC Architecture Template](./assets/demo-architecture-template.md)
- [Shakudo Platform Capabilities](./references/shakudo-platform-capabilities.md)
- [Component Selection Rubric](./references/component-selection-rubric.md)
- [Mock Patterns](./references/mock-patterns.md)
- [POC Library](./references/demo-library.md)
