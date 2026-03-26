# POC Registry

Canonical deployment definitions for the built-in POC library.

Use this file with `kaji-poc-build-deploy` and `shakudo-microservice-lite` to recreate services safely and consistently.

---

## Global defaults

- **Git server**: resolve from repo remote; the demos monorepo is typically `demos`
- **Repo mount in container**: `/tmp/git/monorepo`
- **Branch**: `main`
- **Pipeline type**: `BASH`
- **Port**: `8787` unless explicitly overridden
- **Preferred path pairing for current demos**:
  - `workingDir: /tmp/git/monorepo/`
  - `pipelineYamlPath: <demo-folder>/.../run.sh`
- **Deploy order**:
  1. API / backend
  2. UI / frontend
  3. workers or webhooks

### Lite lifecycle rules

- **Create / inspect / delete** → `shakudo-microservice-lite`
- **Start after delete** → recreate from this registry
- **Stop in lite-only mode** → delete after explicit confirmation
- **Restart in lite-only mode** → delete + recreate after explicit confirmation
- **Scale-to-zero / preserve config** → only if the full `shakudo-microservice` skill is available

### Parameter resolution order

Resolve runtime parameters in this order:
1. user-provided real secret
2. mounted credential already available in the environment
3. registry mock default
4. intentionally blank value only if the app supports it

### URL derivation

- **Public URL**: `https://<subdomain>.dev.hyperplane.dev`
- **Internal URL**: `http://hyperplane-service-<job-prefix>.hyperplane-pipelines.svc.cluster.local:<port>`

### Lite recreate recipe

When full lifecycle controls are unavailable:
1. search for the exact service ID
2. confirm destructive delete with the user
3. delete the old service
4. recreate from this registry definition
5. poll `pipelineJobs`
6. rerun smoke tests before reporting success

### Standard smoke tests

For every service, verify at least one of:
- `/health`
- `/`
- a key business endpoint from the POC

## Registry field reference

Each service definition should provide:
- `jobName`
- `subdomain`
- `jobType`
- `gitServerName`
- `branchName`
- `workingDir`
- `pipelineYamlPath`
- `port`
- `parameters`
- `deployOrder`
- `lifecycleMode`
- `parameters`
- `smokeTests`
- `recreateNotes`

---

## HR Resume Processor

**ClickUp Spec:** `86afrhqxy`
**Client:** Mountain Capital Partners
**Category:** Workflow Automation / HR
**Source:** `devsentient/demos/hr-resume-demo`

### Services

```yaml
- jobName: hr-resume-demo
  subdomain: hr-resume-demo
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: hr-resume-demo/run.sh
  port: "8787"
  deployOrder: 1
  lifecycleMode: lite-create-delete
  parameters: []
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /health
      expected: 200
    - path: /
      expected: 200-or-302
```

### Mock / real notes

- Works end to end in mock mode with hardcoded applicants.
- Real go-live inputs may include `PAYCOM_API_KEY`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `OPENAI_API_KEY`.

### Lite lifecycle notes

- Start: create `hr-resume-demo`
- Stop in lite-only mode: delete after confirmation
- Restart in lite-only mode: delete + recreate

---

## Gallo Freight Exception Hub

**ClickUp Spec:** `86afvjudd`
**Client:** E.J. Gallo Winery
**Category:** Operational Assistant / Workflow Automation
**Source:** `devsentient/demos/gallo-freight-exception-hub`

### Services

```yaml
- jobName: gallo-api
  subdomain: gallo-api
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: gallo-freight-exception-hub/api/run.sh
  port: "8787"
  deployOrder: 1
  lifecycleMode: lite-create-delete
  parameters:
    - key: OPENAI_API_KEY
      default: sk-mock
    - key: N8N_WEBHOOK_URL
      default: ""
    - key: GALLO_API_KEY
      default: ""
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /health
      expected: 200
    - path: /exceptions
      expected: 200

- jobName: gallo-ui
  subdomain: gallo-ui
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: gallo-freight-exception-hub/ui/run.sh
  port: "8787"
  deployOrder: 2
  lifecycleMode: lite-create-delete
  parameters: []
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /
      expected: 200-or-302
```

### Notes

- Deploy `gallo-api` before `gallo-ui`.
- The UI assumes the API is available at `https://gallo-api.dev.hyperplane.dev` unless the frontend build script is changed.
- Pre-built n8n workflows are optional for the POC; the app can still tell the story with direct fallback behavior.

### Go-live inputs

- `OPENAI_API_KEY`
- `N8N_WEBHOOK_URL`
- optional `GALLO_API_KEY`

---

## Reagan NLP-to-SQL

**ClickUp Spec:** `86afrnky0`
**Client:** Reagan Foundation
**Category:** Data & Analytics
**Source:** `devsentient/demos/reagan-nlp-sql-demo`

### Services

```yaml
- jobName: reagan-crm-chat
  subdomain: reagan-crm-chat
  jobType: basic-medium
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: reagan-nlp-sql-demo/run.sh
  port: "8787"
  deployOrder: 1
  lifecycleMode: lite-create-delete
  parameters:
    - key: ANTHROPIC_API_KEY
      default: sk-ant-mock
    - key: DATABASE_URL
      default: seeded-mock-schema
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /
      expected: 200-or-302
```

### Notes

- Use `basic-medium` because the app installs heavier dependencies.
- Best fit when the core value is natural-language access to structured donor / CRM data.
- This is a strong reuse pattern for analytics copilots with a single app surface.

### Go-live inputs

- `ANTHROPIC_API_KEY`
- `DATABASE_URL` or real Salesforce-synced warehouse path

---

## Campbell Ops Co-Pilot

**ClickUp Spec:** `86afxwcn9`
**Client:** Campbell & Company
**Category:** Operational Assistant / Multi-Agent
**Source:** `devsentient/demos/campbell-ops-demo`

### Services

```yaml
- jobName: campbell-ops-demo
  subdomain: campbell-ops-demo
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: campbell-ops-demo/run.sh
  port: "8787"
  deployOrder: 1
  lifecycleMode: lite-create-delete
  parameters:
    - key: ANTHROPIC_API_KEY
      default: sk-ant-mock
    - key: CLICKUP_API_KEY
      default: mock
    - key: MATTERMOST_WEBHOOK_URL
      default: ""
    - key: SUPABASE_URL
      default: sqlite-fallback
    - key: SUPABASE_KEY
      default: ""
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /health
      expected: 200
    - path: /
      expected: 200-or-302
```

### Notes

- Good reference for incident investigation and action-with-confirmation flows.
- Multi-agent framing is acceptable here because the user story naturally separates monitoring, diagnosis, and action.
- Mattermost alerts and ClickUp ticket creation can remain mocked for the POC.

### Go-live inputs

- `ANTHROPIC_API_KEY`
- `CLICKUP_API_KEY`
- `MATTERMOST_WEBHOOK_URL`
- `SUPABASE_URL`
- `SUPABASE_KEY`

---

## Dynex MBS Intelligence Platform

**ClickUp Spec:** `86ag1jmra`
**Client:** Dynex Capital
**Category:** Data & Analytics / Operational Assistant
**Source:** `devsentient/demos/dynex-mbs`

### Services

```yaml
- jobName: dynex-mbs-api
  subdomain: dynex-mbs-api
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: dynex-mbs/api/run.sh
  port: "8787"
  deployOrder: 1
  lifecycleMode: lite-create-delete
  parameters:
    - key: OPENAI_API_KEY
      default: sk-mock
    - key: OTEL_EXPORTER_OTLP_ENDPOINT
      default: https://arize-phoenix.dev.hyperplane.dev
    - key: N8N_WEBHOOK_URL
      default: https://n8n-v2.dev.hyperplane.dev/webhook/briefing-approval
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /health
      expected: 200
    - path: /portfolio
      expected: 200

- jobName: dynex-mbs-ui
  subdomain: dynex-mbs-ui
  jobType: basic-ai-tools-small
  gitServerName: demos
  branchName: main
  workingDir: /tmp/git/monorepo/
  pipelineYamlPath: dynex-mbs/ui/run.sh
  port: "8787"
  deployOrder: 2
  lifecycleMode: lite-create-delete
  parameters: []
  recreateNotes: Delete + recreate if only lite lifecycle is available
  smokeTests:
    - path: /
      expected: 200-or-302
```

### Notes

- Deploy `dynex-mbs-api` before `dynex-mbs-ui`.
- This is the strongest reference for finance / portfolio intelligence apps that combine analytics, briefing generation, and approval workflow.
- The API can run with mocked LLM responses and Phoenix observability enabled in dev.

### Go-live inputs

- `OPENAI_API_KEY` or Azure OpenAI equivalents
- `OPENAI_BASE_URL`
- `OPENAI_API_VERSION`
- `OTEL_EXPORTER_OTLP_ENDPOINT`
- optional `N8N_WEBHOOK_URL`

---

## Adding a new POC entry

When a new POC is built, add:
1. client + category metadata
2. the exact deployment specs for each service
3. deploy order
4. mock / real credential notes
5. smoke tests
6. lite lifecycle notes if there is anything non-obvious about recreate behavior
