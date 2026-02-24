# Kaji Onboarding — Email Questionnaire Template

Use this template when the team member requests an **email format** for client onboarding.

Personalize the subject line, opening paragraph, and closing before sending. Never send this as-is.

---

```
Subject: Kaji AI Agent — Getting Started with [Company Name]

Hi [Contact Name],

[1-2 sentences: personalized opening referencing what you know about their needs or recent conversations]

To get your Kaji environment configured, we need a few details from your team. This typically takes 5-10 minutes to complete.

---

**1. Model Preferences**

We support three AI model providers. Please rank your preference (1 = primary, 3 = backup):

- [ ] Claude (Anthropic) — Strong reasoning, code generation, and long-context tasks
- [ ] GPT (OpenAI) — Broad language understanding and problem-solving
- [ ] Gemini (Google) — Multimodal capabilities and efficiency

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

Every Kaji deployment includes our standard integration package at no extra setup:

**Included by Default:**
- Data lakehouse and analytics
- Team messaging and collaboration (Mattermost)
- Document format conversion
- AI image generation and editing
- Task tracking and workflow management
- Infrastructure and environment management

**Add-On Integrations** — select any additional integrations your team needs:

Development:
- [ ] Browser automation and testing
- [ ] Frontend UI/UX design-to-code
- [ ] Advanced Git operations
- [ ] Interactive browser sessions

Data:
- [ ] PostgreSQL / Supabase
- [ ] Neo4j graph database

Knowledge:
- [ ] Persistent memory across sessions
- [ ] Library documentation lookup

Communication:
- [ ] Meeting transcripts and summaries (Fireflies)

Project Management:
- [ ] Notion (pages, databases, documents)

Search:
- [ ] Web search and research
- [ ] Specialized information retrieval
- [ ] Code search across repositories

Content:
- [ ] Presentation generation

**5. Additional Integrations**

Are there tools not listed above you'd like Kaji to connect to?
___

**6. Communication Preference**

How would your team prefer to interact with Kaji?

- [ ] Mattermost (available now)
- [ ] Slack (Beta)
- [ ] Microsoft Teams (Beta)
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
