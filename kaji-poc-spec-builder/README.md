# kaji-poc-spec-builder

**Convert any client use case into a specced, demoable application — in one Kaji conversation.**

This skill helps you go from "a client told me they have a problem with X" to a complete app specification, demo architecture, mock/real system map, and engineer-ready build brief. Every output is designed to be shown to the client as proof of what you started with — and a clear path to production.

---

## The Core Idea

Every demo we build follows the same pattern:

1. **Build it fast with mocks** — Kaji + Shakudo platform + realistic fake data. Demo-ready in 1–5 days.
2. **Show the client something real** — they see a working app, not slides.
3. **Swap mocks for real systems** — when the client provides credentials, one config change connects the live system. No rebuild.

This skill generates everything you need to execute that pattern.

---

## How to Use It

### Step 1 — Trigger the skill

Start a conversation with Kaji and describe the use case. You can be as rough as you want:

```
@kaji build me a POC spec for [client] — they want to [use case description]
```

```
@kaji spec this out: [paste use case notes, email, or meeting summary]
```

```
@kaji what would we build for a client that needs to [problem]?
```

### Step 2 — Answer up to 3 clarifying questions (if asked)

Kaji will ask at most 3 questions if critical info is missing:
- Who is the primary user?
- What are they doing manually today?
- What would a successful demo look like?

If you don't know, say so — Kaji will make reasonable assumptions and state them clearly.

### Step 3 — Choose your output mode

Kaji will ask (or you can specify upfront):

- **Full Spec + POC Architecture** — complete spec, mock/real map, Shakudo platform map, build brief
- **Quick Spec (Jira-friendly)** — compact version for ClickUp tasks or internal alignment

### Step 4 — Receive the output

Kaji delivers in one pass:

1. **App Spec** — 11 sections including a concrete example interaction
2. **Mock/Real System Map** — every system tagged ✅ Available / 🟡 Mocked / 🔴 Needs Build
3. **Shakudo Platform Map** — which Kaji, n8n, microservice, and platform components are used
4. **Demo Build Brief** — components to build, mock data needed, build time estimate, closest existing demo to fork

### Step 5 — Act on it

At the end, Kaji offers:
- Create a ClickUp task for the demo build
- Generate a client-facing one-pager
- Add this as a use case to the Agentic Engineering Estimator for pricing

---

## Example Conversations

### Rough use case from a sales call
```
@kaji spec this out — we were talking to a logistics company and they 
said their ops team spends 3 hours a day triaging freight exceptions 
across 3 different systems. They're on SAP and use a TMS called project44.
```

### From meeting notes or an email
```
@kaji build a POC spec for Huntington — pasting the use case from the 
discovery call: [paste]
```

### From a vague idea
```
@kaji what would we build for a manufacturing client that wants to know 
why their production line keeps going down?
```

### Quick Jira spec
```
@kaji quick spec for an HR resume screening assistant for internal demo
```

---

## What You Get (Full Mode)

```
App Spec
├── Overview (name, purpose, users, problem)
├── What the App Does
├── Core Experience (step-by-step user flow)
├── Key Capabilities (3–6, in plain language)
├── Intelligence Layer (LLM, agent behavior, tools)
├── Systems Connected (with mock status)
├── Example User Journey
├── Example Interaction (real message + response)
├── What Makes This Valuable
├── Scope for Demo (in / out)
└── Future Enhancements

Mock / Real System Map
├── System | Status | Mock Strategy | Real Integration Path

Shakudo Platform Map
├── Capability | Shakudo Component | Role in Demo

Demo Build Brief
├── 5-minute demo script
├── Components to build + build time
├── Mock data requirements
├── Go-live requirements (what client needs to provide)
└── Closest existing demo to fork
```

---

## File Structure

```
kaji-poc-spec-builder/
├── SKILL.md                                  ← Kaji reads this to run the skill
├── README.md                                 ← This file
├── assets/
│   ├── app-spec-template.md                  ← 11-section spec template
│   └── demo-architecture-template.md         ← Mock/real map + build brief format
└── references/
    ├── shakudo-platform-capabilities.md       ← Kaji, n8n, microservice, Dremio catalog
    ├── mock-patterns.md                       ← Mock strategies by system type (CRM, ERP, DB...)
    └── demo-library.md                        ← Existing demos to fork (Gallo, Reagan, HR, Campbell)
```

---

## The Mock/Real Toggle

Every mocked system in the output follows this principle:

> *"We built this with mocked Salesforce. You give us credentials, we flip one environment variable, and it connects to your live system. No rebuild."*

The demo architecture is designed so the swap is a config change, not a code change. This is explicit in every mock/real map the skill generates.

---

## Tips

- **Give more context = better spec** — paste meeting notes, emails, or a problem description. The richer the input, the tighter the output.
- **State the client's existing systems if you know them** — it changes which components are ✅ available vs 🟡 mocked.
- **Use the Demo Library** — before building from scratch, the skill checks existing demos (Gallo, Reagan, HR Resume, Campbell) and tells you what to fork.
- **Quick Spec for internal alignment** — use Jira mode to create a ClickUp task first, then expand to Full Spec when the client is interested.

---

## Installation

This skill is in the `robs-skills` repository. To install or update:

1. Ensure the `robs-skills` git server is synced on your Shakudo instance
2. Install `kaji-poc-spec-builder` from the Skills Marketplace
3. Kaji will have the skill available immediately in any thread

---

## Related Skills

- **kaji-agentic-engineering-estimator** — Once you have a spec, price the engagement
- **kaji-client-onboarding** — After the demo is approved, plan the full deployment
- **rob-weekly-proserve-update** — Track demo builds and client activity in weekly reports
