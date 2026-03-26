# Component Selection Rubric

Use this rubric to choose the best Shakudo component stack for a POC.

The goal is to make an explicit architecture decision, not to throw every available component into the build.

---

## Step 1: Define the shape of the problem

Answer these questions first:

### Interaction model
- Is the user mostly **asking**, **reviewing**, **approving**, **investigating**, or **monitoring**?
- Is the best interface **chat**, **web UI**, **workflow trigger**, or **hybrid**?

### Data shape
- Is the evidence mostly **structured data**, **documents**, **events**, **messages**, or **mixed**?

### Action model
- Is the app **read-only**, **human-in-the-loop**, **workflow-driven**, or **autonomous**?

### Constraints
- Does the client require strict security or special infra?
- Is the POC timeline tight enough that mocks are preferred?
- Do we need a visible UI for the room?
- Does the user need approval checkpoints or side effects in external systems?

---

## Step 2: Choose the primary interface

| If the user mainly... | Best default |
|---|---|
| asks questions or requests actions in language | Kaji chat |
| reviews records, cards, dashboards, or forms | Web UI |
| kicks off workflows and waits for outcomes | Kaji or UI + n8n |
| explores structured data and metrics | Kaji analytics copilot or analytics UI |
| investigates documents, cases, or knowledge artifacts | Kaji + RAG |

**Rule:** choose one primary interface first. Add a second only if it materially improves the POC story.

---

## Step 3: Choose the core intelligence / execution layer

| Need | Best default |
|---|---|
| natural-language reasoning | Kaji |
| custom domain logic / state / APIs | Shakudo microservice |
| workflow orchestration / approvals | n8n |
| SQL analytics | Dremio |
| document-grounded answers | RAG |
| relationship-aware memory | Graph memory / Neo4j |
| traceability for LLM behavior | Langfuse / Arize Phoenix |
| analyst experimentation | Notebook / JupyterHub |

---

## Step 4: Compare candidate patterns with a scorecard

Score each candidate architecture with **High / Medium / Low** on the dimensions below.

| Dimension | Question |
|---|---|
| Credibility | Will this feel real and convincing in the client meeting? |
| Simplicity | Can we build it quickly without unnecessary moving parts? |
| Reuse | Can we reuse an existing demo or known pattern? |
| Mockability | Can we fake missing systems without damaging the story? |
| Extensibility | Can this evolve into a go-live implementation later? |
| Fit | Does it match the user's actual workflow? |

Candidate patterns to compare:
- chat-native assistant
- UI-first app + API
- workflow-first automation
- analytics copilot
- knowledge assistant
- multi-agent system
- hybrid

### Scorecard template

| Option | Credibility | Simplicity | Reuse | Mockability | Extensibility | Fit | Notes |
|---|---|---|---|---|---|---|---|
| Option A | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | |
| Option B | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | |
| Option C | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | H/M/L | |

Use the scorecard to support the decision, not to replace judgment.

---

## Step 5: Start from the best default stack, then customize

| App pattern | Best default stack | Usually mock first | Avoid by default |
|---|---|---|---|
| Chat-first operational assistant | Kaji + MCP skills + microservice | downstream write actions | full custom UI |
| UI-first operational app | Web UI + microservice + optional Kaji sidecar | external approvals / notifications | deep multi-agent logic |
| Analytics copilot | Kaji + Dremio + optional microservice | live warehouse credentials | RAG-heavy stack |
| Knowledge assistant | Kaji + RAG + optional microservice | private document corpus | SQL-heavy backend |
| Approval / workflow app | Kaji or UI + microservice + n8n | final external side effects | putting all logic in n8n |
| Multi-agent system | Kaji + clear role boundaries + microservice + observability | non-essential agent roles | using multi-agent just for style |

---

## Step 6: Apply best-practice decision rules

### Choose Kaji when
- the core value is conversational or investigative
- the user wants synthesis across sources
- actions are triggered from natural language

### Choose a microservice when
- business logic should live in code
- you need custom endpoints, state, or a UI backend
- the POC needs a stable API layer

### Choose n8n when
- the key flow is event-driven or approval-based
- multiple systems need to be orchestrated visibly
- a human has to approve or intervene between system actions

### Choose Dremio when
- the source of truth is structured data
- NL-to-SQL or analytics is central

### Choose RAG when
- the source of truth is documents or policies
- retrieval quality matters more than table joins

### Choose graph memory when
- entity relationships are central to the value
- memory needs to persist beyond a single interaction

### Choose multi-agent only when
- there are truly distinct responsibilities
- a single agent would be overloaded or hard to explain

---

## Step 7: Simplify for the POC

Before finalizing the stack, ask:
- Can one surface be removed?
- Can one integration be mocked?
- Can one component be deferred to Year 2?
- Is there a simpler existing demo pattern that tells the same story?
- Can the first release succeed with one backend service instead of several?

Required output:
- **build now**
- **mock now**
- **defer later**

---

## Step 8: Produce the final recommendation

Every Full Spec should include a summary like this:

```md
## Recommended Build Strategy
- App type:
- Primary interaction model:
- Recommended architecture:
- Best component stack:
- Closest existing demo to reuse:
- Components to build now:
- Components to mock now:
- Components to defer to Year 2:
- Why this is the best POC choice:
```

And a rationale table like this:

| Need / Function | Recommended Component | Why This Component Fits | Build Now or Later |
|---|---|---|---|
| Natural-language interface | Kaji | The user interacts conversationally and needs synthesis across tools. | Now |
| Business logic / APIs | Microservice | Domain logic should live in code and expose a stable backend. | Now |
| Approval workflow | n8n | Human approvals happen between automated steps and are easy to mock. | Later or Mock |

---

## Red flags

Reconsider the architecture if you see any of these:
- both a rich custom UI and a rich chat assistant are in scope with no clear reason
- multi-agent was chosen but the roles are vague
- RAG was chosen but the real value is in structured data
- n8n is being used as the main place for business logic
- the proposed stack is much larger than the value being proven
- the team cannot explain in one sentence why each component is present
