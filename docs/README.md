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
- Release checklist: [RELEASE_CHECKLIST.md](./RELEASE_CHECKLIST.md)
- ChatGPT submission notes: [CHATGPT_SUBMISSION.md](./CHATGPT_SUBMISSION.md)
- Claude submission notes: [CLAUDE_SUBMISSION.md](./CLAUDE_SUBMISSION.md)
- Claude directory packet: [CLAUDE_DIRECTORY_PACKET.md](./CLAUDE_DIRECTORY_PACKET.md)

## Release Reality

### Production / GA today

- live public MCP endpoint at `https://doxa.papersflow.ai/mcp`
- production PAT support for direct clients
- Codex / Gemini CLI / Claude Code real smoke testing against the live endpoint
- OAuth authorization, token, revoke, and dynamic client registration flows
- PAT create / copy / revoke flows in the PapersFlow settings UI
- OAuth approval flow inside PapersFlow Settings

### Still not release-complete

- final marketplace review prep for ChatGPT and Claude
- broader UX polish for PAT onboarding and client setup
- anonymous quota and premium entitlement hardening
- final production hardening pass on auth and telemetry

## Client Compatibility

- Codex: yes, tested against production
- Gemini CLI: yes, tested against production
- Claude Code: yes, tested against production
- ChatGPT hosted MCP app path: viable now that `search` and `fetch` compatibility tools exist and OAuth metadata is live
- ChatGPT richer Apps SDK UI: still a second step if we want inline panes, charts, or custom presentation
- ChatGPT public app/directory release: still needs more hardening and review prep
- Claude public connector/marketplace release: still needs deployment and auth completion

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

1. Polish the Settings onboarding so users can move from PAT creation to client config without guesswork.
2. Run a final entitlement and anonymous-quota hardening pass.
3. Re-run client smoke tests against production for Codex, Claude Code, and Gemini CLI.
4. Run a full OAuth install test with a marketplace-style client.
5. Prepare company-facing screenshots, policy copy, and support docs.
6. Submit Claude first.
7. Submit ChatGPT after the final compatibility and review pass.
