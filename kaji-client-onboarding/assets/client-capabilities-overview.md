# Shakudo | Kaji AI Agent Platform
## Capabilities Overview

Kaji is your team's AI-powered development partner. It reads, writes, and reasons about code, manages projects, searches the web, generates content, and integrates directly with your existing tools — all through natural conversation.

This document outlines what Kaji can do out of the box, what add-ons are available, and how to request custom tools for your workflow.

---

## Part 1: What Comes Standard

Every Kaji deployment includes the following functions and tools, ready on day one.

### Code & File Operations

These are Kaji's core functions — the fundamental actions it can perform on your codebase.

| Function | What It Does | Example |
|----------|-------------|---------|
| Read Files | Open and read any file in your project, with line-level precision | "Show me lines 50-100 of the auth controller" |
| Write Files | Create new files from scratch | "Create a new React component for the dashboard" |
| Edit Files | Make precise, targeted changes to existing code | "Replace the deprecated API call on line 34 with the new endpoint" |
| Search Files | Find files by name or pattern across your entire project | "Find all TypeScript files in the components directory" |
| Search Content | Find specific text or patterns inside files using regex | "Find everywhere we call the payments API" |
| Run Commands | Execute any shell command — build, test, deploy, install | "Run the test suite" / "Install the lodash package" |

### Code Intelligence

These tools understand your code's structure — not just text, but meaning.

| Tool | What It Does | Example |
|------|-------------|---------|
| Go to Definition | Jump to where any function, class, or variable is defined | "Where is the `calculateTotal` function defined?" |
| Find References | Find every place a symbol is used across the entire project | "Show me everywhere `UserService` is referenced" |
| Symbol Search | List all functions, classes, and exports in a file or project | "What functions does the billing module export?" |
| Error Detection | Surface errors, warnings, and type issues in real-time | "Are there any type errors in the files I just changed?" |
| Safe Rename | Rename a variable, function, or class everywhere it's used | "Rename `getData` to `fetchUserProfile` across the project" |
| Pattern Match | Find and replace code patterns using structure-aware matching (25+ languages) | "Replace all `console.log` calls with our logger" |

### Data & Analytics

| Tool | What It Does | Example |
|------|-------------|---------|
| SQL Querying | Run queries against your data warehouse — analysis, reporting, joins, aggregations | "Show me all orders over $10K from last quarter" |

### Project Management

| Tool | What It Does | Example |
|------|-------------|---------|
| Task Management | Create, update, assign, and track tasks with priorities, due dates, and subtasks | "Create a bug ticket for the login issue and assign it to Sarah" |
| Documents | Create and edit project docs, wikis, and knowledge bases | "Draft a technical spec for the new API" |
| Time Tracking | Start/stop timers, log time entries, review tracked hours | "Log 2 hours on the database migration task" |
| Sprints & Workflows | Organize work across sprints, folders, lists, and statuses | "Move all completed tickets to the Done column" |

### Communication

| Tool | What It Does | Availability |
|------|-------------|--------------|
| Team Messaging | Send messages, reply in threads, share files, search history | Available (Mattermost) |
| Alerts & Monitoring | Notify your team when the agent needs input or hits an issue | Available |
| Scheduled Automation | Run recurring tasks on a cron schedule | Available |
| Slack Integration | Channel and direct message integration | Beta |
| Microsoft Teams | Channel and chat integration | Beta |

### Content Creation

| Tool | What It Does | Example |
|------|-------------|---------|
| Image Generation | Create and edit images with AI — diagrams, mockups, visual assets | "Generate a system architecture diagram" |
| Document Conversion | Convert PDFs, Word docs, web pages, and other formats to clean text | "Convert this PDF to markdown" |

### Infrastructure & DevOps

| Tool | What It Does | Example |
|------|-------------|---------|
| Service Management | Deploy, scale, restart, and monitor microservices | "Scale the API service to 3 replicas" |
| Pipeline Jobs | Create and manage batch jobs and scheduled pipelines | "Run the nightly data sync pipeline" |
| Environment Config | Manage environments, resource limits, and configurations | "What environments are available for GPU workloads?" |
| Image Builds | Build and deploy container images from Dockerfiles | "Build a new image from the updated Dockerfile" |

### AI & Research

| Tool | What It Does | Example |
|------|-------------|---------|
| Multi-Agent Orchestration | Breaks complex work into parallel subtasks delegated to specialized agents | "Refactor the auth module" (spawns code, test, and review agents in parallel) |
| Web Research | Searches the web for current information, docs, and examples | "What's the latest best practice for JWT refresh tokens?" |
| Library Docs | Looks up real-time documentation for any programming library | "How does the useEffect cleanup work in React 19?" |
| Code Examples | Finds real-world code examples from public repositories | "Show me how other projects implement rate limiting in Express" |
| Memory | Maintains context across sessions — remembers decisions, preferences, and project history | Kaji recalls your coding conventions, past decisions, and project context |

---

## Part 2: Add-On Tools

These extend Kaji into additional platforms and workflows. Select what's relevant to your team — each add-on is configured during deployment.

### Additional Data Sources

| Tool | What It Does | Example |
|------|-------------|---------|
| PostgreSQL / Supabase | Direct database queries and backend operations | "Query the users table for accounts created this week" |
| Graph Database (Neo4j) | Query and manage graph-structured data and relationships | "Find all customers connected to this product category" |

### Advanced Knowledge

| Tool | What It Does | Example |
|------|-------------|---------|
| Deep Persistent Memory | Graph-based knowledge that grows over time — learns your patterns, preferences, and context across every interaction | Kaji remembers that your team prefers functional components, uses Tailwind, and follows trunk-based development |

### Additional Project Management

| Tool | What It Does | Example |
|------|-------------|---------|
| Notion | Manage pages, databases, documents, and knowledge bases | "Create a new page in the Engineering wiki for the migration plan" |

### Additional Research & Intelligence

| Tool | What It Does | Example |
|------|-------------|---------|
| Meeting Intelligence | Search and analyze meeting transcripts, summaries, and action items | "What did we decide about the API versioning in last week's meeting?" |
| Advanced Web Research | Deep multi-source research with content extraction and site crawling | "Research all pricing page changes from our top 5 competitors" |

### Additional Content

| Tool | What It Does | Example |
|------|-------------|---------|
| Presentation Generation | Create polished slide decks, documents, and social content with AI | "Create a 10-slide deck on our Q4 product roadmap" |

### Browser Automation

| Tool | What It Does | Example |
|------|-------------|---------|
| Automated Testing | Run browser-based tests, take screenshots, verify UI behavior | "Test the checkout flow and screenshot each step" |
| Web Scraping | Extract structured data from websites | "Scrape the pricing tables from these 3 competitor pages" |
| Form Automation | Fill forms, navigate workflows, interact with web apps | "Fill out the vendor onboarding form with these details" |
| Frontend Development | Turn designs into working code, build UI components, handle styling | "Build a responsive pricing page matching this mockup" |

---

## Part 3: Custom Tools & Integrations

Kaji's toolset is extensible. If your team uses tools not listed above, we can build custom integrations to connect them.

### How Custom Tools Work

Kaji uses a standard protocol (MCP — Model Context Protocol) to connect to external tools. This means virtually any tool with an API can be integrated. The process:

1. You tell us what tool you need connected
2. We build the integration (typical turnaround: 5-15 business days depending on complexity)
3. Kaji can then read from, write to, and interact with that tool through natural conversation

### Common Custom Integration Requests

| Category | Examples |
|----------|---------|
| CRM & Sales | Salesforce, HubSpot, Pipedrive |
| CI/CD & DevOps | GitHub Actions, Jenkins, CircleCI, ArgoCD |
| Monitoring & Observability | Datadog, PagerDuty, Grafana |
| Cloud Providers | AWS, GCP, Azure management APIs |
| Internal APIs | Your own microservices, databases, or business logic |
| Productivity | Google Workspace, Jira, Linear, Confluence |
| Communication | Discord, email (SMTP/IMAP), SMS |
| Security | Vault, SIEM systems, compliance tools |

### Request Your Custom Tools

**Tool 1:**
- Name: ___
- What should Kaji be able to do with it? ___

**Tool 2:**
- Name: ___
- What should Kaji be able to do with it? ___

**Tool 3:**
- Name: ___
- What should Kaji be able to do with it? ___

**Other tools or APIs you'd like connected:**

___

---

## Part 4: Your Use Cases

Tell us what you want Kaji to help with. Be as specific as possible — this directly shapes your deployment.

### Use Case Checklist

Check all that apply:

- [ ] Code generation and scaffolding
- [ ] Code review and refactoring
- [ ] Debugging and troubleshooting
- [ ] Architecture and design consultation
- [ ] Documentation generation
- [ ] Test writing and quality assurance
- [ ] DevOps and CI/CD automation
- [ ] Data analysis and reporting
- [ ] Research and information gathering
- [ ] Project management and planning
- [ ] Pre-sales and technical scoping
- [ ] Content creation (presentations, images, documents)
- [ ] Meeting follow-up and action items
- [ ] Client communication automation
- [ ] Other: ___

### Describe Your Primary Use Cases

For each use case, describe what a typical workflow looks like. The more detail you provide, the better we can configure Kaji.

**Use Case 1:**
- What does your team do today? ___
- What would you like Kaji to handle? ___
- How often does this happen? (daily / weekly / ad-hoc) ___

**Use Case 2:**
- What does your team do today? ___
- What would you like Kaji to handle? ___
- How often does this happen? ___

**Use Case 3:**
- What does your team do today? ___
- What would you like Kaji to handle? ___
- How often does this happen? ___

### Which Add-On Tools Support Your Use Cases?

Based on what you described above, check any add-ons you need:

- [ ] PostgreSQL / Supabase
- [ ] Graph Database (Neo4j)
- [ ] Deep Persistent Memory
- [ ] Notion
- [ ] Meeting Intelligence (Fireflies)
- [ ] Advanced Web Research
- [ ] Presentation Generation
- [ ] Browser Automation (testing, scraping, forms)
- [ ] Frontend Development
- [ ] Slack (Beta)
- [ ] Microsoft Teams (Beta)

---

## What Happens Next

1. You return this document with your selections and use case descriptions
2. We classify your requirements into **go-live essentials** and **post-launch enhancements**
3. You receive a deployment plan with timeline within **1 business day**
4. We provision your environment and begin setup

**Questions? Contact your Shakudo account team or reach out on Mattermost.**
