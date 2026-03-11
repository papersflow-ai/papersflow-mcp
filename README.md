# papersflow-mcp

`papersflow-mcp` is PapersFlow's production / GA hosted MCP server for literature search, citation verification, related-paper discovery, and authenticated research workflows across Claude, Codex, Gemini, ChatGPT-style MCP apps, and other MCP clients.

## Production Endpoint

- MCP URL: `https://doxa.papersflow.ai/mcp`
- OAuth protected resource metadata: `https://doxa.papersflow.ai/.well-known/oauth-protected-resource`
- OAuth authorization server metadata: `https://doxa.papersflow.ai/.well-known/oauth-authorization-server`
- Dynamic client registration: `https://doxa.papersflow.ai/oauth/register`

## Public Tool Surface

- `search`
- `fetch`
- `verify_citation`
- `search_literature`
- `find_related_papers`
- `get_citation_graph`
- `get_paper_neighbors`
- `expand_citation_graph`

Authenticated and paid tools are available through PapersFlow account access and plan entitlements.

## Documentation

- [Setup tutorial](./docs/TUTORIAL.md)
- [MCP release guide](./docs/README.md)

## Gemini CLI Extension

This repository also includes a root `gemini-extension.json` so Gemini CLI can install it as an extension wrapper around the hosted MCP server.

Official Gemini CLI flow supported by the docs:

```bash
gemini extensions install https://github.com/papersflow-ai/papersflow-mcp
```

The extension manifest lives at the repository root and points Gemini CLI at the hosted MCP endpoint using Gemini's remote MCP config shape.

If Gemini shows:

```text
papersflow-mcp (from papersflow-mcp) - Disconnected (OAuth not authenticated)
```

that is expected until OAuth is completed inside Gemini:

```text
/mcp auth papersflow-mcp
/mcp refresh
/mcp list
```

`gemini extensions install` installs the extension package. It does not complete OAuth during install. The browser opens when Gemini starts the remote MCP auth flow, typically via `/mcp auth papersflow-mcp` or the first auth-required connection attempt.

Gemini CLI references:

- `https://geminicli.com/docs/extensions/releasing/`
- `https://geminicli.com/docs/extensions/reference/`
- `https://geminicli.com/docs/extensions/writing-extensions/`

## Client Setup

### Cline (VS Code)

Add PapersFlow as a remote MCP server in your Cline MCP settings:

```json
{
  "mcpServers": {
    "papersflow": {
      "url": "https://doxa.papersflow.ai/mcp",
      "transportType": "streamable-http"
    }
  }
}
```

On first use, Cline will open a browser window for OAuth sign-in to your PapersFlow account. Public tools (literature search, citations, paper discovery) work immediately; authenticated tools require a PapersFlow plan.

### Other Clients

Supported client paths today:

- Codex
- Claude Code
- Gemini CLI
- ChatGPT-style hosted MCP apps

The setup guide includes both command-style and config-style snippets where supported.

## Support And Policies

- Website: `https://papersflow.ai`
- Privacy: `https://papersflow.ai/privacy`
- Terms: `https://papersflow.ai/terms`
- Support: `https://papersflow.ai/contact`
- Support email: `developer@papersflow.ai`

## Notes

- This repository is the public documentation and submission surface for the hosted MCP service.
- The production service is operated by PapersFlow and backed by the main PapersFlow platform.
