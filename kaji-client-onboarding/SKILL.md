---
name: kaji-client-onboarding
description: "Draft and send client onboarding questionnaires for Kaji AI Agent deployments. Generates personalized emails or documents asking clients about model preferences, use cases, integrations, communication, and security requirements. When responses come back, processes them into a must-have/nice-to-have deployment timeline and creates a ClickUp task to track the rollout."
license: MIT
compatibility: opencode
metadata:
  author: robert-shakudo
  version: "2.1"
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

### Step 2: Choose Format and Generate the Outreach

Two options — pick based on what the team member requests (default to Email if no preference):

| Format | When to use | What to send |
|--------|-------------|--------------|
| **Email** | Quick intake, technical clients, short turnaround | Short personalized intro + inline questionnaire from template |
| **Document** | Comprehensive review, formal clients, discovery phase | Capabilities Overview doc (shows full platform, client selects add-ons) |

**Key rules:**
- Never expose internal tool names (no "Dremio MCP", no "ast_grep_search", no "NanoBanana")
- Communication channels other than Mattermost must be marked as **Beta**
- The client's selections drive what additional tools we install

#### If Email Format

Draft a short professional intro and embed the questionnaire from [assets/email-questionnaire-template.md](./assets/email-questionnaire-template.md).

Personalize:
1. Subject line — include company name
2. Opening paragraph — reference what you know about their needs (sales calls, ClickUp notes, Mattermost history)
3. Closing — reference next steps or upcoming meeting

Do not send the template verbatim. The template is a starting point.

#### If Document Format

Use the **Capabilities Overview** (`assets/client-capabilities-overview.md`) as the primary document. This is the polished client-facing doc that shows:
- What comes standard in every deployment
- Add-on tools available
- Use case checklist
- Custom integration request form

Personalize the header with the client's company name and contact. Save as a file and attach to the email/Mattermost message.

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
| Basic Integrations | Always included (Dremio, Mattermost, MarkItDown, NanoBanana, ClickUp, Shakudo Platform) — 0 extra days | — |
| Add-On Integrations | Add-ons they selected | Items marked "eventually" or "maybe" |
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

- [Client Capabilities Overview](./assets/client-capabilities-overview.md) — Primary client-facing document. Lists standard capabilities and add-ons in business language. Client selects use cases and add-ons, which drives what we deploy. **This is the main doc we send.**
- [Timeline Estimation Matrix](./references/timeline-matrix.md) — Day estimates per requirement type (internal)
- [Client Onboarding Questionnaire](./assets/client-onboarding-questionnaire.md) — Detailed intake form (use if client needs more granular questions beyond the capabilities doc)
- [Client Deployment Summary](./assets/client-deployment-summary.md) — Deliverable sent back to client after processing responses
- [Internal Summary Template](./assets/onboarding-summary-template.md) — Internal tracking template


