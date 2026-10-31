---
name: award-travel-finder
description: Search award flight availability across 28 airlines and compare 23 loyalty program award charts via the Award Travel Finder MCP server. Find the best points and miles redemptions across British Airways, Qatar Airways, Cathay Pacific, Virgin Atlantic, Aeroplan, Iberia, Emirates, Qantas, Alaska, American, JetBlue, ANA, Singapore, Turkish, Etihad, Flying Blue, and more.
homepage: https://awardtravelfinder.com/claude
---

# Award Travel Finder

Search award flight availability and compare loyalty program award charts via the Award Travel Finder MCP server. 28 airlines, 23 loyalty programs, one prompt.

## Setup

Sign in with an awardtravelfinder.com account. On first tool call the client opens your browser to sign up via OAuth (~30 seconds).

### Claude Code (recommended)

```
/plugin marketplace add jackculpan/award-travel-finder-plugin
/plugin install award-travel-finder
```

### Claude Desktop / Cursor

Add to your MCP config:

```json
{
  "mcpServers": {
    "awardtravelfinder": {
      "type": "url",
      "url": "https://mcp.awardtravelfinder.com/mcp"
    }
  }
}
```

OAuth-aware clients sign you in on first call automatically. For clients without native OAuth, use `npx mcp-remote https://mcp.awardtravelfinder.com/mcp`.

### Do not send an API key

There is no API key. API-key authentication was retired on 2026-06-07. A static `Authorization` or `X-API-Key` header overrides the OAuth token the client obtained, so every request 401s even though sign-in appeared to succeed.

## Available Tools (21)

### Flight search

| Tool | Description |
|------|-------------|
| `search_availability` | Award seats for a specific route and date. Returns cabin-by-cabin availability. |
| `search_monthly_availability` | Calendar view for a whole month, one airline. Up to 90s. |
| `search_all_airlines` | Fan out a single date across every supported airline at once. |
| `search_hybrid` | Cash + points side-by-side, with hub/connection logic for split-ticket journeys. |
| `get_pricing` | Award chart for a route. Points per cabin class, off-peak and peak. |

### Loyalty programs

| Tool | Description |
|------|-------------|
| `get_program_rates` | Full award chart for a specific program — zones, cabin pricing, partner rules. |
| `get_buy_points_pricing` | Current "buy miles" promotions. |
| `get_status_matches` | Programs that status-match a given tier, and how long it lasts. |

### Hotels

`search_hotels`, `get_hotel_availability`, `monitor_hotel_price`, `list_hotel_bookings`, `get_hotel_booking`

### Trip portfolio (per user)

`add_flight_booking`, `list_flight_bookings`, `get_flight_booking`, `update_flight_booking`, `delete_flight_booking`, `update_points_balance`, `list_points_balances`, `get_portfolio`

## Airlines (19)

**Oneworld** — British Airways (BA), Cathay Pacific (CX), Iberia (IB), Qatar Airways (QR), American Airlines (AA), Alaska Airlines (AS), Qantas (QF), Japan Airlines (JL)

**SkyTeam** — Air France / KLM, Korean Air

**Star Alliance** — Singapore Airlines, ANA, Turkish Airlines, Aeroplan / Air Canada

**Non-aligned / partners** — Virgin Atlantic (VS), Emirates (EK), Etihad (EY), JetBlue (B6), Frontier (F9)

## Loyalty Programs (14)

| Program | Slug | Currency |
|---------|------|----------|
| British Airways | `british-airways` | Avios |
| Iberia | `iberia` | Avios |
| Qatar Airways | `qatar-airways` | Avios |
| Aer Lingus | `aer-lingus` | Avios |
| Virgin Atlantic | `virgin-atlantic` | Virgin Points |
| Aeroplan | `aeroplan` | Aeroplan Points |
| Flying Blue | `flying-blue` | Flying Blue Miles |
| Cathay Pacific | `cathay-pacific` | Asia Miles |
| Singapore Airlines | `singapore-airlines` | KrisFlyer Miles |
| Emirates | `emirates` | Skywards Miles |
| Etihad | `etihad` | Guest Miles |
| Turkish Airlines | `turkish-airlines` | Miles & Smiles |
| ANA | `ana` | Mileage Club |
| JetBlue | `jetblue` | TrueBlue Points |

## Usage Examples

### Search Specific Date

> Find BA business class availability from LHR to JFK on April 15th 2026

Uses `get_pricing` then `search_availability`.

### Search Flexible Dates

> Show me Qatar Airways availability from Doha to Singapore for all of May

Uses `search_monthly_availability`.

### Search Across Every Airline

> What's available JFK→NRT in business class on November 14th 2026?

Uses `search_all_airlines`.

### Multi-Program Comparison

> For LHR→HND in business, compare Avios, Aeroplan, Flying Blue, and Virgin Points. Include taxes and fuel surcharges.

Uses `get_pricing` and `get_program_rates`.

### Cash vs Points (Hybrid)

> SFO→LIS Oct 12 2026, business class. Show cash fare and the best hybrid split-ticket option.

Uses `search_hybrid`.

## Resources

- [Award Travel Finder for Claude Code](https://awardtravelfinder.com/claude) — Plugin landing page
- [Award Travel Finder](https://awardtravelfinder.com)
- [MCP Server Docs](https://mcp.awardtravelfinder.com)
