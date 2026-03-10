# papersflow-mcp Tutorial

## 1. Use the hosted PapersFlow MCP

Production endpoint:

```text
https://doxa.papersflow.ai/mcp
```

OAuth metadata:

```text
https://doxa.papersflow.ai/.well-known/oauth-protected-resource
https://doxa.papersflow.ai/.well-known/oauth-authorization-server
```

If you need local development instead, build and run `doxa-vps`:

```bash
npm --prefix doxa-vps run build
cd doxa-vps
node dist/index.js
```

## 2. Create a token in PapersFlow Settings

1. Sign into PapersFlow.
2. Open `Settings` -> `Integrations` -> `papersflow-mcp`.
3. Create a PAT with the scopes the client needs.
4. Copy the raw token once.

Use `mcp.public.read` for search and citation workflows.

Use `mcp.research.read` or `mcp.research.run` only for plans that should unlock premium research tooling.

## 3. Test the endpoint directly

Initialize:

```bash
curl -sS https://doxa.papersflow.ai/mcp \
  -H "Authorization: Bearer $PAPERSFLOW_MCP_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"curl","version":"0.1.0"}}}'
```

List tools:

```bash
curl -sS https://doxa.papersflow.ai/mcp \
  -H "Authorization: Bearer $PAPERSFLOW_MCP_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list"}'
```

## 4. Connect Codex

Add server:

```bash
export PAPERSFLOW_MCP_TOKEN=YOUR_TOKEN
codex mcp add papersflow-mcp --url https://doxa.papersflow.ai/mcp --bearer-token-env-var PAPERSFLOW_MCP_TOKEN
```

Important:

- `--bearer-token-env-var` expects the env var name, not the raw `pf_pat_...` token value
- export the token first, then reference `PAPERSFLOW_MCP_TOKEN`

Smoke test:

```bash
codex exec --full-auto \
  "Use the papersflow-mcp MCP server to run search_literature for transformer scaling laws and respond with only the first paper title."
```

## 5. Connect Claude Code

Command style:

```bash
claude mcp add --transport http papersflow-mcp https://doxa.papersflow.ai/mcp \
  --header "Authorization: Bearer YOUR_TOKEN"
```

Project or local `.mcp.json`:

```json
{
  "mcpServers": {
    "papersflow-mcp": {
      "type": "http",
      "url": "https://doxa.papersflow.ai/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

Claude project `.mcp.json` does not currently expand env placeholders reliably, so the cleanest path is a direct bearer header.

Example:

```bash
claude -p \
  --allowedTools mcp__papersflow-mcp__search_literature \
  --strict-mcp-config \
  --mcp-config '{"mcpServers":{"papersflow-mcp":{"type":"http","url":"https://doxa.papersflow.ai/mcp","headers":{"Authorization":"Bearer YOUR_TOKEN"}}}}' \
  -- "Use the papersflow-mcp MCP server to run search_literature for transformer scaling laws and respond with only the first paper title."
```

## 6. Connect Gemini CLI

Command style:

```bash
gemini mcp add --scope project --transport http papersflow-mcp https://doxa.papersflow.ai/mcp \
  -H "Authorization: Bearer YOUR_TOKEN"
```

Extension-repo style:

```bash
gemini extensions install https://github.com/papersflow-ai/papersflow-mcp
```

`gemini extensions install` installs the extension package. It does not complete OAuth during install.

Gemini MCP config:

```json
{
  "mcpServers": {
    "papersflow-mcp": {
      "type": "http",
      "url": "https://doxa.papersflow.ai/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

Then start Gemini and use the in-session MCP commands when needed:

```bash
/mcp list
/mcp refresh
```

If Gemini reports:

```text
papersflow-mcp (from papersflow-mcp) - Disconnected (OAuth not authenticated)
```

run:

```bash
/mcp auth papersflow-mcp
/mcp refresh
/mcp list
```

That status is expected until the browser login and OAuth consent flow finishes successfully. If you installed from the extension repo, the browser opens after Gemini starts the auth flow, not during `gemini extensions install`.

Smoke test:

```bash
gemini \
  --prompt "Use the papersflow-mcp MCP server to run search_literature for transformer scaling laws and respond with only the first paper title." \
  --allowedTools mcp__papersflow-mcp__search_literature \
  --approval-mode yolo
```

If the server later requires OAuth in Gemini, use:

```bash
/mcp auth papersflow-mcp
```

Gemini supports both:

- top-level `gemini mcp add/remove/list`
- config-based setup in `~/.gemini/settings.json` or workspace `.gemini/settings.json`

## 7. OAuth approval flow

Current flow:

1. Client hits `/oauth/authorize`
2. User is redirected to `/settings?section=integrations&mcp_request_id=...`
3. User approves or denies from the MCP settings card
4. Approval redirects back with an authorization code
5. Client exchanges the code at `/oauth/token`

Dynamic client registration endpoint:

```text
https://doxa.papersflow.ai/oauth/register
```

## 8. Connect ChatGPT

There are two ChatGPT-shaped paths:

### Path A: hosted MCP app connection

Use the live hosted MCP surface and OAuth metadata:

```text
MCP server URL: https://doxa.papersflow.ai/mcp
Protected resource metadata: https://doxa.papersflow.ai/.well-known/oauth-protected-resource
Authorization server metadata: https://doxa.papersflow.ai/.well-known/oauth-authorization-server
```

Start with these tools:

- `search`
- `fetch`
- `verify_citation`
- `search_literature`
- `find_related_papers`
- `get_citation_graph`
- `get_paper_neighbors`
- `expand_citation_graph`

This is the right first step for a ChatGPT-facing MCP app or internal compatibility pass.

### Path B: richer ChatGPT app UX

If you want inline tables, charts, or custom panes inside ChatGPT, add an Apps SDK UI layer on top
of the MCP server rather than expecting plain MCP tool output to carry the whole experience.

OpenAI references:

- `https://developers.openai.com/apps-sdk/`
- `https://developers.openai.com/apps-sdk/mcp-apps-in-chatgpt`
- `https://developers.openai.com/apps-sdk/build/chatgpt-ui`

## 9. What is still missing before public marketplace release

- final marketplace-specific review pass
- stricter anonymous quota and entitlement hardening
- broader onboarding copy and screenshots for public listing pages
- final ChatGPT and Claude marketplace submission prep
