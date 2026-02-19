---
name: kaji-client-onboarding
description: "Run the Kaji AI Agent onboarding intake for new clients or developers. Walks through a structured questionnaire covering model preferences, API keys, use cases, integrations, communication, and security — then generates a deployment timeline estimate with must-haves vs nice-to-haves."
license: MIT
metadata:
  author: robert-shakudo
  version: "1.0"
  category: onboarding
---

# Kaji Client Onboarding

Conversational onboarding intake for new Kaji AI Agent deployments. Guides the client through structured questions, categorizes requirements into must-haves and nice-to-haves, and produces a deployment timeline estimate.

## When to Activate

- New client or developer is being onboarded onto Kaji
- User says "onboard", "new client setup", "deployment intake", or "how long will Kaji take to deploy"
- Manager asks to run onboarding for a team or individual
- Pre-sales needs a timeline estimate for a prospect

## Core Concepts

This skill runs a **7-section intake questionnaire** interactively using the `question` tool. After collecting all answers, it:

1. Categorizes each requirement as **Must-Have** (blocks deployment) or **Nice-to-Have** (can be added post-launch)
2. Calculates a deployment timeline using the scoring matrix in `references/timeline-matrix.md`
3. Produces a structured summary the team can use for scoping and scheduling

## Onboarding Workflow

### Before Starting

Greet the client briefly. Explain you'll walk them through 7 short sections and give them a timeline estimate at the end. Keep it conversational — not robotic.

### Section 1: Model Requirements

Ask which models they need using the `question` tool.

**MaaS models (currently supported):**
- Claude Opus 4.5+
- GPT 5.2+
- Gemini Pro 3

Ask them to rank their preference 1-3.

**Local models:**
- Ask if they plan to run local models
- If yes, which model(s) are they testing
- Performance preference: Speed vs Quality vs Balance

**Classification:**
- At least 1 MaaS model selected = Must-Have (required for deployment)
- Local model support = Nice-to-Have (can be added later unless it's their only option)

### Section 2: API Key Configuration

Ask whether they will:
- Use their own API key
- Need a company-provided key
- Both (own for dev, company for prod)

**Classification:**
- API key decision = Must-Have (blocks model access)

### Section 3: Use Cases

Ask what they'll use Kaji for. Present these options (multi-select):
- Code generation & implementation
- Code review & refactoring
- Debugging & troubleshooting
- Architecture & design consultation
- Documentation generation
- Test writing
- DevOps / CI/CD automation
- Data analysis & querying
- Research & exploration
- Project management & task tracking

Then ask them to describe their primary use case(s) in a sentence or two.

**Classification:**
- Primary use case(s) = Must-Have
- Secondary / "would be nice" use cases = Nice-to-Have

### Section 4: Integrations

Present the full list of currently supported integrations. Ask them to select all they need.

**Built-in Skills:**
- Playwright (browser automation, testing, screenshots)
- Frontend UI/UX (design-to-code, styling)
- Git Master (commits, rebase, squash, blame, history)
- Dev Browser (persistent browser sessions, web workflows)

**Data & Analytics:**
- Supabase (PostgreSQL queries)
- Dremio (data lakehouse)
- Neo4j (graph database)

**Knowledge & Memory:**
- Graphiti Memory (persistent graph memory)
- Context7 (library docs lookup)

**Communication:**
- Mattermost (messaging, threads, files)
- Fireflies (meeting transcripts, summaries)

**Project Management:**
- ClickUp (tasks, docs, time tracking)
- Notion (pages, databases, documents)

**Search & Web:**
- Tavily Search (web search, crawling)
- Exa Web Search (clean web content)
- Grep.app (GitHub code search)

**Infrastructure:**
- Shakudo Platform (microservices, pipelines, environments)

**Content Creation:**
- Gamma (presentations, documents)
- NanoBanana (AI image generation)

**File Processing:**
- MarkItDown (file/URL to markdown conversion)

**Classification:**
- Integrations they select = Must-Have
- Integrations they mention as "eventually" or "maybe" = Nice-to-Have

### Section 5: Requested New Integrations

Ask if there are integrations NOT on the list above that they'd like added.

**Classification:**
- All requested new integrations = Nice-to-Have (require development)

### Section 6: Communication Preference

Ask how they want to interact with Kaji:
- Mattermost (currently supported)
- Slack
- Microsoft Teams
- CLI / Terminal
- Other

**Classification:**
- Mattermost or CLI = Must-Have (available now, zero setup)
- Slack or Teams = Nice-to-Have (requires integration work)
- Other = Nice-to-Have

### Section 7: Security & Guardrails

Ask what security controls they need:
- Read-only mode (analyze but not modify code)
- Approval-required for all file changes
- Approval-required for git operations only
- Restricted file/directory access
- No access to production environments
- Audit logging of all agent actions
- Custom guardrails

If they select restricted access, ask which paths/environments.

**Classification:**
- Basic guardrails (approval for git, no prod access) = Must-Have
- Advanced guardrails (audit logging, custom rules, read-only mode) = Nice-to-Have

## Generating the Timeline

After all 7 sections, calculate the deployment timeline using the matrix in [references/timeline-matrix.md](./references/timeline-matrix.md).

### Calculation Steps

1. **Start with base**: 1 day for standard single-model MaaS setup
2. **Add per item**: See timeline matrix for exact values per category
3. **Sum must-have days** = Minimum deployment timeline
4. **Sum nice-to-have days** = Additional timeline for full setup
5. **Total** = Must-Have timeline + Nice-to-Have timeline

### Output Format

After calculation, present the summary using this structure:

```
## Kaji Deployment Summary — [Client/Developer Name]
Date: [today]

### Must-Haves (Required for Go-Live)
| # | Requirement | Category | Est. Days |
|---|-------------|----------|-----------|
| 1 | [item]      | [cat]    | [days]    |
...
**Must-Have Subtotal: X days**

### Nice-to-Haves (Post-Launch Enhancements)
| # | Requirement | Category | Est. Days |
|---|-------------|----------|-----------|
| 1 | [item]      | [cat]    | [days]    |
...
**Nice-to-Have Subtotal: X days**

### Timeline Estimate
- Go-Live (Must-Haves only): **X business days**
- Full Setup (Everything): **Y business days**
- Recommended buffer: +20%

### Notes
[Any special considerations, dependencies, or risks]
```

## Create ClickUp Deployment Task

After presenting the summary and getting client confirmation, **automatically create a ClickUp task** to track the deployment.

### Task Creation

Use the `create_task` tool with these parameters:

- **List**: Use the client's list if one exists under `Kaji` folder, otherwise create in `Agent Skills`
- **Name**: `Kaji Deployment — [Client/Company Name]`
- **Priority**: 2 (High)
- **Description**: Populate with the full deployment summary (must-haves, nice-to-haves, timeline, config)

### Subtasks

Create a subtask for **every must-have item** so the team can track progress:

```
Parent: "Kaji Deployment — Acme Corp"
├── Subtask: "Configure Claude Opus 4.5+ model access"          (Must-Have)
├── Subtask: "Provision company API keys"                        (Must-Have)
├── Subtask: "Set up Supabase integration"                       (Must-Have)
├── Subtask: "Configure Mattermost channel"                      (Must-Have)
├── Subtask: "Implement git operation approval guardrails"       (Must-Have)
├── Subtask: "Testing & validation period"                       (Must-Have)
└── Subtask: "Go-live"                                           (Must-Have)
```

Nice-to-have items go in the parent task description under a separate section — they don't need individual subtasks yet.

### Task Description Template

Use this markdown structure for the ClickUp task description:

```markdown
## Deployment Overview
- **Client**: [name]
- **Company**: [company]
- **Go-Live Estimate**: [X business days]
- **Full Setup Estimate**: [Y business days]
- **Primary Model**: [model]
- **Communication Channel**: [channel]

## Must-Haves (Go-Live Requirements)
| # | Requirement | Category | Est. Days |
|---|-------------|----------|-----------|
[populated from intake]

## Nice-to-Haves (Post-Launch)
| # | Requirement | Category | Est. Days |
|---|-------------|----------|-----------|
[populated from intake]

## Configuration
- **Models**: [list]
- **API Keys**: [own/company/both]
- **Integrations**: [list]
- **Security**: [list]

## Timeline
| Phase | Duration |
|-------|----------|
| Environment Setup | X days |
| Model Configuration | X days |
| Integration Setup | X days |
| Security Configuration | X days |
| Testing & Validation | X days |
| Go-Live | X days |
| **Total** | **X days** |
```

### After Task Creation

1. Share the ClickUp task URL with the client/manager
2. Confirm: "I've created a ClickUp task to track your deployment: [URL]. Your team can follow progress there."
3. If the onboarding was done via Mattermost, also post the task link in the thread

## Practical Guidance

### Conversation Style
- Keep questions concise — use the `question` tool, don't dump walls of text
- Group related follow-ups (don't ask 20 separate questions)
- If the client seems unsure about something, default it to Nice-to-Have
- Be helpful, not interrogative

### Common Patterns
- Most developers need: 1 MaaS model + Git Master + 1-2 integrations = **2-3 day deployment**
- Teams needing Slack/Teams integration add **3-5 days** for channel setup
- Custom security guardrails add **2-4 days** depending on complexity
- New integration requests (not on the list) add **5-10 days** each

### Anti-Patterns
- Don't skip sections even if the client says "just set it up"
- Don't estimate without completing all 7 sections
- Don't mark something Must-Have just because the client mentioned it casually — ask if it blocks their go-live

## Guidelines

1. Always use the `question` tool for multi-choice questions — never plain text lists
2. Complete all 7 sections before generating the timeline
3. Explicitly confirm the must-have vs nice-to-have classification with the client before presenting the final summary
4. Include the 20% buffer in timeline estimates — things always take longer
5. Always create the ClickUp deployment task with subtasks — this is not optional
6. Save the final summary to a file and offer to send it via Mattermost

## Integration

- ClickUp — Create deployment task with subtasks for every must-have item
- mattermost-notify — Send the final summary and ClickUp task link to the client or manager via DM
- project-memory — Record onboarding decisions for future reference
- shakudo-microservice — Reference when estimating infrastructure setup time

## References

- [Timeline Estimation Matrix](./references/timeline-matrix.md) — Day estimates per requirement type
- [Onboarding Summary Template](./assets/onboarding-summary-template.md) — Output template

---

## Skill Metadata

**Created**: 2026-02-19
**Last Updated**: 2026-02-19
**Author**: Robert @ Shakudo
**Version**: 1.0
