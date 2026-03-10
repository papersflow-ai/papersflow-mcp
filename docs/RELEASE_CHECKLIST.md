# papersflow-mcp Release Checklist

This is the execution checklist for moving `papersflow-mcp` from hosted alpha to a marketplace-ready release.

## 1. Settings UI QA

- Verify the `papersflow-mcp` tabbed section renders correctly on desktop.
- Verify the `papersflow-mcp` tabbed section renders correctly on mobile and narrow widths.
- Verify `Client setup` snippets can be copied without layout breakage.
- Verify `Tokens` tab allows PAT create, copy, and revoke.
- Verify `Connected apps` shows approved OAuth clients after a real approval flow.
- Verify `Release` tab matches the actual live entitlement model.
- Verify the OAuth approval redirect lands users on the integrations section with the MCP card visible.

## 2. Direct Client Acceptance Tests

### Codex

- Create a PAT in Settings.
- Use the `Client setup` snippet.
- Confirm `tools/list`.
- Confirm `search_literature`.
- Confirm `verify_citation`.
- Confirm a revoked PAT fails cleanly.

### Claude Code

- Create a PAT in Settings.
- Paste the config from `Client setup`.
- Confirm `tools/list`.
- Confirm `search_literature`.
- Confirm a wrong or revoked token fails cleanly.

### Gemini CLI

- Create a PAT in Settings.
- Paste the config from `Client setup`.
- Confirm `tools/list`.
- Confirm `search_literature`.
- Confirm a wrong or revoked token fails cleanly.

## 3. OAuth Acceptance Tests

- Register a new OAuth client at `/oauth/register`.
- Start an authorization request.
- Approve it from PapersFlow Settings.
- Exchange the auth code at `/oauth/token`.
- Confirm `tools/list`.
- Confirm a public tool call.
- Confirm denied-consent behavior.
- Confirm revoked access token behavior.
- Confirm stale registration access token behavior.

## 4. Entitlement And Scope Tests

- Guest access can only use public tools.
- Signed-in access without premium scopes cannot use research tools.
- Paid OAuth installs can use paid OAuth-safe tools.
- Paid OAuth installs cannot use PAT-only tools such as `run_python_plot`.
- Paid PAT installs can use PAT-only tools.
- Non-paid PAT installs are blocked from paid and research tools.
- Wrong-scope tokens fail with explicit `INSUFFICIENT_SCOPE`.
- Premium-required tools fail with explicit `UPGRADE_REQUIRED`.

## 5. Quota And Abuse Tests

- Confirm anonymous MCP quota behavior under repeated public calls.
- Confirm rate-limit response shape is stable and user-readable.
- Confirm repeated failed auth attempts do not produce unsafe errors.
- Confirm upstream timeout or provider-failure paths remain safe and bounded.

## 6. Production Runtime Checks

- Confirm `/.well-known/oauth-protected-resource` is correct and uses `https`.
- Confirm `/.well-known/oauth-authorization-server` is correct and uses `https`.
- Confirm `/oauth/jwks.json` is non-empty.
- Confirm `tools/list` works on the public endpoint.
- Confirm `search_literature` works on the public endpoint.
- Confirm invocation logging is written successfully in production.

## 7. Submission Prep

### Claude

- Final hosted OAuth install test with the exact connector path intended for submission.
- Screenshots of approval and successful tool use.
- Support and troubleshooting copy prepared.

### ChatGPT

- Final compatibility pass with the exact OpenAI app flow.
- Confirm `search` and `fetch` behave as expected.
- Listing copy, support info, privacy, and terms links prepared.

## 8. Launch Decision

Only call this release-ready when:

- the UI flow is self-explanatory
- PAT and OAuth both work end-to-end
- entitlement and quota behavior are predictable
- production telemetry is confirmed
- the marketplace submission assets are complete
