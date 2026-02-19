---
name: kaji-client-onboarding
description: "Draft and send client onboarding questionnaires for Kaji AI Agent deployments. Generates personalized emails or documents asking clients about model preferences, use cases, integrations, communication, and security requirements. When responses come back, processes them into a must-have/nice-to-have deployment timeline and creates a ClickUp task to track the rollout."
license: MIT
metadata:
  author: robert-shakudo
  version: "2.0"
  category: onboarding
---

# Kaji Client Onboarding

Helps the Shakudo team draft onboarding questionnaires (email or document) to send to clients, then processes client responses into a deployment plan with timeline estimates and a ClickUp tracking task.

## When to Activate

- Team member says "send onboarding to [client]", "draft onboarding email for [client]", or "prepare onboarding doc"
- Team member says "process onboarding responses from [client]" or provides client answers
- Pre-sales needs a deployment timeline estimate for a prospect
- Any mention of client onboarding, intake, or deployment scoping for Kaji

## Core Concepts

This skill has **two phases**:

1. **Phase 1 — Draft & Send**: Generate a personalized onboarding email or document for the client. The team member reviews, optionally edits, then sends it.
2. **Phase 2 — Process & Plan**: When client responses come back, classify requirements as must-have/nice-to-have, calculate a deployment timeline, and create a ClickUp task with subtasks.

## Phase 1: Draft the Onboarding Email or Document

### Step 1: Gather Context

Ask the team member (not the client) for basic context using the `question` tool:

- Client/company name
- Primary contact name and email
- What do we already know about their needs? (any context from sales calls, notes, etc.)
- Preferred format: Email or Document?

### Step 2: Generate the Outreach

Using the context provided, draft a **personalized** onboarding email or document.

#### If Email Format

Draft a professional email from the Shakudo team to the client. Structure:

```
Subject: Kaji AI Agent — Getting Started with [Company Name]

Hi [Contact Name],

[1-2 sentences: personalized opening referencing what we know about their needs or recent conversations]

To get your Kaji environment configured, we need a few details from your team. This typically takes 5-10 minutes to complete.

---

**1. Model Preferences**

We support three AI model providers. Please rank your preference (1 = primary, 3 = backup):

- [ ] Claude Opus 4.5+ (Anthropic) — Optimized for complex reasoning and code generation
- [ ] GPT 5.2+ (OpenAI) — Advanced language understanding and problem-solving
- [ ] Gemini Pro 3 (Google) — Multimodal capabilities and efficiency

Preference ranking: ___

Are you interested in running locally-hosted models?  [ ] Yes  [ ] No  [ ] Let's discuss

Performance priority:  [ ] Speed  [ ] Quality  [ ] Balance

**2. API Key Setup**

- [ ] We'll provide our own API keys
- [ ] We'd like Shakudo to provide and manage keys
- [ ] Both — own keys for dev, managed keys for production

**3. Use Cases**

What will your team use Kaji for? (check all that apply)

- [ ] Code generation and scaffolding
- [ ] Code review and refactoring
- [ ] Debugging and troubleshooting
- [ ] Architecture consultation
- [ ] Documentation generation
- [ ] Test writing and QA
- [ ] DevOps and CI/CD automation
- [ ] Data analysis and insights
- [ ] Research and information gathering
- [ ] Project management and planning
- [ ] Other: ___

Primary use case (brief description): ___

**4. Platform Integrations**

Kaji connects to the tools you already use. Select what's relevant:

Development:
- [ ] Browser automation and testing (Playwright)
- [ ] Frontend UI/UX design-to-code
- [ ] Advanced Git operations
- [ ] Interactive browser sessions

Data:
- [ ] PostgreSQL / Supabase
- [ ] Dremio data lakehouse
- [ ] Neo4j graph database

Knowledge:
- [ ] Persistent memory across sessions
- [ ] Library documentation lookup

Communication:
- [ ] Mattermost
- [ ] Fireflies (meeting transcripts)

Project Management:
- [ ] ClickUp
- [ ] Notion

Search:
- [ ] Web search and research
- [ ] Code search across repositories

Infrastructure:
- [ ] Shakudo Platform management

Content:
- [ ] Presentation generation (Gamma)
- [ ] Image generation

Files:
- [ ] Document format conversion

**5. Additional Integrations**

Are there tools not listed above you'd like Kaji to connect to?
___

**6. Communication Preference**

How would your team prefer to interact with Kaji?

- [ ] Mattermost (available now)
- [ ] Slack
- [ ] Microsoft Teams
- [ ] CLI / Terminal
- [ ] Other: ___

**7. Security Requirements**

What guardrails does your team need?

- [ ] Code change approval required before execution
- [ ] Git operation approval (commits, pushes, merges)
- [ ] Restricted file/directory access
- [ ] No production environment access
- [ ] Full audit logging of all actions
- [ ] Read-only mode (analyze only, no modifications)
- [ ] Custom compliance rules: ___

Specific paths or environments to restrict: ___

---

Once we receive your responses, we'll have a deployment plan with timeline ready within 1 business day.

[Personalized closing referencing next steps or upcoming meeting]

Best,
[Sender name]
Shakudo Team
```

#### If Document Format

Use the client-facing questionnaire template from `assets/client-onboarding-questionnaire.md` as the base. Personalize the header with the client's company name and contact info. Save as a file and attach.

### Step 3: Review with Team Member

Present the draft to the team member. Ask:
- "Here's the onboarding email for [Client]. Want me to adjust anything before sending?"
- If they approve, offer to send via Mattermost DM, or save as a file they can email manually.
- If they want changes, edit and re-present.

### Step 4: Send or Save

- **If sending via Mattermost**: Post the questionnaire as a message or file attachment
- **If email**: Save the draft to a file so the team member can copy/paste into their email client
- **If document**: Save to a file and offer to attach to a Mattermost message

---

## Phase 2: Process Client Responses

When the team member provides client responses (pasted text, forwarded message, or summarized answers):

### Step 1: Parse Responses

Extract answers for all 7 sections. If any section is missing, flag it and ask the team member whether to follow up with the client or use a default.

### Step 2: Classify Must-Haves vs Nice-to-Haves

Apply these classification rules:

| Category | Must-Have | Nice-to-Have |
|----------|-----------|--------------|
| Models | At least 1 MaaS model selected | Local model support |
| API Keys | Key decision (own/company/both) | — |
| Use Cases | Primary use case(s) | Secondary "would be nice" items |
| Integrations | Integrations they selected | Items marked "eventually" or "maybe" |
| New Integrations | — | All (require development) |
| Communication | Mattermost or CLI (available now) | Slack, Teams, other (require work) |
| Security | Basic (git approval, no prod access) | Advanced (audit logging, custom rules, read-only) |

Present the classification to the team member for confirmation before proceeding.

### Step 3: Calculate Timeline

Use the scoring matrix in [references/timeline-matrix.md](./references/timeline-matrix.md).

1. **Start with base**: 1 day for standard single-model MaaS setup
2. **Add per item**: See timeline matrix for exact values per category
3. **Sum must-have days** = Go-live timeline
4. **Sum nice-to-have days** = Post-launch additions
5. **Apply 20% buffer** (round up to nearest whole day)

### Step 4: Generate Deployment Summary

Present to the team member:

```
## Kaji Deployment Summary — [Client/Company Name]
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

### Step 5: Create ClickUp Deployment Task

After the team member confirms the summary, create a ClickUp task:

- **List**: Client's list under `Customers` folder if it exists, otherwise `Agent Skills`
- **Name**: `Kaji Deployment — [Client/Company Name]`
- **Priority**: 2 (High)
- **Subtasks**: One for every must-have item + "Testing & validation" + "Go-live"

Nice-to-have items go in the parent task description only.

**Task description** should include: deployment overview, must-haves table, nice-to-haves table, configuration summary, and phased timeline.

### Step 6: Share Results

1. Share the ClickUp task URL with the team member
2. Offer to send the deployment summary to the client (via Mattermost or as a saved document using the `assets/client-deployment-summary.md` template)
3. Offer to post the task link in a relevant Mattermost channel

---

## Practical Guidance

### Personalization
- Always reference what we know about the client (from sales calls, existing ClickUp tasks, Mattermost history)
- Don't send a generic template — tailor the opening and closing to the relationship
- If we know their industry or tech stack, mention relevant integrations first

### Tone
- Professional but warm — this is client-facing
- Keep the questionnaire concise — busy people won't fill out a 10-page form
- The email should take 5-10 minutes to respond to, max

### Common Patterns
- Most developers need: 1 MaaS model + Git + 1-2 integrations = **2-3 day deployment**
- Teams needing Slack/Teams add **3-5 days**
- Custom security guardrails add **2-4 days**
- New integration requests add **5-10 days** each

### Anti-Patterns
- Don't send a raw template without personalizing it
- Don't process incomplete responses without flagging missing sections
- Don't create the ClickUp task before the team member confirms the classification

## Guidelines

1. Always personalize the outreach — never send a generic template
2. Always present the draft to the team member for review before sending
3. When processing responses, confirm must-have/nice-to-have classification with the team member
4. Include the 20% buffer in all timeline estimates
5. Always create the ClickUp deployment task with subtasks after confirmation
6. Save the deployment summary to a file for the client record

## Integration

- ClickUp — Create deployment task with subtasks for every must-have item
- mattermost-notify — Send the questionnaire or deployment summary via DM
- project-memory — Record onboarding decisions for future reference

## References

- [Timeline Estimation Matrix](./references/timeline-matrix.md) — Day estimates per requirement type
- [Client Onboarding Questionnaire](./assets/client-onboarding-questionnaire.md) — Branded document template
- [Client Deployment Summary](./assets/client-deployment-summary.md) — Deliverable sent back to client
- [Internal Summary Template](./assets/onboarding-summary-template.md) — Internal tracking template

---

## Skill Metadata

**Created**: 2026-02-19
**Last Updated**: 2026-02-19
**Author**: Robert @ Shakudo
**Version**: 2.0
