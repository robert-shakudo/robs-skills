---
name: rob-weekly-proserve-update
description: Generate Robert's weekly Professional Services strategic update by reviewing all company channels, Rob-and-Kaji notes, Fireflies meetings, and Mattermost activity. Produces a GM-level strategic report per client. Use when Robert asks for his weekly update or functional update.
license: MIT
compatibility: opencode
metadata:
  author: robert
  version: "3.0"
  category: reporting
---

# Rob's Weekly Professional Services Strategic Update

Generates Robert's GM-level weekly ProServe report. Reviews Mattermost, Fireflies, ClickUp, HubSpot, and memory per client, separates ProServe work from other teams, maps everything to OKRs, and flags risks. This is NOT a task list — it surfaces what matters strategically.

**Key upgrade in v3.0:** Interactive pre-flight, week-to-week memory, HubSpot/email coverage, 5 OKR objectives.

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
- ClickUp MCP tools (`ClickUp_*`)
- Graphiti memory tools (`graphiti-memory_*`)
- Robert's user ID: `czhmoe714jrt7rxnxc78wn57me`
- Team ID: `e84jmiiex3fciejstpig4qot3o`

---

## PHASE 1: Interactive Pre-flight (ALWAYS run first)

Before pulling any data, run through this conversation with Robert. Do not skip steps.

### Step 1 — Confirm Date Range
Say: *"What week are we covering? (I'll default to [last Mon–Fri] if you don't specify)"*
- If Robert confirms or stays silent → use the most recent Mon–Fri range
- If Robert specifies → use that range for all data pulls

### Step 2 — Load Memory
Before asking further questions, silently load previous week context:
1. Read the last 2–4 entries in `references/weekly-history.md`
2. Run: `graphiti-memory_search_memory_facts(query="ProServe weekly carry-forwards open blockers priorities")`
3. Run: `graphiti-memory_search_memory_facts(query="dark accounts Campbell Whitecap Dynex Hitachi")`
4. Note any carry-forwards and patterns — surface them during the per-objective check-ins

### Step 3 — Fresh Context Check
Say: *"Before I pull data — anything new I should know? Deals closed or signed, new clients, important context that won't be in Mattermost/Fireflies/ClickUp?"*

If Robert provides context:
- Accept it, note it as "per Robert"
- Ask: *"Should I save this to memory for future reports?"* If yes → `graphiti-memory_add_memory`

### Step 4 — Per-Objective Check-in
Run through each OKR objective quickly. Say:

*"Quick check on each objective — anything time-sensitive I should prioritize?"*

1. **Obj 1 (Kaji Adoption):** "Any internal team updates — utilization, sprint, Kaji reliability this week?"
2. **Obj 2 (Agentic FDE):** "Any Agentic FDE methodology progress or new client conversations to flag?"
3. **Obj 3 (Strategic Relationships):** "Any exec meetings, dark account outreach, or SOW movements?"
4. **Obj 4 (Revenue Growth):** "Any deals signed, deals at risk, or tier conversations started?"
5. **Obj 5 (Enterprise Support):** "Any incidents, access issues, or PagerDuty progress?"

If Robert adds info for any objective → log it, ask: *"Save this to memory?"*

### Step 5 — Confirm Data Pull
Say: *"I'll now pull from Mattermost, Fireflies, ClickUp, and memory. For HubSpot/email — any SOW threads or deal emails I should know about? (I can't access HubSpot directly.)"*

Accept Robert's HubSpot/email input, then proceed.

---

## PHASE 2: Data Collection (Per Client — Follow in Order)

See `references/data-sources.md` for full tool instructions and search strategies.

### For Each Client:

**Step 1 — Mattermost**
- Pull the client's dedicated channel (see `references/channel-map.md`)
- For clients without a channel: search by name
- Check lt-standup for ProServe and EM updates
- Note who posted — ProServe vs other teams

**Step 2 — Fireflies**
- Search: `fireflies_search(query="[client name]", fromDate="...", toDate="...")`
- For each meeting: pull summary for decisions, action items, attendees
- Attribute: Robert / Juliana / another team ran it

**Step 3 — ClickUp**
- Search tasks by client name for the week
- Note completions, blockers, cross-team involvement

**Step 4 — Memory Cross-Reference**
- Check carry-forwards from previous weeks for this client
- Flag if a carry-forward is now resolved or still open

**Step 5 — HubSpot / Email (from Robert's input)**
- Document any SOW, email, or deal context Robert provided in pre-flight

### Always Check These Internal Channels:
- **Rob-and-Kaji** (`6a3metsdnpn77q69ydj3tg6iwa`): Strategic decisions, new initiatives, OKR references
- **Customer Engagement Team** (`3fut87ap6tyadbsqo7z3me98ay`): Pipeline updates, escalations

---

## PHASE 3: Synthesis Check (Before Generating Report)

After pulling all data, present a brief summary to Robert:

*"Here's what I found. Does anything look wrong or missing before I generate the report?"*

Present as:
- **Per OKR objective:** 2–3 key items found
- **Carry-forwards from last week:** resolved vs. still open
- **New patterns emerging:** anything flagged across 2+ weeks

Wait for Robert's confirmation. Incorporate any corrections. Ask if new info should be saved to memory.

---

## PHASE 4: Report Generation

### OKRs — 5 Objectives

See `references/okrs-2026.md` for full definitions, KRs, current state, and active priorities.

Use these exact tags when assigning OKR alignment:
- `Kaji Adoption` → Obj 1
- `Agentic FDE Offering` → Obj 2
- `Strategic Relationships` → Obj 3
- `Revenue Growth` → Obj 4
- `Enterprise Support` → Obj 5

A single client may map to multiple objectives — list all that apply.

### FORBIDDEN — Never Include
- Color emojis (🟢🟡🔴) anywhere in the document
- Summary status table with emojis
- OKR list at the top of the document
- "Day 15" phase — only use Day 0-30, Day 30-60, Day 60+

### Report Header
```
# Professional Services Weekly Update | [Date Range]
```

### Objectives Pulse Section (after header, before client sections)
```
## Objectives Pulse

**Obj 1 — Kaji Adoption:** [one sentence on current state + movement this week]
**Obj 2 — Agentic FDE Offering:** [one sentence]
**Obj 3 — Strategic Relationships:** [one sentence + coverage count e.g. "12/18 clients in sync"]
**Obj 4 — Revenue Growth:** [one sentence + pipeline status]
**Obj 5 — Enterprise Support:** [one sentence]
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

**OKR:** [tag(s)]

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

**OKR:** [tag(s)]

**Strategic**
[GM-level analysis]
```

### Phase Framework
- **Day 0-30:** Foundation — new client, platform setup, initial onboarding
- **Day 30-60:** Acceleration — first use cases, building momentum
- **Day 60+:** Scale — mature engagement, expansion, renewal conversations

**SOW rule:** Clients cannot have a SOW until Day 60.

---

## Team Structure

### ProServe (Robert's Organization)
- **Engineering**: Yiran, Usama, Daniel, Hassan, Umang, Alok
- **Engagement Management**: Juliana

### Other Teams — Attribute Separately
- **Applications Team**: Chinthuran and team
- **Stack Components Team**
- **Platform Team**
- **Sales/Execs**: Yevgeniy, Ethan

**CRITICAL**: When another team does the work, attribute it to them. ProServe may coordinate but never claims other teams' work.

---

## Structural Rules

### 1. Factual vs Strategic Separation
- **What Happened / Onboarding Status**: Verifiable facts only
- **Strategic**: GM-level analysis — why it matters, what Robert needs to do

### 2. Silent Accounts Must Have Reasons
Never say "No activity this week" without explaining WHY:
- On hold (state reason) / Waiting on client (state what) / Check-in overdue (flag it)

### 3. Cross-Functional Attribution
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

## PHASE 5: Memory Save (After Report)

After generating the report, say:

*"Report complete. Before we close — anything specific to save for next week? Any context, decisions, or priorities I won't find in the tools?"*

Then save:

1. **Graphiti memory** — save structured episode:
```
graphiti-memory_add_memory(
  name="ProServe Weekly Update [DATE RANGE]",
  episode_body="[summary: OKR pulse, key movements, open carry-forwards, patterns]",
  source="text",
  group_id="proserve-weekly",
  source_description="Weekly ProServe report summary"
)
```

2. **weekly-history.md** — append new entry using the template in `references/weekly-history.md`:
   - OKR Pulse (one line per objective)
   - Key Movements (3–5 bullets)
   - Open Carry-Forwards (items → carry to next week)
   - Patterns Flagged
   - Notes Saved by Robert

---

## Accounts to Track

See `references/client-registry.md` for the full client list with phases.

---

## Execution Checklist

**Pre-flight**
- [ ] Date range confirmed
- [ ] Previous week memory loaded (Graphiti + weekly-history.md)
- [ ] Robert's fresh context collected
- [ ] Per-objective check-in complete (all 5 objectives)
- [ ] HubSpot/email context collected from Robert

**Data Pull**
- [ ] All customer channels scanned
- [ ] lt-standup reviewed (ProServe Engineer + EM updates)
- [ ] Rob-and-Kaji channel reviewed
- [ ] Customer Engagement Team channel checked
- [ ] Fireflies meetings searched per client
- [ ] ClickUp tasks reviewed per client
- [ ] Memory carry-forwards checked per client

**Report**
- [ ] Objectives Pulse section included
- [ ] Each client has phase assessment
- [ ] Each client has OKR tag(s)
- [ ] Strategic commentary per client (GM-level, not task-level)
- [ ] Silent accounts have stated reasons
- [ ] Cross-team attribution explicit
- [ ] Deliverables have named owners
- [ ] Meetings attributed by name
- [ ] Blockers Summary table populated
- [ ] No color emojis anywhere
- [ ] No OKR list at top

**Memory Save**
- [ ] Graphiti episode saved
- [ ] weekly-history.md updated with new entry
- [ ] Robert's additional notes saved

---

## Error Handling

- Missing data source → note the gap explicitly, proceed with available data
- Channel not found → "[Client] - Channel not found; searched by name"
- Fireflies no results → "No Fireflies meetings found for [dates]"
- HubSpot not accessible → ask Robert directly
- Memory empty → proceed without cross-reference, note "first week of memory tracking"
- Never fabricate data or make assumptions
