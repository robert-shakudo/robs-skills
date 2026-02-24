---
name: rob-weekly-proserve-update
description: Generate Robert's weekly Professional Services strategic update by reviewing all company channels, Rob-and-Kaji notes, Fireflies meetings, and Mattermost activity. Produces a GM-level strategic report per client. Use when Robert asks for his weekly update or functional update.
license: MIT
compatibility: opencode
metadata:
  author: robert
  version: "2.0"
  category: reporting
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

## OKRs

See [references/okrs-2026.md](./references/okrs-2026.md) for the current OKR cycle. Three objective areas:
- **Kaji Onboarding** (Internal + External)
- **Agentic FDE Offering**
- **Strategic Client Relationships**

When assigning OKR alignment in the report, use the objective name (not "Obj 1 KR2").

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

See [references/channel-map.md](./references/channel-map.md) for all client channel IDs and internal channels.

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

See [references/client-registry.md](./references/client-registry.md) for the full client list with phases. Update that file as clients move phases — do not edit SKILL.md for phase changes.

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


