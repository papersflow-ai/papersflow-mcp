# ChatGPT Submission Notes

This file is the company-facing checklist for a future ChatGPT app or MCP-style submission based on `papersflow-mcp`.

## Current Position

What is already live:

- public MCP endpoint: `https://doxa.papersflow.ai/mcp`
- protected-resource metadata
- authorization-server metadata
- JWKS endpoint
- dynamic client registration
- OAuth approval inside PapersFlow Settings
- PAT-based setup for direct power-user clients
- public compatibility tools: `search` and `fetch`

What this means:

- the technical base is now credible for OpenAI-side compatibility work
- internal and private-alpha testing can proceed with real hosted infrastructure
- we are no longer blocked on basic MCP transport or auth metadata

## Integration Shape

Treat ChatGPT as two layers, not one:

### Layer 1: hosted MCP app

This is already close.

- live MCP endpoint
- OAuth metadata
- dynamic registration
- public `search` and `fetch` compatibility tools

### Layer 2: richer Apps SDK experience

This is a separate product step on top of the MCP server if we want richer in-ChatGPT UI:

- inline panes
- structured presentation beyond plain tool text
- richer output flows for plots, tables, and artifacts

Official references:

- `https://developers.openai.com/apps-sdk/`
- `https://developers.openai.com/apps-sdk/mcp-apps-in-chatgpt`
- `https://developers.openai.com/apps-sdk/build/chatgpt-ui`

## What Still Needs To Be True Before Submission

- final ChatGPT-specific compatibility pass using the exact current OpenAI app flow
- review of tool descriptions and metadata for marketplace-safe wording
- final anonymous quota and premium entitlement hardening
- final privacy, support, and failure-mode copy for public listing pages
- screenshots, company description, and support contact packaging
- decision on whether we submit as a strong hosted MCP app first or wait for a minimal Apps SDK UI layer

## Tool Story For Submission

Public tools to emphasize first:

- `search`
- `fetch`
- `verify_citation`
- `search_literature`
- `find_related_papers`

Tools that should remain authenticated or paid:

- `summarize_evidence`
- `run_deepscan`
- `get_deepscan_status`
- `get_deepscan_report`
- `run_python_plot`

Positioning:

- public value: research search, citation checks, and related-paper discovery
- authenticated value: account-linked workflows
- paid value: heavier research runs, summaries, plotting, and DeepScan

## Required Assets

- short company description for PapersFlow
- short MCP/app description
- support email or support URL
- privacy policy URL
- terms of service URL
- screenshots or a short walkthrough of the app approval flow
- one simple “how to connect” support article

## Testing Before Submission

1. Run a real production install flow from a fresh client registration.
2. Approve the request through PapersFlow Settings.
3. Exchange the auth code for tokens.
4. Call `tools/list`.
5. Call `search` and `fetch`.
6. Confirm expected tool gating for non-public scopes.
7. Confirm denial flow and revoked-token behavior.
8. Confirm public-tool descriptions read cleanly in a ChatGPT app context.
9. Decide whether plots and richer outputs will be links first or handled with an Apps SDK UI layer.

## Submission Risk Notes

- “MCP compatible” does not guarantee “approved by the directory”
- tool descriptions must be conservative and review-friendly
- quota and entitlement messaging must be explicit enough to avoid user confusion
- plain MCP is enough for tool access, but not automatically the strongest in-ChatGPT product experience
