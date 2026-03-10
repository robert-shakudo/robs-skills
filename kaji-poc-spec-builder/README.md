# kaji-poc-spec-builder

**Convert any client use case into a complete, buildable POC spec — in one Kaji conversation.**

---

## How This Skill Works

When you trigger this skill, Kaji loads the following and uses them to run:

| File | What It Does |
|------|-------------|
| `SKILL.md` | Execution logic — the phases, rules, output format, anti-patterns, and checklist Kaji follows |
| `assets/app-spec-template.md` | The 13-section spec template (sections 0–12) that gets filled out |
| `assets/demo-architecture-template.md` | Mock/real system map + POC build brief format |
| `references/shakudo-platform-capabilities.md` | Catalog of Shakudo and Kaji components — used to map app functions to platform components |
| `references/mock-patterns.md` | Standard mock strategies by system type (CRM, ERP, DB, etc.) — used to fill the mock/real map |
| `references/demo-library.md` | Library of existing POCs (Gallo, Reagan, HR Resume, Campbell, Dynex) — used to identify what to fork |

**Context Kaji reads before asking you anything:**
- ClickUp task (description, comments, subtasks)
- Mattermost channel or thread (for context, decisions, corrections)
- Notion pages (for client architecture, infra details, security posture)
- Fireflies meeting summaries (for pain points, agreed scope, systems mentioned)
- Anything you paste directly

**Instructions Kaji follows:**
- Research first, ask questions second
- Ask if client infra is known or should default to Shakudo dev
- Show the full spec before offering any ClickUp update
- Ask if you want a walk-through (never auto-generates it)
- Write output into ONE ClickUp task, no subtasks
- Never use "demo" language — it's a POC
- Never describe a live build — the POC is always pre-built

---

## Trigger Phrases

```
@kaji build me a spec for [client] — [use case]
@kaji spec this out: [paste notes / email / problem]
@kaji build a spec from this ClickUp task: [URL]
@kaji rewrite this into a spec: [URL or paste]
@kaji what would we build for [client/problem]?
```

To print the walk-through at any time:
```
@kaji show me the walk-through
@kaji walk-through for this spec
@kaji how do we present this POC?
```

---

## What Happens Step by Step

**1. Kaji reads all available sources first**
Before asking anything, Kaji reads the ClickUp task, Mattermost thread, Notion pages, or Fireflies meeting you pointed to. It summarizes what it found in 2–3 sentences and proceeds.

**2. Kaji asks the infrastructure question**
> *"Do you have the client's infrastructure details (cloud, auth, LLM setup)? Or should I default to Shakudo dev for the POC and mark go-live as TBD?"*

- **If you have it** → client infra fills Section 0, Section 6, and the environment table
- **If not** → POC uses `dev.hyperplane.dev` (Shakudo GCP), go-live marked `[TBD]`

**3. Kaji asks up to 3 more questions** (only if critical info is still missing)
- Who is the primary user?
- What are they doing manually today?
- What makes this POC successful for the client?

**4. Kaji fetches the client website** (if client name is known)
- Section 0: company background, industry, size, tech environment
- Section 12: brand colors, font, logo
- Done in one pass

**5. Kaji builds and shows the full spec**
- Sections 0–12 (no placeholders)
- Mock/Real System Map with environment table
- Shakudo Platform Map

**6. Kaji asks if you want a walk-through**
> *"Do you want a POC walk-through added?"*

- **Yes** → generates the presenter flow table (5–10 steps with talking points) and shows it
- **No** → skips it
- Walk-through is **not included** in the ClickUp task — it's a separate presenter output only

**7. Kaji offers to update ClickUp**
- Writes the full spec into ONE task, no subtasks
- Description IS the spec

---

## What You Get

```
Section 0   Company Introduction (who they are, size, tech env, audience)
Section 1   Overview (app name, purpose, users, problem)
Section 2   What the App Does
Section 3   Core Experience (step-by-step user flow)
Section 4   Key Capabilities (App Functions)
Section 5   Intelligence Layer (LLM, agent, tools)
Section 6   Systems Connected (with mock status + environment)
Section 7   Example User Journey
Section 8   Example Interaction (real message + real response)
Section 9   What Makes This Valuable
Section 10  Scope for POC (in / out / Year 2)
Section 11  Future Enhancements
Section 12  Design, UI & UX (brand colors, components, layout)

Mock/Real System Map
  ├── System | Status | Mock Strategy | Real Integration | What Changes on Go-Live
  └── Environment: POC (Shakudo dev.hyperplane.dev) → Go-Live (client env)

Shakudo Platform Map
  └── App Function | Shakudo Component | Role

POC Build Brief
  ├── Environment vars (dev block + go-live block)
  ├── Build checklist (checkboxes)
  ├── Acceptance criteria (checkboxes)
  └── Go-live requirements from client (checkboxes)

Walk-Through (optional, on request only — not in ClickUp)
  └── Step | User Does | App Does | Talking Point
```

---

## Key Rules (Built Into the Skill)

| Rule | Why |
|------|-----|
| POC is always pre-built | Client sees a working app, not a construction site |
| POC runs in Shakudo dev (GCP) | Fastest to build and demo; same app deploys to client env on go-live |
| One ClickUp task, no subtasks | Description IS the full spec — everything in one place |
| Walk-through is optional and separate | Presenter flow ≠ build spec; not everyone needs it |
| Show spec before offering ClickUp update | You read it first, then decide what to do |
| Ask about infra before assuming | Client stack is often different from what's in the task |

---

## File Structure

```
kaji-poc-spec-builder/
├── SKILL.md                                  ← Execution logic Kaji follows
├── README.md                                 ← This file
├── assets/
│   ├── app-spec-template.md                  ← 13-section spec template (sections 0–12)
│   └── demo-architecture-template.md         ← Mock/real map + build brief format
└── references/
    ├── shakudo-platform-capabilities.md       ← Kaji, n8n, microservice, Dremio, Arize Phoenix
    ├── mock-patterns.md                       ← Mock strategies by system type
    └── demo-library.md                        ← Existing POCs: Gallo, Reagan, HR, Campbell, Dynex
```

---

## Related Skills

- **kaji-agentic-engineering-estimator** — Price the engagement from the spec
- **kaji-client-onboarding** — Plan the full deployment after the POC is approved
- **rob-weekly-proserve-update** — Track POC builds and client activity in weekly reports
