# Data Sources — ProServe Weekly Update

Every weekly report pulls from these 5 systems in order. Do not skip any source. Missing data from one source should be noted explicitly in the report.

---

## 1. Mattermost

**Tools:** `mattermost_get_channel_messages`, `mattermost_search_posts`

**Robert's user ID:** `czhmoe714jrt7rxnxc78wn57me`  
**Team ID:** `e84jmiiex3fciejstpig4qot3o`

### Always Check

| Channel | ID | What to Look For |
|---------|----|-----------------|
| Rob-and-Kaji | `6a3metsdnpn77q69ydj3tg6iwa` | Strategic decisions, new initiatives, OKR references, Robert's priorities |
| Customer Engagement Team | `3fut87ap6tyadbsqo7z3me98ay` | Pipeline updates, escalations, cross-team coordination |
| lt-standup | Search by name | ProServe Engineer updates, Juliana EM updates |

### Per-Client Channels (check each client)

See [`channel-map.md`](./channel-map.md) for full channel ID list.

For clients without a dedicated channel (Loblaw, Campbell/Martinrea, Annexus, BWX Technologies, ICI, Strategic Pharmacy Partners, Dynex, Ritual): search by client name using `mattermost_search_posts`.

### What to Extract
- EOW updates from engineers and Juliana
- Blockers mentioned in threads
- Decisions made (especially client-facing)
- Who posted (ProServe engineer vs. other team) — attribute correctly
- Significant threads started mid-week

---

## 2. Fireflies

**Tools:** `fireflies_fireflies_search`, `fireflies_fireflies_get_transcript`, `fireflies_fireflies_get_summary`

### Search Strategy
1. Search for each client name: `fireflies_search(query="[client name]", fromDate="[week start]", toDate="[week end]")`
2. For each meeting found: pull summary for action items and decisions
3. Note: attendees, who ran the meeting (Robert vs. Juliana vs. another team)

### What to Extract
- Key decisions made in calls
- Action items assigned (and to whom)
- Client sentiment signals (if present in notes)
- Whether Robert attended or not (attribution matters)
- Any commitments made (SOW, timeline, scope)

### Attribution Rule
- Robert ran the meeting → "Robert met with..."
- Juliana ran it → "Juliana met with..."
- Another team ran it → attribute to that team; note ProServe's role if any

---

## 3. ClickUp

**Tools:** `ClickUp_get_workspace_tasks`, `ClickUp_get_workspace_hierarchy`

### Search Strategy
1. Search tasks by client name for the week's date range
2. Check sprint tasks for completion and blockers
3. Look for cross-team tasks (Apps, Platform, Stack, Sales involvement)

### What to Extract
- Tasks completed this week (with owner names)
- Tasks blocked (with blocker reason and owner)
- Sprint progress vs. plan
- New tasks created (signals new scope or issues)
- Cross-team contributions (attribute correctly)

### Notes
- ClickUp tasks often confirm what was discussed in Mattermost or Fireflies
- Use ClickUp to validate delivery claims with specifics (task names, completion dates)

---

## 4. HubSpot

**Access:** Robert accesses HubSpot directly. Ask Robert to share relevant email threads or deal updates if not accessible via tool.

### What to Look For
- **Email threads** with client stakeholders (deals, proposals, escalations)
- **SOW status changes** (sent, reviewed, signed, declined)
- **Deal stage movements** in the pipeline
- **Renewal discussions** — especially for dark accounts (Whitecap, Campbell)
- **Exec-level conversations** Robert is having directly

### How to Handle HubSpot During Interactive Pre-flight
Ask Robert: *"Any HubSpot updates I should know about? SOWs sent or signed, email conversations with clients, deal movements?"*

Document whatever Robert shares and attribute it as "per Robert's HubSpot / email review."

---

## 5. Memory (Graphiti + Weekly History)

**Tools:** `graphiti-memory_search_memory_facts`, `graphiti-memory_search_nodes`, `graphiti-memory_add_memory`

**File reference:** [`weekly-history.md`](./weekly-history.md)

### At Report Start — Load
1. Read the last 4 entries in `weekly-history.md` for carry-forwards and patterns
2. Search Graphiti memory: `search_memory_facts(query="ProServe weekly update carry-forwards open blockers")` 
3. Search for each dark account: `search_memory_facts(query="[client name] status blockers")`
4. Load any pattern flags (e.g., "Reagan cert expiry pattern")

### What Memory Surfaces
- Unresolved carry-forwards from previous weeks
- Recurring issues flagged as patterns
- Context Robert manually added in previous sessions
- OKR trend signals (e.g., "Q1 revenue target at risk — 3 consecutive weeks with 0 signed")

### At Report End — Save
After generating the report, ask Robert:
> "Before we close — is there anything specific you want me to remember for next week? Any context that wasn't in the tools?"

Then save to both:
1. **Graphiti** via `add_memory` with source="text", group="proserve-weekly" — key decisions, carry-forwards, patterns
2. **weekly-history.md** — append new weekly entry using the standard format in that file

---

## Source Priority When Conflicting

| Conflict | Priority |
|----------|----------|
| Tool says X, Robert says Y | Robert's version is correct — note the discrepancy |
| Mattermost vs. Fireflies on same event | Use both, Fireflies often has more detail |
| ClickUp task vs. Mattermost update | Mattermost for current status; ClickUp for formal completion |
| Memory says open, tools show resolved | Confirm with Robert, update memory |

---

## Missing Data Protocol

Never fabricate or assume. If a source is unavailable:

- Mattermost channel not found → "No dedicated channel found for [client]; searched by name"
- Fireflies no results → "No Fireflies meetings found for [client] this week"
- ClickUp no tasks → "No ClickUp activity found for [client] this week"
- HubSpot not queried → "HubSpot not checked — ask Robert for email/deal updates"

Always note the gap explicitly in the report section for that client.
