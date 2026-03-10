# papersflow-mcp

`papersflow-mcp` is the production / GA MCP server for PapersFlow’s hosted research tooling.

The hosted service runs on PapersFlow infrastructure and uses the PapersFlow platform for identity, billing, installation state, token storage, and invocation telemetry.

## Goals

- expose a narrow, high-trust research surface to external AI clients
- keep public tools read-only and guest-safe
- gate account and paid workflows behind PapersFlow auth and plan entitlements
- support both PAT-based direct installs and OAuth-based marketplace installs

## Tool Surface

### Public

- `search`
- `fetch`
- `verify_citation`
- `search_literature`
- `find_related_papers`

### Signed-in

- `summarize_evidence`

### Paid

- `run_deepscan`
- `get_deepscan_status`
- `get_deepscan_report`
- `run_python_plot`

## Runtime Layout

- Streamable HTTP MCP endpoint
- OAuth metadata and token routes
- Tool catalog and execution layer
- PapersFlow-backed auth, billing, quota, and installation state

## Operational Docs

- Setup tutorial: [TUTORIAL.md](./TUTORIAL.md)

## Release Reality

### Production / GA today

- live public MCP endpoint at `https://doxa.papersflow.ai/mcp`
- production PAT support for direct clients
- Codex / Gemini CLI / Claude Code real smoke testing against the live endpoint
- OAuth authorization, token, revoke, and dynamic client registration flows
- PAT create / copy / revoke flows in the PapersFlow settings UI
- OAuth approval flow inside PapersFlow Settings

## Client Compatibility

- Codex: yes, tested against production
- Gemini CLI: yes, tested against production
- Claude Code: yes, tested against production
- ChatGPT hosted MCP app path: supported through the hosted MCP endpoint and OAuth metadata
- ChatGPT richer Apps SDK UI: a separate layer if inline panes or custom presentation are needed
- Claude connector path: supported through the hosted MCP endpoint and OAuth metadata

## User Setup Reality

- Codex: best for command-line setup, but users must pass an env var name to `--bearer-token-env-var`, not the raw PAT value
- Claude Code: supports both `claude mcp add --transport http ...` and `.mcp.json`
- Gemini CLI: supports `gemini mcp add/remove/list` and also config-based setup in `~/.gemini/settings.json` or workspace `.gemini/settings.json`
- ChatGPT: use the hosted MCP endpoint plus OAuth metadata first; richer UI inside ChatGPT is a second Apps SDK layer

## Official Platform References

- OpenAI developer portal:
  `https://developers.openai.com/`
- OpenAI resources hub:
  `https://developers.openai.com/resources`
- Anthropic Claude Code MCP docs:
  `https://docs.anthropic.com/en/docs/claude-code/mcp`
- Anthropic custom connectors overview:
  `https://support.anthropic.com/en/articles/11175166-about-custom-connectors`

## Recommended Release Sequence

1. Connect the hosted MCP endpoint from your client of choice.
2. Use public tools first for literature search and citation workflows.
3. Use OAuth or account entitlements for authenticated and paid workflows.
