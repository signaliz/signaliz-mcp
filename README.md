# Signaliz MCP

Signaliz is a B2B data quality, enrichment, and agentic GTM platform for go-to-market teams and AI agents. The Signaliz MCP server gives MCP clients access to 60+ Signaliz API capabilities for verified lead generation, email verification, company enrichment, data-quality pipelines, Ops routines, approvals, ICPs, app actions, and workspace controls.

This repository is the public distribution manifest for Signaliz MCP. It contains registry metadata, indexable tool schemas, and setup instructions for MCP directories and clients. The production server is hosted by Signaliz; this repository does not contain the private server implementation.

## Hosted Endpoint

```text
https://api.signaliz.com/functions/v1/signaliz-mcp
```

Transport: Streamable HTTP

Authentication: Signaliz API key or OAuth, depending on client support.

## Quick Start

Create an API key in Signaliz from Settings > Developer > API Access, then connect your MCP client to:

```text
https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_API_KEY
```

Clients that support headers can connect to the base endpoint and send:

```text
Authorization: Bearer YOUR_API_KEY
```

Clients that support OAuth discovery can use the protected-resource metadata advertised by the server.

## Local Stdio Bridge

For clients that prefer local stdio MCP servers, install the npm bridge:

```bash
claude mcp add signaliz -e SIGNALIZ_API_KEY=sk_your_key -- npx -y @signaliz/mcp-server
```

Claude Desktop, Cursor, Windsurf, Cline, and other stdio clients can use:

```json
{
  "mcpServers": {
    "signaliz": {
      "command": "npx",
      "args": ["-y", "@signaliz/mcp-server"],
      "env": {
        "SIGNALIZ_API_KEY": "sk_your_key"
      }
    }
  }
}
```

## What Signaliz Does

Signaliz exposes tools for:

- Finding verified emails for known contacts.
- Verifying deliverability for one email or thousands of emails.
- Discovering buying signals such as hiring, funding, product launches, partnerships, leadership changes, expansion, acquisitions, awards, regulatory events, and earnings.
- Finding B2B professionals, target accounts, local businesses, or outreach-ready leads.
- Running custom AI prompts across records with structured output fields.
- Calling external APIs through generic HTTP tools.
- Uploading lists and running governed Signaliz systems against them.
- Creating autonomous Ops Routines that work on a GTM goal over time.
- Chaining routines, streaming results, emitting events, and resolving approvals.
- Connecting apps, discovering available actions, and executing app actions through Signaliz.
- Managing budgets, blocklists, ICPs, campaign books, and durable agent memory.

Every tool accepts `output_format`, which can be `markdown` or `json`. Use `json` when another agent, script, or workflow needs structured output.

## Access Model

Signaliz plans include full API and MCP access, unlimited workspaces, unlimited integrations, audit trail and MCP analytics, BYOK OpenRouter support, and all core platform features. Plans differ by included monthly GTM credits, and additional credits can be purchased for metered provider-backed actions such as email verification, person or company discovery, company signal enrichment, monitoring, custom AI prompts, and Lead Finder.

Signaliz is not positioned as unlimited-volume or all-you-can-use data. The product is designed around accuracy, timing, governed workflows, and transparent GTM credit usage.

## Capability Index

The full indexable tool schema lives in [`tools/tools.json`](tools/tools.json). It is generated from the actual `@signaliz/mcp-server@2.0.1` `tools/list` response so directories can index the same names, descriptions, and input schemas exposed by the npm bridge.

### Email, Contact, and Company Enrichment

| Tool | What it does |
| --- | --- |
| `find_contacts_with_email` | Finds contacts at a company and returns verified work emails. |
| `find_emails_with_verification` | Finds and verifies one professional email address from a name, company domain, or LinkedIn URL. |
| `verify_email` | Verifies one email address for deliverability. |
| `enrich_company_signals` | Researches a company for structured signals and source-backed developments. |
| `company_intelligence` | Runs AI-powered company research from websites, domains, LinkedIn URLs, and extra URLs. |
| `execute_primitive` | Runs small synchronous jobs for email verification, email finding, signal enrichment, custom AI prompts, data cleaning, or HTTP requests. |
| `generic_http_request` | Executes one custom HTTP request with headers, auth, body templates, variables, timeout, and response extraction. |
| `custom_ai_prompt` | Runs a custom AI prompt over records with model selection, structured output, and optional Signaliz AI Search context. |

### Batch Jobs

| Tool | What it does |
| --- | --- |
| `find_and_verify_emails` | Finds and verifies emails for up to 5,000 contacts asynchronously. |
| `verify_emails` | Verifies up to 5,000 emails asynchronously. |
| `enrich_company_signals_batch` | Enriches signals for up to 5,000 companies asynchronously. |
| `batch_http_request` | Executes up to 5,000 HTTP requests asynchronously. |
| `check_job_status` | Polls async jobs, reports progress, and retrieves paginated results. |

Batch tools return a `job_id` immediately. Poll with `check_job_status`, or pass `callback_url` and optional `callback_secret` on supported batch tools to receive a completion webhook.

### Lead and Account Discovery

| Tool | What it does |
| --- | --- |
| `generate_leads` | Finds B2B professionals with company data and verified emails for outreach-ready lead lists. |
| `generate_local_leads` | Finds local businesses and verified contact emails from Google Maps-style local search. |
| `find_people_signaliz` | Finds professional profiles without verified emails for research and list building. |
| `find_companies_signaliz` | Finds company records for account discovery, market sizing, and TAM research. |

### Lists, Systems, and Pipeline Runs

| Tool | What it does |
| --- | --- |
| `list_capabilities` | Lists available Signaliz capabilities, schemas, categories, and credit costs. |
| `upload_data` | Uploads inline records, public CSV URLs, Google Sheets URLs, or Google Drive share URLs as workspace lists. |
| `list_systems` | Lists saved Signaliz automation systems in the workspace. |
| `get_system` | Gets a saved system's phases, required inputs, and configuration. |
| `create_system` | Creates a saved automation system from a plain-English description. |
| `run_system` | Runs a saved system using inline data, a saved list, the latest workspace list, or prior run data. |
| `get_run` | Gets one system run's status and results. |
| `list_runs` | Lists recent system runs with status, timing, and summaries. |

### Ops Routines as Agents

| Tool | What it does |
| --- | --- |
| `create_routine` | Creates a goal-driven Ops Routine with cadence, policy guardrails, wake events, and output sinks. |
| `list_routines` | Lists Ops Routines by status. |
| `get_routine` | Gets full routine details, policy, sinks, recent ticks, and outcomes. |
| `update_routine` | Updates routine name, goal, cadence, status, policy, sinks, or wake events. |
| `run_routine_now` | Triggers an Ops Routine immediately with an optional one-off instruction. |
| `delete_routine` | Soft-archives or permanently deletes a routine after confirmation. |
| `get_routine_ticks` | Lists historical routine ticks with status, record counts, credits, and errors. |
| `get_tick_items` | Fetches records produced by a specific tick. |
| `get_last_tick_items` | Fetches records from the latest successful tick. |
| `get_routine_results` | Agent-friendly helper that combines routine metadata with latest produced items. |
| `agent_workflow` | Creates, activates, triggers, optionally waits for, and optionally cleans up a one-shot Ops Routine. |
| `stream_routine_results` | Returns an SSE endpoint for real-time item events from a running routine tick. |
| `chain_routines` | Chains two routines so upstream output feeds downstream seed items. |
| `get_chain_status` | Shows chained routine execution status and per-stage item counts. |
| `emit_event` | Emits an event that can wake routines subscribed through `wake_on_events`. |

### Approvals and Feedback

| Tool | What it does |
| --- | --- |
| `approvals_list` | Lists pending, approved, or rejected human-in-the-loop approvals. |
| `approvals_decide` | Approves or rejects one or more pending approval tokens. |
| `set_item_feedback` | Stores feedback on a produced lead or item so future routine ticks can improve. |

### App Connections and Actions

| Tool | What it does |
| --- | --- |
| `list_connections` | Lists active app connections in the workspace. |
| `list_available_connectors` | Lists available connectors across CRM, marketing, communication, productivity, analytics, development, support, finance, HR, ecommerce, email, and sales. |
| `list_connectors` | Legacy alias for available connector listing. |
| `connect_app` | Starts the app connection flow and returns authentication instructions. |
| `discover_actions` | Lists available entities and actions for a connected app. |
| `execute_connection_action` | Executes read or write actions against a connected app. |
| `list_connection_audit_log` | Shows connection lifecycle events, tests, and errors for debugging. |

### GTM Books and ICPs

| Tool | What it does |
| --- | --- |
| `quickstart_gtm_book` | Launches a coordinated GTM Campaign Book made of multiple Ops Routines. |
| `list_books` | Lists GTM Campaign Books in the workspace. |
| `book_roll_up` | Gets aggregate performance metrics across a campaign book. |
| `list_icps` | Lists Ideal Customer Profiles and optional Octave import context. |
| `get_icp` | Gets full ICP details, criteria, pain points, triggers, titles, departments, geography, and related playbook data. |
| `import_icps_from_octave` | Imports Octave playbooks as Signaliz ICPs. |

### Workspace Controls

| Tool | What it does |
| --- | --- |
| `budget_get_status` | Gets current credit balance, spend rate, limits, and projected runway. |
| `budget_set_limit` | Sets daily, monthly, or system-specific credit limits and alert thresholds. |
| `manage_blocklist` | Adds, removes, or lists suppressed domains and emails. |
| `write_agent_memory` | Stores durable agent memory for future sessions. |
| `query_agent_memory` | Searches stored agent memories with semantic retrieval. |
| `list_output_sinks` | Deprecated compatibility tool; Signaliz now returns JSON, CSV artifacts, and webhooks. |

## Registry Metadata

- Official MCP Registry manifest: [`server.json`](server.json)
- Smithery URL-publish config schema: [`smithery.config-schema.json`](smithery.config-schema.json)
- Glama ownership metadata: [`glama.json`](glama.json)
- OAuth details: [`docs/authentication.md`](docs/authentication.md)

## Safety Notes

Read-only enrichment and discovery tools do not mutate your external systems. Tools that upload data, create or run systems, connect apps, execute app actions, update blocklists, approve work, or delete routines can change Signaliz workspace resources or connected systems and should be used with your normal approval controls.

## Links

- Homepage: https://signaliz.com
- NPM package: https://www.npmjs.com/package/@signaliz/mcp-server
- Hosted MCP endpoint: https://api.signaliz.com/functions/v1/signaliz-mcp
- Official MCP Registry name: `io.github.signaliz/signaliz-mcp`
- Main Signaliz repo: https://github.com/signaliz/signaliz
- Public manifest repo: https://github.com/signaliz/signaliz-mcp
- Claude Code plugin: https://github.com/signaliz/claude-code-plugin
- Skills: https://github.com/signaliz/signaliz-skills
