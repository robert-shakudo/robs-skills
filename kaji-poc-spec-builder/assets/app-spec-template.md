# [App Name] — Application Specification

> **Instructions**: Fill out every section. Do not leave placeholders. If information is unknown, state your assumption and mark it `[ASSUMED]`. The Example Interaction (Section 8) is mandatory — write a real message and response.

---

## 0. Company Introduction

> This section establishes who the client is before the spec begins. It should be readable by anyone on the Shakudo team picking this up for the first time — and by a client executive reviewing what we've built.

**Company Name:** [Full legal or brand name]

**Industry / Sector:** [e.g., Quantitative Finance, Healthcare Technology, Logistics & Supply Chain]

**What They Do:**
[2–3 sentences in plain language. What is their business? What do they produce, manage, or operate? Who are their customers?]

**Company Size:**
- Employees: [headcount or range — e.g., "~200 employees"]
- Scale: [relevant scale metric — AUM, revenue range, # of locations, # of systems managed, etc.]
- Geography: [HQ location + any key offices or operational footprint]

**Technology Environment:**
[Describe their infra posture relevant to this demo — cloud-native, on-premise, air-gapped, hybrid, legacy systems, security posture, compliance requirements. 2–4 bullet points.]
- [e.g., Air-gapped infrastructure — all compute on-premise, no public cloud]
- [e.g., Strict security — no external model APIs, LLM inference must be local]
- [e.g., Primary tooling: Airflow, Confluence, JIRA, Perforce, TeamCity]

**Audience for This Brief:**
[Who is this spec written for? Who will read and act on it?]
- Internal: [e.g., Shakudo ProServe team, solutions engineer, demo presenter]
- Client: [e.g., VP of Engineering, Head of Operations, IT leadership]

**Relationship Context:**
[1–2 sentences on where we are with this client — new prospect, active engagement, post-demo follow-up, etc.]

---

## 1. Overview

**Application Name:**
[Name of the app]

**Client / Opportunity:**
[Client name or "Internal Demo"]

**Purpose:**
[Describe what the app does in 2–3 plain sentences. What does it do? Who uses it? What does it help them accomplish?]

**Primary Users:**
- [User role / persona]
- [User role / persona]

**Problem It Solves:**
[What manual work, broken process, or information gap does this address? Be specific about the pain — not "improves efficiency" but "ops team spends 3 hours per day triaging tickets by hand".]

---

## 2. What the App Does

[Describe the app simply. Answer these three questions in 1–2 paragraphs:]

- What does the user come here to do?
- What outcome does the app help them achieve?
- What makes this experience better than what they're doing today?

---

## 3. Core Experience

[Walk through the user experience from beginning to end. Use numbered steps. Be concrete — what does the user actually see and do?]

1. The user opens the app / enters the workflow
2. The user [asks a question / starts a task / reviews a dashboard]
3. The app [gathers context / queries systems / runs analysis]
4. The app presents [results / guidance / actions / a summary]
5. The user [reviews / approves / acts on] the output

---

## 4. Key Capabilities

[List 3–6 capabilities. Name each one clearly. Describe what the user can do — not what the technology does.]

**Capability 1 — [Name]**
[What the user can do and what they get from it]

**Capability 2 — [Name]**
[What the user can do and what they get from it]

**Capability 3 — [Name]**
[What the user can do and what they get from it]

**Capability 4 — [Name]** *(optional)*
[What the user can do and what they get from it]

**Capability 5 — [Name]** *(optional)*
[What the user can do and what they get from it]

---

## 5. Intelligence Layer

**LLM Usage**
The application uses LLM capabilities to [understand user questions / summarize information / generate responses / reason over data / extract insights from content].

Specifically:
- [What the LLM does — e.g., "converts natural language questions into SQL queries"]
- [What the LLM does — e.g., "summarizes retrieved documents into a 3-sentence answer"]

**Agent Behavior**
[Does the application behave like an agent — making decisions, choosing tools, performing multi-step tasks?]

The application [uses / does not use] agent-like behavior to [determine the best next step / gather information from connected systems / coordinate multi-step tasks / guide the user through a workflow].

Specifically:
- [Describe one agentic behavior — e.g., "decides which system to query based on the user's question"]
- [Describe one agentic behavior — e.g., "breaks a complex request into sub-tasks and executes them in order"]

**Skills / Tools / Actions**
The application can use connected skills, tools, or system actions to retrieve information and complete tasks.

| Tool / Skill | What It Does |
|---|---|
| [Tool name] | [What the app can do with it] |
| [Tool name] | [What the app can do with it] |
| [Tool name] | [What the app can do with it] |

---

## 6. Systems Connected

[List all external systems, databases, or platforms the app connects to. For a demo/POC, note which are mocked.]

| System | Type | Demo Status | Notes |
|--------|------|-------------|-------|
| [System name] | [CRM / ERP / DB / API / etc.] | ✅ Live / 🟡 Mocked / 🔴 TBD | [Any notes] |
| [System name] | [Type] | ✅ / 🟡 / 🔴 | |
| [System name] | [Type] | ✅ / 🟡 / 🔴 | |

---

## 7. Example User Journey

[Write a short, natural scenario — 3–5 sentences. Use a real persona and a realistic situation. Make it feel like something that could happen tomorrow.]

[User persona] opens the app and [does something specific]. They [ask a question / trigger a workflow / review a dashboard]. The system [does something specific — queries a system, runs analysis, retrieves data]. The user receives [a specific result] and [takes a specific next action].

---

## 8. Example Interaction

> This section is mandatory. Write a real message and a real response — not placeholders.

**User:**
[Exact message the user sends — a real question or request]

**App:**
[Exact response the app returns — a real answer, formatted naturally. Include relevant data points, a recommendation, or a list. Show what "good" looks like.]

---

## 9. What Makes This Valuable

[Describe the value in concrete business or user terms. Avoid generic language. Quantify where possible.]

This application provides value by helping users:
- [Specific value — e.g., "identify freight exceptions in under 30 seconds instead of reviewing 3 separate systems"]
- [Specific value — e.g., "surface the recommended action alongside the issue, reducing the decision burden on the ops team"]
- [Specific value — e.g., "act on insights from Mattermost without switching to another tool"]

---

## 10. Scope for Demo or MVP

**In Scope**
- [Feature / capability included in this demo]
- [Feature / capability included in this demo]
- [Feature / capability included in this demo]

**Out of Scope (for now)**
- [Feature deferred to v2 — note why]
- [System not connected in demo — note what mock replaces it]
- [Feature requiring client credentials or infra not available yet]

---

## 11. Future Enhancements

[What would make this more powerful after the demo is validated?]

- [Enhancement — e.g., "Connect to live Salesforce via MCP instead of mocked CRM data"]
- [Enhancement — e.g., "Add approval workflow that writes back to ERP"]
- [Enhancement — e.g., "Multi-agent coordination for complex investigation paths"]
- [Enhancement — e.g., "Personalization based on user role and historical activity"]
- [Enhancement — e.g., "Deeper integration with [System] for real-time data"]

---

## 12. Design, UI & UX

> Fill this section whenever the app has a user-facing interface — dashboard, web UI, or chat UI skin. Skip if the only interface is Kaji chat with no custom frontend.

**Visual Tone**
[Describe the overall look and feel — e.g., "dark enterprise dashboard", "clean light SaaS", "terminal-style ops tool", "consumer-friendly warm interface".]

**Brand Identity**

| Element | Value |
|---------|-------|
| Primary color | `#[hex]` — [description] |
| Secondary / background | `#[hex]` — [description] |
| Accent / alert color | `#[hex]` — [description] |
| Text color | `#[hex]` — [description] |
| Font family | [Font name] — [weights used] |
| Logo | [Source URL or "provided by client"] |

**Color Usage Guide**
- [Primary color]: [Where it's used — e.g., "interactive elements, buttons, highlights, active states"]
- [Background]: [Where it's used — e.g., "page and card backgrounds"]
- [Accent color]: [Where it's used — e.g., "critical alerts, P1 incidents, error states"]
- [Text]: [Where it's used — e.g., "all body text, headings on light backgrounds"]

**UI Layout**

Describe the page/screen structure:
- [Page or screen 1] — [what the user sees and does here]
- [Page or screen 2] — [what the user sees and does here]
- [Page or screen 3] — [what the user sees and does here]

**Component Design**

| Component | Design Notes |
|-----------|-------------|
| [e.g., Status indicator] | [e.g., Teal = healthy, Red = critical, Grey = unknown] |
| [e.g., Incident card] | [e.g., Dark background, colored left border by severity] |
| [e.g., Primary button] | [e.g., Teal fill, white text, Poppins 500] |
| [e.g., Alert / notification] | [e.g., Red background, bold white text, dollar impact prominent] |
| [e.g., Data table] | [e.g., Dark rows, alternating shades, teal column headers] |

**Platform / Responsive Targets**
- [e.g., Desktop first — 1440px primary width]
- [e.g., Mobile not required for demo]
- [e.g., Mattermost chat — no custom styling needed]

**Accessibility Notes** *(optional)*
- [Any contrast, font size, or accessibility requirements]

---

## Jira-Friendly Summary

> Use this for ClickUp tasks, sprint planning, or quick internal alignment.

**Client**
[Company name — industry — size — one line on what they do]

**Overview**
[1–2 sentence description of what the app is and why it matters]

**User Problem**
[1 sentence on the pain being solved]

**Core Experience**
[1–2 sentences on how the user interacts with it]

**Key Capabilities**
- [Capability]
- [Capability]
- [Capability]

**Intelligence Layer**
- LLM usage: [yes — for what]
- Agent behavior: [yes/no — for what]
- Skills/tools: [list the key integrations]

**Systems Connected**
- [System] — [live/mocked]
- [System] — [live/mocked]

**Example User Journey**
[1 short paragraph]

**Value Demonstrated**
[What this shows to the client]

**Scope (Demo)**
[What's in, what's out]

**Design**
- Visual tone: [e.g., dark enterprise / light SaaS]
- Primary color: `#[hex]`
- Background: `#[hex]`
- Font: [Font name]
- UI pages: [list]
