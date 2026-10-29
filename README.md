# Award Travel Finder — Claude Code Plugin & MCP Server

Search award flight availability, compare loyalty programs, and find the best points redemptions from your AI assistant. This plugin connects to the [Award Travel Finder](https://awardtravelfinder.com) MCP server, which exposes 127 award-travel tools over a single hosted endpoint.

**Works with Claude Code, Claude Desktop, Cursor, Windsurf, ChatGPT, Grok, Antigravity, and any MCP-compatible client.**

## Quick start

```
/plugin marketplace add AwardTravelFinder/mcp
/plugin install award-travel-finder@award-travel-finder
/award-travel-finder:setup
```

Other clients (Codex, Cursor, ChatGPT, Gemini): tell your agent: Read https://awardtravelfinder.com/setup.md and follow it to set up Award Travel Finder.

## Quick install

### Claude Code plugin (recommended)

```
/plugin marketplace add AwardTravelFinder/mcp
/plugin install award-travel-finder@award-travel-finder
```

### Gemini CLI / Antigravity

```
gemini extensions install https://github.com/AwardTravelFinder/mcp
```

### Any other MCP client

Point your client at the endpoint:

```
https://mcp.awardtravelfinder.com/mcp
```

For clients that take a config file:

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

Clients with native remote-MCP support (Cursor, Windsurf, ChatGPT developer mode, Grok custom connectors) can use the URL directly without `mcp-remote`.

Per-client setup guides: [awardtravelfinder.com/mcp](https://awardtravelfinder.com/mcp).

## Authentication

The server uses **OAuth 2.1** as the MCP spec describes. On your first tool call the client opens your browser to sign in — about 30 seconds, no card. There is no API key.

> **Do not set an `Authorization` or `X-API-Key` header.** API-key authentication was retired on 2026-06-07. A leftover static header overrides the OAuth token the client obtained, and every request then fails with 401 even though the sign-in appeared to succeed. If you followed an older version of these instructions, delete the `headers` block from your config.

A free account covers everyday trip planning. [Premium](https://awardtravelfinder.com/pricing) unlocks unlimited searches and every cabin.

## What you can do

Ask naturally — the bundled `award-travel` skill activates on its own when you mention award flights, points, miles, or an airline:

```
"Find business class from London to New York using Avios"
"Are there any award seats on Cathay Pacific HKG-LHR next month?"
"Which program has the cheapest business class to Tokyo?"
"How many Avios do I need for London to New York in business?"
```

Or use the slash commands:

```
/atf:search-flights BA LHR JFK 2026-06-15
/atf:compare-programs british-airways emirates
```

## Task skills

Step-by-step playbooks that run on the Award Travel Finder tools. Claude picks the right one on its own, or you can ask for it by name. Each one needs the Award Travel Finder MCP connector. The same skills show up as MCP prompts (slash commands) in any client that supports prompts. Full list: [awardtravelfinder.com/mcp/skills](https://awardtravelfinder.com/mcp/skills).

| Skill | What it does |
|-------|--------------|
| `setup` | Connect your account, save your points and cards, run a first award search |
| `plan-award-trip` | Route and dates to a bookable award, with transfer path, hold plan and booking steps |
| `where-can-my-points-take-me` | 3-5 award trips you can afford with the points you hold |
| `book-award-step-by-step` | Safe booking checklist: confirm space, hold, transfer, ticket |
| `maximize-transfer-bonus` | Is a live transfer bonus worth using, and how many points to move |
| `check-trip-before-departure` | Pre-flight brief: seat, lounges, wifi, airport waits, reprice check |
| `hotel-price-drop-rebook` | Track an award hotel stay and rebook when the points rate drops |
| `missing-points-claim` | Audit a hotel folio and write the missing-points or dispute claim |
| `status-match-and-challenge` | Cheapest path to an elite tier: match, challenge, earn or run |
| `audit-my-portfolio` | Value every balance and flag idle or expiring points |
| `next-credit-card` | Pick the next travel card from your spend and goals |
| `family-or-group-trip` | Award seats for 2+ travellers and how to split the points |
| `weekly-deal-review` | Weekly digest of bonuses, points sales and seats on your routes |
| `set-up-alerts` | Route monitors, hotel price tracking and standing orders |
| `cancel-or-change-award` | Cancel or change fee, where points go, and whether to rebook |
| `find-my-bookings-and-balances` | Find bookings and points balances through your own email and browser connectors |

## Coverage

**28 airlines** can be searched one at a time with `search_availability`:

British Airways, Qatar Airways, Cathay Pacific, Virgin Atlantic, Iberia, Emirates, Qantas, Alaska Airlines, American Airlines, JetBlue, Frontier, Southwest, Delta, Japan Airlines, United, TAP Portugal, Cape Air, Icelandair, Condor, Aer Lingus, El Al, Turkish Airlines, ANA, Lufthansa, Air France, KLM, Etihad, Smiles (GOL).

**11 of those** are in the `search_all_airlines` fan-out, which sweeps them together in one call: British Airways, Cathay Pacific, Virgin Atlantic, Iberia, Qatar Airways, Alaska Airlines, JetBlue, Frontier, Southwest, Qantas, Smiles (GOL). Award inventory is genuinely sparse, so a route with no results usually means no seats rather than a broken search.

**23 loyalty programs** have award-chart data via `get_program_rates`:

`british-airways`, `qatar-airways`, `singapore-airlines`, `cathay-pacific`, `emirates`, `etihad`, `flying-blue`, `aeroplan`, `turkish-airlines`, `ana`, `jetblue`, `jet2-emirates`, `virgin-atlantic`, `iberia`, `alaska-airlines`, `american-airlines`, `qantas`, `delta-skymiles`, `united`, `lifemiles`, `marriott-bonvoy`, `hilton-honors`, `hyatt`.

**4 hotel programs** for award nights: Marriott, Hilton, IHG, Hyatt.

## Tools

127 tools in total. The most-used ones:

| Tool | What it does |
|------|--------------|
| `search_availability` | Award seats for one airline, route and date |
| `search_all_airlines` | Sweep the 9 fan-out airlines in one call |
| `search_monthly_availability` | A whole month, day by day |
| `search_hybrid` | Cash-and-points combinations |
| `get_pricing` | Award-chart pricing for a route |
| `get_program_rates` | The full award chart for a program |
| `get_points_valuation` | What a currency is worth |
| `find_transfer_paths` | Routes from your cards into a program |
| `compare_transfer_options` | Which transfer gets you there cheapest |
| `get_status_matches` | Elite status match opportunities |
| `search_hotels` / `get_hotel_availability` | Award nights across the 4 hotel programs |
| `get_portfolio` / `list_points_balances` | Your tracked balances |
| `add_flight_booking` / `list_flight_bookings` | Your trip log |

Run `tools/list` against the endpoint for the full catalogue — it answers without authentication, so you can inspect the server before signing in.

## Repository structure

This repo is the public home for every Award Travel Finder AI and MCP surface — the Claude Code plugin, the MCP server manifest we publish to the official registry, and the metadata directories index.

```
mcp/
  server.json            # Official MCP registry manifest
  gemini-extension.json  # Gemini CLI / Antigravity extension manifest
  .claude-plugin/
    plugin.json          # Plugin metadata
    marketplace.json     # Marketplace listing
  .mcp.json              # MCP server connection config
  logo.png               # 400x400 marketplace logo
  llms-install.md        # Agent-readable setup steps
  commands/
    search-flights.md    # /atf:search-flights
    compare-programs.md  # /atf:compare-programs
  skills/
    award-travel/        # Activates on award-travel questions
    plan-award-trip/     # ...and the 15 task skills listed above
  openclaw/
    SKILL.md             # OpenClaw-flavoured variant of the same skill
```

The MCP server itself is a hosted service at `mcp.awardtravelfinder.com`; this repo holds the manifests and client-facing docs, not the server implementation.

## Links

- [Award Travel Finder](https://awardtravelfinder.com) — the web app
- [Setup guides for every client](https://awardtravelfinder.com/mcp)
- [Transfer bonus tracker](https://awardtravelfinder.com/transfer-bonuses)
- [Pricing](https://awardtravelfinder.com/pricing)

## Support

Open an issue here, or email [support@awardtravelfinder.com](mailto:support@awardtravelfinder.com).

## License

MIT
