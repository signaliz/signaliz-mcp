# Authentication

Signaliz supports API-key authentication and advertises OAuth protected-resource metadata for clients that support MCP remote OAuth flows.

## API Key

Create an API key in the Signaliz dashboard:

1. Open Signaliz.
2. Go to Settings > Developer > API Access.
3. Create an API key.
4. Store it in your MCP client as a secret.

You can pass the key as a query parameter:

```text
https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_API_KEY
```

Or as an Authorization header:

```text
Authorization: Bearer YOUR_API_KEY
```

## OAuth Metadata

The MCP endpoint returns `401 Unauthorized` for unauthenticated JSON-RPC calls and includes a `WWW-Authenticate` header pointing clients at the protected-resource metadata document.

Current metadata advertises:

- Resource: `https://api.signaliz.com/functions/v1/signaliz-mcp`
- Authorization endpoint: `https://signaliz.com/oauth/authorize`
- Token endpoint: `https://api.signaliz.com/functions/v1/signaliz-mcp/token`
- Dynamic client registration endpoint: `https://api.signaliz.com/functions/v1/signaliz-mcp/register`
- PKCE: `S256`

Supported scopes include:

- `signals:read`
- `enrichment:read`
- `systems:read`
- `systems:execute`
- `output:read`
- `output:write`

## Smithery Configuration

For Smithery URL publishing, use [`smithery.config-schema.json`](../smithery.config-schema.json). It prompts users for a Signaliz API key and forwards it to the hosted endpoint as the `api_key` query parameter.
