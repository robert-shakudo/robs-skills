# kaji-poc-spec-builder

**Convert any client use case into a complete, buildable POC spec — in one Kaji conversation.**

Takes a use case (from ClickUp, Fireflies, Notion, Mattermost, or plain text) and produces a full 13-section app specification, mock/real system map, Shakudo platform map, and POC build brief — complete enough for an engineer or agent to start building immediately.

---

## The Core Pattern

1. **Build in Shakudo dev env (GCP)** — POC is pre-built before any client meeting using mocked systems and realistic fake data
2. **Show a working application** — client sees a real app, not slides
3. **Deploy to client's env on go-live** — same app, one config change per system, no rebuild

---

## How to Use

**Give Kaji a source** — as rough as you want:

```
@kaji build me a spec for [client] — [use case description]
```
```
@kaji spec this out: [paste meeting notes / email / problem description]
```
```
@kaji build a spec from this ClickUp task: https://app.clickup.com/t/[id]
```
```
@kaji rewrite this into a spec: [Notion URL / Mattermost thread / Fireflies meeting]
```

**Kaji will:**
1. Read all provided sources (ClickUp, Mattermost, Notion, Fireflies) before asking anything
2. Fetch the client's website for company background (Section 0) and brand colors (Section 12)
3. Ask at most 3 questions if critical info is still missing
4. Build the full spec and show it
5. Offer to write it into one ClickUp task (no subtasks)

---

## What You Get

```
Section 0   Company Introduction
Section 1   Overview
Section 2   What the App Does
Section 3   Core Experience
Section 4   Key Capabilities (App Functions)
Section 5   Intelligence Layer (LLM, agent, tools)
Section 6   Systems Connected (with mock status)
Section 7   Example User Journey
Section 8   Example Interaction (real message + response)
Section 9   What Makes This Valuable
Section 10  Scope for POC
Section 11  Future Enhancements
Section 12  Design, UI & UX (brand colors, components, layout)

Mock/Real System Map
  ├── System | Status | Mock Strategy | Real Integration | What Changes on Go-Live
  └── Environment table: POC (Shakudo dev) vs Go-Live (client env)

Shakudo Platform Map
  └── App Function | Shakudo Component | Role

POC Build Brief
  ├── Environment vars (dev block + go-live block)
  ├── Build checklist (checkboxes)
  ├── Acceptance criteria (checkboxes)
  ├── Go-live requirements from client (checkboxes)
  └── POC walk-through (5–10 steps for the presenter)
```

---

## Key Rules

- **POC is always pre-built** — never described as a live build
- **One ClickUp task, no subtasks** — the description IS the full spec
- **Spec shown first** — Kaji never offers to update ClickUp before the spec is on screen
- **Research before questions** — Kaji reads all available sources before asking anything
- **Never assume the tech stack** — always checks Notion/Mattermost for actual client infra

---

## File Structure

```
kaji-poc-spec-builder/
├── SKILL.md                                  ← Kaji execution logic
├── README.md                                 ← This file
├── assets/
│   ├── app-spec-template.md                  ← 13-section spec template (sections 0–12)
│   └── demo-architecture-template.md         ← Mock/real map + build brief format
└── references/
    ├── shakudo-platform-capabilities.md       ← Kaji, n8n, microservice, Dremio, Arize Phoenix
    ├── mock-patterns.md                       ← Mock strategies by system type
    └── demo-library.md                        ← Existing POCs to fork (Gallo, Reagan, HR, Campbell, Dynex)
```

---

## Related Skills

- **kaji-agentic-engineering-estimator** — Price the engagement from the spec
- **kaji-client-onboarding** — Plan the full deployment after the POC is approved
- **rob-weekly-proserve-update** — Track POC builds and client activity in weekly reports
