---
name: weekly-deal-review
description: "A weekly digest of live transfer bonuses, points sales and route-monitor hits that match the user's balances. Use when the user asks \"any good deals this week?\", or wants a regular review. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Weekly deal review

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `search_monthly_availability`, `create_standing_order`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Load context

- Call `list_points_balances` and `list_route_monitors`.
- Call `list_flight_bookings` for upcoming trips.

## 2. Collect deals

- Call `whats_on_sale` for transfer bonuses and buy-points sales.
- Keep only deals for currencies the user holds, or programmes their monitors and trips use.
- For each buy-points sale, call `get_buy_points_pricing` and compare with `get_points_valuation`. Flag a sale only when the price per point is below the valuation for a known use.

## 3. Route monitors

- For each active monitor, call `search_monthly_availability` or `search_all_airlines` for its window.
- Report new seats only.

## 4. Status matches

- Call `get_status_matches`. Mention offers where the user holds an eligible status.

## Output format

Four short sections: Transfer bonuses, Points sales, Seats on your routes, Status matches. One line each item with the action. End with an offer to automate this review with `create_standing_order` (cadence weekly).
