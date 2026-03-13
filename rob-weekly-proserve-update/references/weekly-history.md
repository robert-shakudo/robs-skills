# Weekly History — ProServe Update Memory Log

This file is a running log of weekly report summaries. The skill uses this alongside Graphiti graph memory to build cross-week context: unresolved blockers, emerging patterns, priority carry-forwards, and OKR trend signals.

**How it's used:**
- At the start of each new report, the skill reads the last 4 entries and loads any open carry-forwards
- After generating a report, the skill writes a new entry here
- Graphiti memory stores richer relationship-level data; this file is the human-readable log

---

## Entry Format

```
## Week of [DATE RANGE]
Generated: [YYYY-MM-DD]

### OKR Pulse
- Obj 1 (Kaji Adoption): [status / key movement]
- Obj 2 (Agentic FDE): [status / key movement]
- Obj 3 (Strategic Relationships): [status / key movement]
- Obj 4 (Revenue Growth): [status / key movement]
- Obj 5 (Enterprise Support): [status / key movement]

### Key Movements
- [bullet: what changed, what shipped, what got decided]

### Open Carry-Forwards (unresolved from this week → next week)
- [ ] [item] — Owner: [name] — Context: [why it's open]

### Patterns Flagged
- [recurring issue, trend, or risk observed]

### Notes Saved by Robert
- [anything Robert added manually at end of session]
```

---

## Weekly Entries

<!-- Most recent entry at top -->

## Week of Mar 9–13, 2026
Generated: 2026-03-13

### OKR Pulse
- Obj 1 (Kaji Adoption): Adoption complete across ProServe. Support team pending. Utilization baseline still in progress.
- Obj 2 (Agentic FDE): AI Gateway demo to Huntington (Robert+Juliana+Scott, 11 HNB). Campbell spec + Larry in demo. Dynex MBS POC spec created. CloudHQ/Dynex/Reagan demos still pending.
- Obj 3 (Strategic Relationships): **LOBLAW SHUTDOWN** — major account risk. Larry (Campbell) confirmed exec engaged. Whitecap reactivating (2 new use cases). Juliana on vacation; Elizabeth covering CentralReach/CloudHQ/Reagan next week.
- Obj 4 (Revenue Growth): 0 new signatures. Strategic Pharmacy stalled (Ben newborn). Annexus IT unblocked — SOW starting. Multiple new Sales prospects (not yet ProServe).
- Obj 5 (Enterprise Support): Loblaw shutdown is primary incident. HNB cyber upgrade Monday. PagerDuty routing map still not built (4th week).

### Key Movements
- **Loblaw shut down Shakudo until further notice** (Mar 10) — data breach related; AI Gateway on hold; 67+ API co-development suspended
- Huntington AI Gateway demo delivered (Robert + Juliana + Scott Tadman, 11 HNB attendees)
- Campbell: Larry, Rick, Jimmy all in Mar 6 demo; 4 production use cases documented; Kaji Ops Co-Pilot spec complete
- Dynex: MBS Intelligence POC spec created by Robert (Mar 10); demo app deployed; demo not yet booked
- CentralReach: Kaji deployment progressing, Azure auth ongoing; Juliana on vacation Mar 14
- CloudHQ weekly sync: demo blocked on Floyd (2nd week)
- Martinrea: Seaweed migration from MinIO planned this weekend
- Annexus IT access unblocked; SOW process starting
- Whitecap reactivating: AI data discovery + K8s upgrade tasks opened

### Open Carry-Forwards
- [ ] **URGENT** Robert exec escalation at Loblaw — data breach/shutdown context, timeline and scope — Owner: Robert
- [ ] Robert post-demo follow-up to Campbell (Rick/Larry/Jimmy) — PoC spec + success metrics — Owner: Robert
- [ ] Book Dynex demo with Corbin; if unresponsive escalate to COO — Owner: Ethan
- [ ] Lock CloudHQ demo date once Floyd returns — Owner: Elizabeth (Juliana out)
- [ ] Lock Reagan meeting date before next Friday — Owner: Elizabeth
- [ ] Ethan reach Adam Houtman (CEO) at Strategic Pharmacy for budget/timeline confirmation — Owner: Ethan
- [ ] Robert email Ian at MCP re: Kaji for Teams requirements — Owner: Robert
- [ ] Annexus Growth option SOW conversation with Taylor — Owner: Robert
- [ ] PagerDuty routing map + responsibilities — Owner: Platform/Stack leads (4 weeks, no progress)
- [ ] Environment access audit per team per client — Owner: TBD (4 weeks, no owner)
- [ ] Support team onboarding to Kaji — Owner: Robert
- [ ] Utilization baseline with engineer-level tracking — Owner: Robert/Alok
- [ ] Agentic FDE blog post + EM training — Owner: Robert

### Patterns Flagged
- Loblaw: first major shutdown event — client data breach impacting Shakudo engagement; pattern to watch
- Demo-to-booking gap: Robert building demos (Dynex MBS app built, deployed) but demo still not booked — disconnect between build and schedule
- Juliana vacation coverage: Elizabeth now running 3 accounts (CentralReach, CloudHQ, Reagan) simultaneously — capacity risk
- PagerDuty: 4 consecutive weeks with no action past "channel created"
- Environment access audit: 4 weeks without owner — assign or remove from tracking

### Notes Saved by Robert
- "1 signed" from Mar 6 report — client identity not confirmed; clarify with Robert
- Loblaw shutdown context: public data breach Mar 7-8 weekend
- Elizabeth Escudo covering ProServe during Juliana vacation week of Mar 16

---

## Week of Mar 2–6, 2026
Generated: 2026-03-06

### OKR Pulse
- Obj 1 (Kaji Adoption): All ProServe teams on Kaji. v1 playbook created. Support team onboarding is next. Utilization baseline in progress — engineer-level history tracking still needed.
- Obj 2 (Agentic FDE): Playbook v1 complete and in active use. Demos delivered: Campbell, EJ Gallo. Demos booked: Dynex, CentralReach, CloudHQ, Hitachi. Reagan rescheduled.
- Obj 3 (Strategic Relationships): Whitecap done. Dynex COO done. Hitachi unblocked. Annexus IT access done. MCP prod in progress. Loblaw Health (Andrew) engaged.
- Obj 4 (Revenue Growth): 1 net-new signed (client TBD — Robert to confirm). Strategic Pharmacy in proposal revision. CentralReach new health project interest.
- Obj 5 (Enterprise Support): PagerDuty channel created. Platform + Stack aligned. Routing map not yet built. Access audit still not started (3rd week carried).

### Key Movements
- Agentic FDE playbook v1 completed — in active client use
- Campbell team demo delivered; Larry (exec) outreach still needed
- EJ Gallo Kaji demo completed
- Dynex COO meeting done; Kaji demo booked this week
- Hitachi Opus fix resolved; Veronica aligned; demo scheduled; use case design underway
- Annexus IT access unblocked (Taylor); starting on SOW
- Whitecap check-in completed by Andres
- CentralReach new ProServe health project interest surfaced
- MCP (Ian) production deployment alignment in progress
- Loblaw Health (Andrew) outreach successful — engaged
- PagerDuty cross-team channel created; Platform + Stack teams joined

### Open Carry-Forwards
- [ ] Robert reaches Larry at Campbell — exec strategic alignment post-demo — Owner: Robert
- [ ] PagerDuty: Build full routing map and responsibilities across all teams — Owner: Platform/Stack leads
- [ ] Support team onboarding to Kaji — Owner: Robert
- [ ] Utilization baseline — add engineer-level history tracking — Owner: Robert/Alok
- [ ] Strategic Pharmacy — close proposal revisions — Owner: Robert
- [ ] CentralReach health project — scope and price — Owner: Robert
- [ ] Execute demos: Dynex, CentralReach, CloudHQ — Owner: Robert/Juliana
- [ ] Reagan Foundation — rebook meeting — Owner: Juliana
- [ ] Agentic FDE blog post + internal promotion — Owner: Robert
- [ ] Train all EMs on Agentic FDE playbook — Owner: Robert
- [ ] Environment access audit per team per client — Owner: TBD (carried 3 weeks)
- [ ] MCP production deployment support — Owner: Robert/team

### Patterns Flagged
- Environment access audit: carried 3 consecutive weeks — no owner, no action
- Demo momentum strong but conversion to SOW still lagging — 5 demos active, 1 signed
- Q1 revenue: 1 of 3 net-new signed — end of Q1 approaching, 2 still needed
- Reagan Foundation: meeting pattern of rescheduling — may need different escalation approach

### Notes Saved by Robert
- Kaji session broke during Mar 6 report run — Robert wrote the report manually
- "1 signed" in Obj 4 — client not specified in report; Robert to confirm which client
- Annexus SOW process now starting after IT access resolved

---

## Week of Feb 23–27, 2026
Generated: 2026-02-27

### OKR Pulse
- Obj 1 (Kaji Adoption): Adoption proven by outage impact — no baseline metric yet to score 2x target
- Obj 2 (Agentic FDE): Template exists (SkiOS/MCP), multiple clients operating in Agentic FDE mode informally — needs packaging
- Obj 3 (Strategic Relationships): 61% sync coverage (11/18) — below 70% target; dark accounts: Campbell, Hitachi, Whitecap, Dynex
- Obj 4 (Revenue Growth): 0 of 3 net-new clients signed; Strategic Pharmacy closest ($505K); QuadReal ($300K) under CIC review
- Obj 5 (Enterprise Support): Routing working organically but informally; PagerDuty not implemented; access gaps across all teams

### Key Movements
- Robert delivered SkiOS architecture + Gamma presentation for MCP (Agentic FDE template)
- New Annexus SOW sent ($27K–$82K), anchored to Agentic FDE model — blocked on IT access
- FlexiVan onboarded on Kaji
- EJ Gallo SOW sent (Expense Express)
- ICI ETF kicking off March 2
- Strategic Pharmacy SOW confirmed ($505K) — awaiting kick-off timing
- QuadReal ($300K) entering March CIC review — 4 vendors competing
- Dynex extended 3 months post-churn
- Reagan cert expiry: 3rd occurrence in 4 months (pattern)

### Open Carry-Forwards
- [ ] Robert reaches Larry at Campbell — exec alignment conversation — Owner: Robert
- [ ] Andres proactive Whitecap check-in before renewal window — Owner: Andres
- [ ] Dynex COO meeting support — Kaji value narrative — Owner: Ethan/Yevgeniy (Robert support)
- [ ] Hitachi use case session with Sumit + Michael — blocked on Opus fix — Owner: Robert
- [ ] Annexus IT access unblock through Taylor — Owner: Robert/Taylor
- [ ] Strategic Pharmacy kick-off date confirmation — Owner: Robert
- [ ] PagerDuty implementation kickoff — Owner: TBD
- [ ] Environment access audit per team per client — Owner: TBD
- [ ] Agentic FDE methodology doc (using MCP as reference) — Owner: Robert
- [ ] Utilization baseline definition (hours saved, tickets deflected, tasks automated) — Owner: Robert/Alok

### Patterns Flagged
- Reagan cert expiry: 3 occurrences in 4 months — cert monitoring needed across ALL clients
- Agentic FDE being delivered without the formal label at Loblaw, CentralReach, BWX — needs naming/packaging
- Q1 revenue target (3 net-new at $18K/mo) at risk — 0 signed with limited weeks remaining
- Dark account cluster (Campbell, Hitachi, Whitecap, Dynex) all require proactive outreach simultaneously

### Notes Saved by Robert
- OKRs Notion page: https://www.notion.so/shakudo/ProServe-OKRs-29ad36b5509080729d23c795109da13f
- Agentic FDE methodology doc to be built from SkiOS/MCP as template
- Alok owns sprint coordination going forward

