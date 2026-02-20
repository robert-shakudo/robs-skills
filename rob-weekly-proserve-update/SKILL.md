---
name: rob-weekly-proserve-update
description: Generate Robert's weekly Professional Services strategic update by reviewing all company channels, Rob-and-Kaji notes, Fireflies meetings, and Mattermost activity. Produces a GM-level strategic report per client. Use when Robert asks for his weekly update or functional update.
license: MIT
compatibility: opencode
metadata:
  author: robert
  version: "1.0"
  category: reporting
  priority: p1
---

# Rob's Weekly Professional Services Strategic Update

## When to Use This Skill

This skill should be invoked when Robert asks for:
- His weekly update
- Functional update
- ProServe report
- "What happened this week"
- Weekly summary or overview

**Trigger phrases:**
- "weekly update"
- "functional update"
- "proserve report"
- "what happened this week"
- "run my weekly report"

## Prerequisites

Before running this skill, ensure:
- Mattermost MCP tools are available (`mattermost_*` functions)
- Fireflies MCP tools are available (`fireflies_*` functions)
- Access to Shakudo's internal Mattermost workspace
- Robert's user ID: `czhmoe714jrt7rxnxc78wn57me`
- Team ID: `e84jmiiex3fciejstpig4qot3o`

## Overview

This skill generates a GM-level strategic weekly report for Professional Services by:
1. Collecting data from all customer channels, Rob-and-Kaji notes, and Fireflies meetings
2. Synthesizing updates into a strategic format focused on relationships, revenue, and risk
3. Producing a structured markdown report organized by customer with health indicators

**Strategic Focus:** This is NOT a task list. Robert is the GM and needs to know WHY things matter, WHAT risks exist, and WHERE he should focus attention.

---

## Data Collection Phase

Follow these steps sequentially, using parallel calls where possible.

### Step 1: Determine the Reporting Week

- Default to the current week (Monday-Friday)
- If Robert specifies a week, use that date range
- Calculate the Monday and Friday dates for the reporting period
- Format as `YYYY-MM-DD` for API queries

### Step 2: Scan All Customer Company Channels

Review ALL customer channels for "End of Week Update" posts and significant activity. Use parallel calls to fetch multiple channels simultaneously.

**Customer Channel Map:**

| Customer | Channel ID | Channel Name |
|----------|------------|--------------|
| CentralReach | z7mp4zwxwjf7zejx73f9snon9o | Company Channel - CentralReach |
| CloudHQ | usjmzm83m7ytpfcud3ix64btpo | Company Channel - CloudHQ |
| EJ Gallo | 36b1cxary3fgzyjyiht8178oxa | Company Channel - EJ Gallo |
| FlexiVan | n7koczpmxpdc5gccdc6jtzb8gy | Company Channel - FlexiVan |
| Huntington National Bank | qhco3cqqipy65mnkwoxo9ppepe | Company Channel - Huntington National Bank |
| J.T. Magen | 3zjffm3gfgdxkmzg8rp785d8w | Company Channel - J.T. Magen & Company Inc. |
| KEV Group | u4x4cym7spr3zr8npikt5in4jh | Company Channel - KEV Group Inc |
| MCP | zyu3k63tktgojj1z5jot746way | Company Channel - MCP |
| QuadReal | 8dj19yq4yp8uum153onwn7mf1a | Company Channel - QuadReal |
| Thales Group / Hitachi | xc7bfhmquf8o58djmhybmmdknc | Company Channel - Thales Group / Hitachi |
| Whitecap Resources | g3agr94typ843fpr94maoqez4o | Company Channel - Whitecap Resources |
| Ronald Reagan Foundation | eqxee6yput88u8yjxaecykcefr | Ronald Reagan Presidential Foundation & Institute |

**Customers requiring channel search:**
- Loblaw → Search for "Company Channel - Loblaw Companies Limited"
- Campbell & Company / Martinrea → Search for "Company Channel - Campbell & Company"
- Annexus Group → Search for "Company Channel - Annexus Group"
- BWX Technologies → Search for "Company Channel - BWX Technologies"
- Investment Company Institute (ICI) → Search for "Company Channel - Investment Company Institute"
- Strategic Pharmacy Partners → Search for "Company Channel - Strategic Pharmacy Partners"
- Dynex → Search for "Company Channel - Dynex"
- Ritual → Search for "Company Channel - Ritual"

**For channels without IDs:**
1. Use `mattermost_search_channels` or browse team channels to locate them
2. Look for channels starting with "Company Channel -"
3. Record the channel ID for future reference

**For each channel:**
1. Search for posts containing "End of Week Update" from the reporting week
2. Search for posts with keywords: "blocker", "risk", "issue", "decision", "milestone", "delivered"
3. Scan for any high-engagement threads or important updates
4. Note if the channel was silent (no activity)

**Filtering Rules:**
- EXCLUDE session setup messages, bot commands, and kaji's own responses
- EXCLUDE routine status updates unless they contain strategic information
- INCLUDE customer sentiment, stakeholder changes, SOW discussions, blockers, deliverables

### Step 3: Review Rob-and-Kaji Channel

Channel ID: `6a3metsdnpn77q69ydj3tg6iwa`

Look for:
- What Robert personally worked on that week
- Strategic decisions or discussions with kaji
- New initiatives, process changes, or workflow improvements
- Client meeting prep or follow-ups
- Kaji Agentic Engineering delivery model updates
- Team capacity or resourcing discussions
- Process improvements or new frameworks being developed
- OKR discussions or quarterly goal references
- 30-60-90 client lifecycle discussions
- Sprint/SOW model changes

**Key Questions:**
- What strategic decisions did Robert make?
- What new initiatives started?
- What problems is Robert solving?
- What customer relationships is he managing directly?

### Step 4: Search Fireflies for Robert's Meetings

Use `fireflies_fireflies_search` to find meetings from the reporting week:

```
query: "from:YYYY-MM-DD to:YYYY-MM-DD participants:robert@shakudo.io"
```

Or use `fireflies_fireflies_get_transcripts` with `fromDate` and `toDate` filters.

**Meeting Categories to Identify:**
- Customer meetings (strategic, escalation, check-ins)
- Internal ProServe team discussions
- Sales/pipeline meetings relevant to ProServe
- Stakeholder alignment or leadership meetings

**For each relevant meeting:**
1. Get the transcript or summary
2. Extract: key decisions, action items, customer sentiment, strategic shifts
3. Note: which customer, what was discussed, what Robert committed to

### Step 5: Check Customer Engagement Team Channel

Channel ID: `3fut87ap6tyadbsqo7z3me98ay`

Scan for:
- New sales opportunities that may become ProServe engagements
- Customer escalations or support issues
- Pipeline updates affecting ProServe capacity planning
- Customer feedback or sentiment relevant to ProServe delivery

### Step 6: Parallel Execution Strategy

To maximize efficiency:
1. Fire all known customer channel searches in parallel (batch the `mattermost_get_posts` or `mattermost_search_posts` calls)
2. While those run, search for unknown channels
3. Query Fireflies and Rob-and-Kaji channel simultaneously
4. Collect results as they arrive

---

## Report Synthesis Phase

### Output Format

The report MUST follow this EXACT structure:

```markdown
# Professional Services Weekly Update | [Month Day-Day, Year]

---

## Customer Updates

### [Customer Name] (SOW: [Active/Re-engagement/Proposal Stage/Pre-Sales] | Phase: Day [N] - [Foundation/Acceleration/Scale]) [🟢/🟡/🔴]
**Update**

- [Bullet points of what happened this week]
- [Key milestones, deliverables, meetings]
- [Any blockers or issues]

**Strategic**: [1-2 sentences of GM-level strategic commentary - WHY this matters, what it means for the relationship, what Robert needs to know/act on]

**OKR Alignment**: [Which company/team OKR does this client engagement map to? Is it on track?]

---

[Repeat for each customer with activity or status]

## Accounts Needing Attention

| Customer | Risk | Action Needed |
|----------|------|---------------|
| [Customer] | [Brief risk description] | [What Robert should do] |

## Process & Internal Updates

- [Any internal team changes, process improvements, new tools]
- [Kaji Agentic Engineering updates if applicable]
- [Team capacity or resourcing notes]
- [Robert's personal strategic work this week]

## Processes Being Built/Improved This Week

- [Active process work Robert is driving]
- [New frameworks, templates, or workflows being developed]
- [Example: "Kaji Agentic Engineering v1 - formalized ProServe + Dev + EM workflow"]
- [Example: "Sprint-based SOW model with CR process for scope changes"]
- [Example: "Skill Marketplace security scanning workflow"]

## Week Ahead Preview (Optional)

- [Known upcoming customer meetings]
- [Anticipated deliverables or milestones]
- [Risks to watch]
```

### Health Status Indicators

Use these criteria for customer health:

- **🟢 Healthy:** On track, strong engagement, no blockers, meeting commitments
- **🟡 Needs Attention:** Minor blockers, slight delays, engagement dipping, upcoming decisions needed
- **🔴 At Risk:** Major blockers, customer dissatisfaction, missed commitments, churn risk, escalation required

### SOW Status Categories

- **Active:** Currently delivering under signed SOW
- **Re-engagement:** Previously active, now returning or restarting
- **Proposal Stage:** SOW being negotiated or proposed
- **Pre-Sales:** Discovery phase, scoping, not yet formally engaged
- **On Hold:** Paused by customer or Shakudo
- **Completed:** SOW finished, potential for renewal

### Strategic Commentary Guidelines

For each customer, the "Strategic" section should answer:
- **WHY does this matter?** (Impact on revenue, relationship, or delivery model)
- **WHAT should Robert know?** (Decision needed, risk to mitigate, opportunity to seize)
- **WHERE should attention go?** (Stakeholder to engage, blocker to remove, team to support)

**Examples:**
- "Strategic: Strong momentum—this is our showcase for Kaji Agentic Engineering delivery. Keep stakeholder engagement high as we approach renewal discussions in Q2."
- "Strategic: Relationship at risk due to missed delivery. Robert needs to engage VP stakeholder directly and commit to revised timeline."
- "Strategic: Low activity suggests deprioritization on their end. Recommend proactive check-in to reconfirm commitment or pause SOW."

### Content Curation Rules

**INCLUDE:**
- Relationship dynamics (stakeholder changes, sentiment shifts)
- Revenue implications (SOW expansions, renewals, at-risk contracts)
- Strategic risks (technical blockers that threaten timelines, customer dissatisfaction)
- Delivery model wins (successful Kaji Agentic Engineering use cases)
- Cross-customer patterns (same issue appearing in multiple accounts)

**EXCLUDE:**
- Granular technical task lists (unless they represent a strategic blocker)
- Routine status updates without strategic context
- Session setup messages, bot interactions, kaji's own responses
- Internal Shakudo operations not relevant to ProServe

### Handling Missing Data

- **No "End of Week Update" post:** Check for any activity in the channel; if none, note "No updates this week"
- **Silent customer channel:** Flag as "No activity—recommend proactive check-in"
- **New customers not in channel map:** Include them with a note: "NEW - not in standard channel map; channel ID [ID]"
- **Missing Fireflies meetings:** Note "No customer meetings this week" if none found

---

## Kaji Agentic Engineering Context

When synthesizing updates, frame delivery in the context of Shakudo's ProServe model:

**Kaji Agentic Engineering = Process + Team + Agents**

- **What it is:** Delivery cut from days to minutes using Kaji agents for component installation, support escalation resolution, and use case delivery
- **How it works:** Teams work on USE CASES collaboratively with agents, not just task execution
- **Why it matters:** This is Shakudo's differentiated ProServe model; Robert needs to know where it's winning and where it's struggling

**When mentioning Kaji in reports:**
- Highlight successful use case deliveries enabled by Kaji
- Note where agents reduced delivery time or manual effort
- Flag where Kaji adoption is lagging or blocked
- Connect customer satisfaction to Kaji-enabled delivery speed

---

## 30-60-90 Day Client Framework

For each customer, assess where they are in the engagement lifecycle:

### Framework Definition
- **Day 0-30 (Foundation):** Platform deployed, initial use cases scoped, team onboarded, first quick wins delivered
- **Day 30-60 (Acceleration):** Use cases in production, team self-sufficient on basics, expansion use cases identified, value metrics established
- **Day 60-90 (Scale):** Multiple use cases live, internal champions cultivated, renewal/expansion conversations started, ROI documented

### How to Apply
For each customer in the report, add a "**Phase**" line after the SOW status:
- Example: `### EJ Gallo (SOW: Active | Phase: Day 45 - Acceleration) 🟢`
- Estimate the day count from SOW start or re-engagement start
- Note what milestones should be hit at this phase vs what has been achieved
- Flag if a customer is behind their phase target

### Phase Assessment Questions
- **Foundation (0-30):** Is the platform stable? Are use cases scoped? Is the team engaged?
- **Acceleration (30-60):** Are use cases delivering value? Is the team building capability? Are we seeing expansion signals?
- **Scale (60-90):** Is value documented? Are champions advocating? Is renewal/expansion on the table?

---

## OKR Alignment

When building the report, connect each customer engagement to relevant OKRs:

### How to Identify OKRs
1. Search for OKR posts in Mattermost (search terms: "OKR", "objective", "key result", "Q1 goals", "Q2 goals")
2. Check if Robert has referenced OKRs in Rob-and-Kaji channel
3. If no formal OKRs found, infer from strategic priorities:
   - Revenue growth / renewal targets
   - Customer expansion / upsell
   - Delivery efficiency (Kaji Agentic Engineering adoption)
   - Customer satisfaction / NPS
   - Team capability building

### OKR Mapping per Customer
For each customer, note:
- Which OKR(s) this engagement supports
- Whether progress this week moved the OKR forward or backward
- Any risks to OKR achievement

---

## Example Invocations

Robert will trigger this skill with phrases like:

```
@kaji weekly update
@kaji run my proserve weekly report
@kaji functional update this week
@kaji what happened in proserve this week
@kaji give me the weekly summary
```

---

## Execution Checklist

Before submitting the report, verify:

- [ ] All known customer channels scanned
- [ ] Rob-and-Kaji channel reviewed for strategic context
- [ ] Fireflies meetings searched and relevant ones analyzed
- [ ] Customer Engagement Team channel checked
- [ ] Each customer has a health indicator (🟢🟡🔴)
- [ ] Each customer has a SOW status
- [ ] 30-60-90 phase assessed for each customer
- [ ] OKR alignment noted where applicable
- [ ] Strategic commentary provided for each customer
- [ ] "Accounts Needing Attention" table populated (if any)
- [ ] Process & Internal Updates section completed
- [ ] Processes section populated with active improvement work
- [ ] No bot messages, session setup, or kaji responses included
- [ ] Report is strategic, not tactical
- [ ] Date range is clearly stated in title

---

## Error Handling

If data collection fails:
- Note which sources were unavailable (e.g., "Fireflies API unavailable")
- Proceed with available data and flag gaps explicitly
- Do NOT fabricate data or make assumptions

If a customer channel cannot be found:
- Note: "[Customer] - Channel not found; unable to include in this report"
- Recommend updating the channel map

If Robert's meeting data is incomplete:
- Note: "Fireflies data incomplete for [dates]"
- Include only confirmed meetings

---

## Continuous Improvement

After each weekly report:
- Note any new customers that should be added to the channel map
- Flag any channels that have moved or been renamed
- Record any strategic commentary patterns that worked well
- Suggest process improvements to Robert for better data capture

---

## Notes

- This skill was developed iteratively over 3 weeks (Jan 26-30, Feb 2-6, Feb 9-13, 2026)
- Format evolved based on Robert's feedback and actual usage patterns
- Focus shifted from tactical task tracking to strategic GM-level insights
- Health indicators and SOW status added to enable quick risk assessment
- Kaji Agentic Engineering framing added to align with delivery model narrative
