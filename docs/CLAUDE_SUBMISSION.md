# Claude Submission Notes

This file is the company-facing checklist for Anthropic Connector Directory submission of `papersflow-mcp`.

## Current Position

What is already live:

- public MCP endpoint: `https://doxa.papersflow.ai/mcp`
- OAuth metadata and protected-resource discovery
- OAuth approval flow in PapersFlow Settings
- dynamic client registration
- PAT setup for direct Claude Code usage
- live production smoke tests with Claude Code
- production / GA hosted MCP service for the current public research surface

What this means:

- Claude Code users can already connect to the hosted server
- the production endpoint is suitable for Connector Directory review
- the main remaining work is submission polish, screenshots, and final reviewer-account prep

## What Still Needs To Be True Before Submission

- one final hosted install test using the exact Claude connector flow we intend to support
- final wording pass on connector description, scopes, and support copy
- polished onboarding screenshots from Settings approval to successful connection
- final entitlement and anonymous quota hardening
- support and operational ownership notes for the company

## Recommended Initial Surface

Lead with the narrow public tools:

- `verify_citation`
- `search_literature`
- `find_related_papers`

Keep these gated until we are fully comfortable:

- `summarize_evidence`
- `run_deepscan`
- `get_deepscan_status`
- `get_deepscan_report`
- `run_python_plot`

## PAT Vs OAuth Guidance

- Claude Code local and project setups can use PATs today
- connector and marketplace-style installs should use OAuth
- do not ask normal end users to hand-edit raw configs if a cleaner approval flow exists

## Required Assets

- short PapersFlow company description
- short Claude connector description
- support URL or support email
- privacy policy URL
- terms URL
- screenshots of the approval flow
- short troubleshooting guide
- dedicated reviewer account with sample data

## Submission Links

- privacy: `https://papersflow.ai/privacy`
- terms: `https://papersflow.ai/terms`
- support: `https://papersflow.ai/contact`
- support email: `developer@papersflow.ai`

## Example Prompts

1. `Find 5 papers on retrieval-augmented generation evaluation and return the title, year, and DOI for each.`
2. `Verify this citation and return the normalized metadata plus an APA citation: Attention Is All You Need, Vaswani et al., 2017.`
3. `Find papers related to "Attention Is All You Need" and group them into similar papers, references, and later citations.`

## Testing Before Submission

1. Create a fresh dynamic client registration.
2. Start the authorization request.
3. Approve it from PapersFlow Settings.
4. Exchange the auth code.
5. Confirm `tools/list`.
6. Confirm a real `search_literature` call.
7. Confirm a denied approval case.
8. Confirm a revoked-token case.

## Submission Risk Notes

- working in Claude Code is necessary but not sufficient for marketplace approval
- the OAuth flow needs to look stable and intentional, not experimental
- plan-based tools must be clearly described so reviewers understand what is public and what is paid
