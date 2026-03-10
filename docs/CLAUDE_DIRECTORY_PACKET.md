# Claude Directory Packet

This file is the paste-ready packet for Anthropic's remote MCP server submission form for `papersflow-mcp`.

## Production Status

- Status: Production / GA for the hosted MCP service
- Remote MCP endpoint: `https://doxa.papersflow.ai/mcp`
- Auth: OAuth 2.0 for connector-style installs, PAT support for direct power-user clients
- Transport: Streamable HTTP over HTTPS

## Public Company And Policy Links

- Company: `PapersFlow`
- Website: `https://papersflow.ai`
- Privacy Policy: `https://papersflow.ai/privacy`
- Terms of Service: `https://papersflow.ai/terms`
- Support URL: `https://papersflow.ai/contact`
- Support Email: `developer@papersflow.ai`

## Short Company Description

PapersFlow is a research workspace for literature discovery, citation verification, evidence synthesis, and agentic research workflows.

## Short MCP Description

`papersflow-mcp` is a remotely hosted MCP server that gives Claude a narrow, high-trust research interface for finding papers, verifying citations, and discovering related literature.

## Recommended Initial Tool Surface

Lead with:

- `verify_citation`
- `search_literature`
- `find_related_papers`

Compatibility tools also available:

- `search`
- `fetch`

Keep account-linked and premium tools gated until requested:

- `summarize_evidence`
- `run_deepscan`
- `get_deepscan_status`
- `get_deepscan_report`
- `run_python_plot`

## Three Example Prompts

1. `Find 5 papers on retrieval-augmented generation evaluation and return the title, year, and DOI for each.`
2. `Verify this citation and return the normalized metadata plus an APA citation: Attention Is All You Need, Vaswani et al., 2017.`
3. `Find papers related to "Attention Is All You Need" and group them into similar papers, references, and later citations.`

## Reviewer Test Account Package

Anthropic's guide asks for a standard testing account with sample data. Prepare a dedicated reviewer account with:

- Account email: `mcp-review@papersflow.ai` or similar dedicated reviewer alias
- Plan: `Pro` or higher
- OAuth approval enabled
- At least one completed DeepScan report in the account
- At least one sample paper collection or library-linked artifact if authenticated tools will be reviewed
- One dedicated support contact for account resets and troubleshooting

Suggested reviewer note:

`A dedicated reviewer account with sample data is available on request via developer@papersflow.ai.`

## Connector-Facing Notes

- Hosted production endpoint: `https://doxa.papersflow.ai/mcp`
- OAuth metadata:
  - `https://doxa.papersflow.ai/.well-known/oauth-protected-resource`
  - `https://doxa.papersflow.ai/.well-known/oauth-authorization-server`
- Dynamic client registration:
  - `https://doxa.papersflow.ai/oauth/register`

## Submission Checklist

- `doxa-vps` redeployed with live tool annotations
- fresh connector install tested from Claude
- denied auth flow tested
- revoked token flow tested
- screenshots captured
- privacy / terms / support links verified publicly accessible
