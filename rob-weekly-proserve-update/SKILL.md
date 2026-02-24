---
name: rob-weekly-proserve-update
description: Generate Robert's weekly Professional Services strategic update by reviewing all company channels, Rob-and-Kaji notes, Fireflies meetings, and Mattermost activity. Produces a GM-level strategic report per client. Use when Robert asks for his weekly update or functional update.
license: MIT
compatibility: opencode
metadata:
  author: robert
  version: "2.0"
  category: reporting
  priority: p1
---

# Rob's Weekly Professional Services Strategic Update

Generates Robert's GM-level weekly ProServe report. Reviews Mattermost channels, Fireflies meetings, and ClickUp per client, separates ProServe work from other teams, maps everything to OKRs, and flags risks. This is NOT a task list — it surfaces what matters strategically.

## When to Use This Skill

Trigger phrases:
- "weekly update"
- "functional update"
- "proserve report"
- "what happened this week"
- "run my weekly report"

## Prerequisites

- Mattermost MCP tools (`mattermost_*`)
- Fireflies MCP tools (`fireflies_*`)
- Robert's user ID: `czhmoe714jrt7rxnxc78wn57me`
- Team ID: `e84jmiiex3fciejstpig4qot3o`

---

## Team Structure

### ProServe (Robert's Organization)
- **Engineering**: Yiran, Usama, Daniel, Hassan, Umang, Alok, and others
- **Engagement Management**: Juliana

### Other Teams — Attribute Separately
- **Applications Team**: Chinthuran and team
- **Stack Components Team**
- **Platform Team**
- **Sales/Execs**: Yevgeniy, Ethan, etc.

**CRITICAL**: When another team does the work, attribute it to them. If Apps built MCP servers, that's Apps work. ProServe may coordinate but never claims other teams' work.

---

## Robert's 2026 OKRs

### Kaji Onboarding (Internal + External)
- **KR1:** 100% of ProServe EMs and Engineers in Kaji-enabled dev streams with full automation by **Q1-26**
- **KR2:** Internal Dev/EM workflow with complete visibility (tasks, time, deliverables) by **Q1-26**
- **KR3:** Baseline team utilization metric established, reach 70% by **Q3-26**

### Agentic FDE Offering
- **KR1:** 80% of active clients onboarded to Agentic FDE / Agentic Engineering process by **Q2-26**
- **KR2:** Deliver updated Agentic Engineering model to all clients by **Q2-26**:
  - **New clients (Day 0-60):** New platform + component install process with Agentic delivery from day one
  - **Existing clients (Day 60+):** Use case-driven adoption — migrate via targeted use cases

### Strategic Client Relationships
- **KR1:** 90% of active clients with regular strategic meetings (weekly/bi-weekly) and documented outcomes by **Q1-26**
- **KR2:** Strategic architecture roadmaps to 80% of top-tier clients by **Q3-26**
- **KR3:** 75% monetization rate for all new ProServe engagements by **Q3-26**

---

## Data Collection (Per Client — Follow in Order)

### Step 1: Mattermost
- **lt-standup channel**: Check ProServe Engineer Team updates and Juliana's EM updates
- **Client channel**: EOW updates, blockers, decisions, significant threads
- Note who posted (ProServe vs other teams)

### Step 2: Fireflies
- Search past week for meetings with the client name
- Extract: attendees, key decisions, action items, Robert vs others

### Step 3: ClickUp
- Client tasks, sprint progress, blockers, completed work
- Note cross-team collaboration (Apps, Platform, Stack, Sales)

### Step 4: HubSpot (if applicable)
- Email threads, proposals, deal updates, SOW status, renewal discussions

### Step 5: Cross-Team Check
Scan for involvement from Apps (Chinthuran), Platform, Stack, Sales/Execs (Yevgeniy, Ethan).
If another team ran the demo, call, or technical work — call it out. State ProServe's specific role.

### Customer Channel Map

| Customer | Channel ID |
|----------|------------|
| CentralReach | z7mp4zwxwjf7zejx73f9snon9o |
| CloudHQ | usjmzm83m7ytpfcud3ix64btpo |
| EJ Gallo | 36b1cxary3fgzyjyiht8178oxa |
| FlexiVan | n7koczpmxpdc5gccdc6jtzb8gy |
| Huntington National Bank | qhco3cqqipy65mnkwoxo9ppepe |
| J.T. Magen | 3zjffm3gfgdxkmzg8rp785d8w |
| MCP | zyu3k63tktgojj1z5jot746way |
| QuadReal | 8dj19yq4yp8uum153onwn7mf1a |
| Thales Group / Hitachi | xc7bfhmquf8o58djmhybmmdknc |
| Whitecap Resources | g3agr94typ843fpr94maoqez4o |
| Ronald Reagan Foundation | eqxee6yput88u8yjxaecykcefr |

Search by name for: Loblaw, Campbell/Martinrea, Annexus, BWX Technologies, ICI, Strategic Pharmacy Partners, Dynex, Ritual.

Also review:
- **Rob-and-Kaji channel** (`6a3metsdnpn77q69ydj3tg6iwa`): Strategic decisions, new initiatives, OKR references
- **Customer Engagement Team** (`3fut87ap6tyadbsqo7z3me98ay`): Pipeline updates, escalations

---

## Output Format

### FORBIDDEN — Never Include
- Color emojis (🟢🟡🔴) anywhere in the document
- Summary status table with emojis
- OKR list at the top of the document
- "Day 15" phase — only use Day 0-30, Day 30-60, Day 60+

### Report Header
```
# Professional Services Weekly Update | [Date Range]
```

### Client Section — Scale Accounts (Day 60+)
```
### [Client Name]
**Phase:** Day 60+ (Scale)

**What Happened**
- [Factual bullet — what shipped, what was agreed, what's blocked]
- [Name the people, no "we" unless both Robert and Juliana attended]

**Cross-Team** (include only if applicable)
- [Team] did [X]; ProServe [coordinated/supported/led]

**OKR:** [Kaji Onboarding | Agentic FDE Offering | Strategic Client Relationships]

**Strategic**
[GM-level analysis: why this matters, what Robert needs to do, risk or opportunity]
```

### Client Section — Foundation / Acceleration (Day 0-60)
```
### [Client Name]
**Phase:** Day [X] (Foundation/Acceleration)

**Onboarding Status**
- [Current onboarding activities]
- [Platform setup progress]
- [Team enablement status]

**SOW:** [No SOW until Day 60 / In discussion / Active]

**Blocker** (if any)
- [Specific blocker with owner]

**OKR:** [Kaji Onboarding | Agentic FDE Offering | Strategic Client Relationships]

**Strategic**
[GM-level analysis]
```

### Phase Framework
- **Day 0-30:** Foundation — new client, platform setup, initial onboarding
- **Day 30-60:** Acceleration — first use cases, building momentum
- **Day 60+:** Scale — mature engagement, expansion, renewal conversations

**SOW rule:** Clients cannot have a SOW until Day 60.

---

## Structural Rules

### 1. Factual vs Strategic Separation
- **What Happened / Onboarding Status**: Verifiable facts only — what shipped, agreed, blocked
- **Strategic**: Robert's GM-level analysis — why it matters, what to do next

### 2. Silent Accounts Must Have Reasons
Never say "No activity this week" without explaining WHY:
- On hold (state reason)
- Waiting on client (state what)
- Check-in overdue (flag it)
- Deliberate pause (state why)

### 3. Cross-Functional Attribution
If someone else did the work, name them and their team:
- "Applications (Chinthuran) built the MCP servers; ProServe (Juliana) coordinated logistics"
- "Sales (Yevgeniy) led the demo; Robert provided strategic follow-up"

### 4. Deliverable Ownership
Every deliverable needs a name: "Kaji skill built by [Name]", "SOW draft prepared by [Name]"

### 5. Meeting Attribution
- Robert had the meeting → "Robert met with..."
- Juliana had the meeting → "Juliana met with..."
- Never say "we" unless both attended

---

## Closing Sections

### Blockers Summary
| Client | Blocker | Owner | Days Blocked |
|--------|---------|-------|--------------|

### Churned Accounts (if any)
| Client | Status | Last Activity | Notes |
|--------|--------|---------------|-------|

---

## Accounts to Track

| Client | Phase |
|--------|-------|
| CentralReach | Day 60+ - Scale |
| Loblaw | Day 60+ - Scale |
| Huntington | Day 60+ - Scale |
| EJ Gallo | Day 60+ - Scale |
| MCP | Day 60+ - Scale |
| QuadReal | Day 30 - Foundation |
| Thales/Hitachi | Day 60+ - Scale |
| BWX Technologies | Day 60+ - Scale |
| CloudHQ | Day 60+ - Scale |
| Annexus | Day 0-30 - Foundation |
| ICI | Day 0-30 - Foundation |
| Whitecap Resources | Day 60+ - Scale |
| FlexiVan | Day 60+ - Scale |
| Campbell/Martinrea | Day 60+ - Scale |
| J.T. Magen | Day 0-30 - Foundation |
| Reagan Foundation | Day 60+ - Scale |
| Strategic Pharmacy | Day 0 - Pre-engagement |

---

## Kaji Agentic Engineering Context

**Kaji Agentic Engineering = Process + Team + Agents**

When synthesizing updates:
- Highlight successful Kaji use case deliveries and where adoption is winning
- Note where Kaji reduced delivery time or manual effort
- Flag where Kaji adoption is lagging or blocked
- Connect customer satisfaction to Kaji-enabled delivery speed

---

## Execution Checklist

- [ ] All customer channels scanned (EOW updates + significant activity)
- [ ] lt-standup channel reviewed for ProServe Engineer and EM updates
- [ ] Rob-and-Kaji channel reviewed for strategic context
- [ ] Fireflies meetings searched and analyzed
- [ ] Customer Engagement Team channel checked
- [ ] Each client has phase assessment (Day 0-30, 30-60, 60+)
- [ ] Each client has OKR alignment (OKR name, not "Obj 1 KR2")
- [ ] Strategic commentary provided for each client (GM-level, not task-level)
- [ ] Silent accounts have stated reasons for inactivity
- [ ] Cross-team attribution explicit (ProServe vs Apps/Platform/Stack/Sales)
- [ ] Deliverables have named owners
- [ ] Meetings attributed by name (Robert vs Juliana vs joint)
- [ ] Blockers Summary table populated (if any)
- [ ] No color emojis anywhere in document
- [ ] No OKR list at top of document
- [ ] Report is strategic, not a task list
- [ ] Date range clearly stated in title

---

## Error Handling

- Missing data source → note the gap explicitly, proceed with available data
- Channel not found → "[Client] - Channel not found; cannot include in this report"
- Fireflies incomplete → "Fireflies data incomplete for [dates]"
- Never fabricate data or make assumptions

---

## Memory (Future Enhancement)

Track per-client state week-over-week:
- SOW status, phase, last activity date, current blockers, OKR alignment, key contacts, next milestone

This enables the skill to reference prior weeks automatically and flag stale accounts.

---

## Notes

- v2.0 — major update Feb 2026 based on weekly update review feedback
- Key changes: 2026 OKRs, NO color emojis, team attribution rules added, factual/strategic separation enforced, silent account requirement, phase framework simplified (0-30, 30-60, 60+), SOW timing rule (not before Day 60), team structure documented
