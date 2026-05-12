# Signaliz MCP

Signaliz is a B2B data quality and enrichment platform for go-to-market teams and AI agents. The hosted Signaliz MCP server helps agents find and verify professional emails, verify deliverability, enrich companies with buying signals, clean and govern lists, and run data-quality pipelines before records move into CRMs, sequencers, or other systems of record.

This repository is the public distribution manifest for the hosted Signaliz MCP server. It contains metadata, tool schemas, and setup instructions for MCP registries and clients. The production server is hosted by Signaliz.

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

## What Signaliz Does

Signaliz exposes tools for:

- Finding and verifying professional email addresses from a person's name and company domain.
- Verifying existing email addresses at batch scale.
- Enriching companies with structured business signals such as hiring, funding, product launches, partnerships, leadership changes, expansion, acquisitions, awards, regulatory events, and earnings.
- Uploading CSV or JSON lists for processing.
- Building and running governed data pipelines with validation, deduplication, blocklists, data contracts, dead-letter handling, and output sinks.
- Checking async job and pipeline status.

## Key Tools

| Tool | Purpose | Safety |
| --- | --- | --- |
| `list_capabilities` | List available Signaliz pipeline capabilities. | Read-only |
| `find_emails_with_verification` | Find and verify one professional email address. | Read-only, idempotent |
| `find_and_verify_emails` | Submit up to 5,000 contacts for async email finding and verification. | Read-only, idempotent |
| `verify_emails` | Submit up to 5,000 emails for async deliverability verification. | Read-only, idempotent |
| `verify_email` | Verify one email address. | Read-only, idempotent |
| `enrich_company_signals` | Discover structured company buying signals. | Read-only, idempotent |
| `execute_primitive` | Run small synchronous enrichment, verification, AI prompt, or HTTP jobs. | Depends on selected capability |
| `check_job_status` | Poll async batch jobs. | Read-only |
| `upload_data` | Upload CSV or JSON records into a workspace list. | Writes workspace data |
| `create_system` | Create a saved Signaliz automation pipeline. | Writes workspace configuration |
| `run_system` | Execute a saved pipeline. | May write outputs depending on pipeline |
| `manage_blocklist` | Manage suppression, DNC, bounce, competitor, or custom blocklists. | Writes workspace policy |

The full indexable tool schema lives in [`tools/tools.json`](tools/tools.json).

## Registry Metadata

- Official MCP Registry manifest: [`server.json`](server.json)
- Smithery URL-publish config schema: [`smithery.config-schema.json`](smithery.config-schema.json)
- Glama ownership metadata: [`glama.json`](glama.json)
- OAuth details: [`docs/authentication.md`](docs/authentication.md)

## Safety Notes

Signaliz is designed as a data-quality checkpoint for agent workflows. Email verification and company enrichment tools are read-only against the user's systems of record. Pipeline, upload, output, and blocklist tools can create or update Signaliz workspace resources and should be used with the client's normal consent controls.

## Links

- Website: https://signaliz.com
- Hosted MCP endpoint: https://api.signaliz.com/functions/v1/signaliz-mcp
- Main Signaliz repo: https://github.com/signaliz/signaliz
- Claude Code plugin: https://github.com/signaliz/claude-code-plugin
- Skills: https://github.com/signaliz/signaliz-skills
