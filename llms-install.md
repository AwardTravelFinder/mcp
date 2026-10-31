# Installing Award Travel Finder — instructions for AI agents

You are setting up the Award Travel Finder MCP server for a user. Follow these
steps exactly.

## 1. Add the server

Award Travel Finder is a **hosted remote MCP server**. Nothing is installed
locally, there is no build step, and there are no dependencies to fetch.

Endpoint:

```
https://mcp.awardtravelfinder.com/mcp
```

Transport: **streamable HTTP**. The server is request/response only — it does not
push server-initiated messages, so a standalone `GET /mcp` correctly returns 405.
That is expected; do not treat it as a failure.

If the client accepts a remote URL directly (Cline, Cursor, Windsurf, ChatGPT
developer mode, Grok custom connectors), use the URL as-is. If the client only
accepts a stdio command, bridge it:

```json
{
  "mcpServers": {
    "awardtravelfinder": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.awardtravelfinder.com/mcp"]
    }
  }
}
```

## 2. Do NOT add any authentication headers

This is the single most common setup failure.

- Do **not** add an `Authorization` header.
- Do **not** add an `X-API-Key` header.
- Do **not** ask the user for an API key. There is no API key. API-key auth was
  retired on 2026-06-07.

The server uses OAuth 2.1 and advertises it through RFC 9728 protected-resource
metadata at `/.well-known/oauth-protected-resource`. The client discovers this
on its own from the `WWW-Authenticate` challenge and runs the flow. Dynamic
client registration (RFC 7591) is supported, so no client ID needs configuring.

A static `Authorization` header overrides the OAuth token the client obtained.
The sign-in appears to succeed and then every request returns 401 in a loop. If
the user reports that symptom, the fix is to delete the header from their config.

## 3. Verify before handing back

Call `tools/list`. It answers **without authentication**, so you can confirm the
connection works before the user signs in. A healthy server returns ~86 tools.

Then have the user run one real query, for example:

```
Find business class award seats from London to New York in June
```

The first tool call opens the user's browser to sign in. That takes about 30
seconds and needs no payment card. After they authorize, the call completes.

## 4. Set expectations

- Some tools may not be included in every account plan. If a tool reports that
  it is not included, tell the user and continue with the other steps.
- Award inventory is genuinely sparse. An empty result usually means there are no
  seats on that route and date, not that the search failed. Suggest nearby dates
  or a different airline rather than retrying the identical query.
- 28 airlines can be searched individually; `search_all_airlines` sweeps 11 of them
  at once. Prefer `search_all_airlines` when the user has not named an airline.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Repeated 401 after a successful sign-in | A leftover static `Authorization` or `X-API-Key` header | Remove the `headers` block |
| 403 `insufficient_scope` | Token lacks `mcp:read` | Re-run the OAuth flow |
| 405 on `GET /mcp` | No SSE stream is offered | Expected — the client continues POST-only |
| `invalid_client` or an auth loop | Stale registration cached by `mcp-remote` | Clear `~/.mcp-auth/` and reconnect |
