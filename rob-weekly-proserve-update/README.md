# Rob's Weekly ProServe Update — Skill

Generates Robert's GM-level weekly Professional Services strategic update, mapped to OKRs, pulling from every data source Shakudo runs on.

---

## How to Trigger

Say any of:
- `"weekly update"`
- `"functional update"`
- `"proserve report"`
- `"what happened this week"`
- `"run my weekly report"`

The skill will run an **interactive pre-flight** before pulling data — asking you to confirm context and fill in what it can't find in tools.

---

## What It Covers

### The 5 OKR Objectives

All client activity is mapped to one or more of:

| # | Objective | Focus |
|---|-----------|-------|
| 1 | Kaji Adoption | Internal team utilization, dev stream automation |
| 2 | Agentic FDE Offering | Methodology delivery, client transitions |
| 3 | Strategic Client Relationships | Exec sync coverage, SOW pipeline, dark accounts |
| 4 | Revenue Growth | Net-new clients, tier upgrades, conversion |
| 5 | Enterprise Support | PagerDuty, environment access, proactive monitoring |

Full OKR details with KRs, current state, and priorities: [`references/okrs-2026.md`](./references/okrs-2026.md)

---

## Data Sources

The skill pulls from **5 live systems** every week:

| Source | What It Finds |
|--------|--------------|
| **Mattermost** | EOW updates, blockers, decisions, client threads, lt-standup, Rob-and-Kaji channel |
| **Fireflies** | Meeting transcripts, decisions, action items from client calls |
| **ClickUp** | Sprint tasks, deliverables, blockers, completed work per client |
| **HubSpot** | Email threads, proposals, SOW status, deal updates, renewal discussions |
| **Memory (Graphiti)** | Context from previous weeks — carries forward unresolved items, patterns, open questions |

Full source details and search instructions: [`references/data-sources.md`](./references/data-sources.md)

---

## How Memory Works

The skill maintains **week-to-week memory** using Graphiti graph memory:

1. **Start of report:** loads last 4 weeks of stored context — open blockers, pending priorities, patterns across clients
2. **During synthesis:** cross-references current findings against stored history
3. **End of report:** you confirm what to save, then the skill saves a structured episode for the current week

This means:
- Recurring issues (like Reagan cert expiry) get flagged as patterns
- Unresolved priorities carry forward automatically
- You don't re-explain context each week

Weekly history log: [`references/weekly-history.md`](./references/weekly-history.md)

---

## Interactive Pre-flight

Before pulling any data, the skill asks:

1. **Date range** — "What week are we covering?" (auto-suggests current week)
2. **Fresh context** — "Anything new I should know before I pull? Deals closed, new clients, important context not in tools?"
3. **Per-objective prompts** — Quick check-in on each of the 5 OKRs for anything time-sensitive
4. **Confirmation** — "I'll now pull from Mattermost, Fireflies, ClickUp, HubSpot, and memory. Ready?"

After pulling:
5. **Review findings** — Presents key items per objective, asks "Anything missing or wrong?"
6. **Generate report**
7. **Memory save** — "Anything specific to save for next week?"

---

## File Structure

```
rob-weekly-proserve-update/
├── SKILL.md                    # Agent instructions (this is what Kaji reads)
├── README.md                   # This file — for humans
└── references/
    ├── okrs-2026.md            # Full OKR definitions, KRs, current state, priorities
    ├── client-registry.md      # All active clients with phase (Day 0-30, 30-60, 60+)
    ├── channel-map.md          # Mattermost channel IDs per client
    ├── data-sources.md         # How to pull from each data source
    └── weekly-history.md       # Week-to-week memory log
```

---

## Team Reference

| Person | Role | Notes |
|--------|------|-------|
| Robert | GM, ProServe | Owns the report, strategic decisions |
| Juliana | Engagement Manager | EM updates, client coordination |
| Alok | Engineering Lead | Sprint coordination, capacity |
| Yiran, Usama, Daniel, Hassan, Umang | ProServe Engineers | Delivery work |
| Yevgeniy, Ethan | Sales/Execs | Attribute separately from ProServe work |
| Chinthuran | Apps Team Lead | Attribute separately — not ProServe |

**Critical rule:** Never claim another team's work as ProServe. If Apps built it, say Apps built it.

---

## Output Format

The report is structured as:
- **Per-client sections** with phase, factual summary, cross-team attribution, OKR tag, and GM-level strategic commentary
- **Blockers Summary table** at the end
- **Churned/Inactive Accounts table** if applicable

No color emojis. No OKR list at top. Strategic, not a task list.
