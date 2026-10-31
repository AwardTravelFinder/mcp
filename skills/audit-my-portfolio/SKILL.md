---
name: audit-my-portfolio
description: "Values every balance, flags idle and expiring points, and gives the best use for each currency. Use when the user asks \"what are my points worth?\", \"what should I do with my points?\" or wants a check-up. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Audit my points portfolio

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `update_points_balance`, `list_hotel_bookings`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Load the portfolio

- Call `get_portfolio`, then `list_points_balances`.
- If there are no balances, ask the user for them and save each with `update_points_balance`.

## 2. Value each balance

- Call `get_points_valuation` for each currency.
- Total value = balance x cents per point. Sort from highest to lowest.

## 3. Flag risk

- Expiring: balances with an expiry date in the next 6 months.
- Idle: a programme with no activity and no upcoming trip that uses it.
- Stranded: a small balance below the cheapest award. Call `get_award_chart_faq` for the cheapest award in that programme.

## 4. Best use per currency

- Transferable currencies: call `find_transfer_paths` to the 2 strongest partners for the user's home airport.
- Airline and hotel programmes: use `get_award_chart_faq` for sweet spots.
- Check `whats_on_sale` for a live bonus that changes the answer.

## 5. Upcoming trips

- Call `list_flight_bookings` and `list_hotel_bookings`. Suggest which balance pays for each unbooked part.

## Output format

1. Total portfolio value.
2. Table: programme, balance, cents per point, value, status (active / idle / expiring / stranded).
3. Top 3 actions, most urgent first.
4. Offer: set up alerts with the set-up-alerts skill.
