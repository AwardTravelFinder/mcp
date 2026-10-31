---
name: family-or-group-trip
description: "Finds award seats for 2+ travellers together and splits the points cost across balances and accounts. Use when the user travels with 2 or more people on points. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Plan a family or group award trip

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `search_multi_passenger`, `search_monthly_availability`, `compare_transfer_options`; also plan-dependent: `create_client_portfolio`, `generate_branded_pdf`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Collect the inputs

- Need origin, destination, dates, cabin and passenger count. Ask only for what is unknown.
- Call `list_points_balances`. Ask if other travellers have their own balances or household pooling.

## 2. Search for seats together

- Call `search_multi_passenger` with the passenger count. It drops single-seat results.
- For a month, call `search_monthly_availability` and keep only days with enough seats.
- No result for the full group: search for a split (e.g. 2 + 2 on two flights the same day), and say so.

## 3. Split the cost

- Points needed = points per person x passengers.
- Call `compare_transfer_options` with the total points needed.
- If one balance is short, combine balances: transfer from 2 currencies, or use a household pool.
- Check `whats_on_sale` for a transfer bonus or a buy-points sale. Call `get_buy_points_pricing` to cost a top-up.

## 4. Book safely

- Load `get_skill` with key award-holds.
- Book all passengers on one reservation when one programme pays. Transfer points before you book only when the seats show live for all passengers.

## 5. Follow-ups

- Offer `create_route_monitor` with the passenger count when seats are short.
- For advisors: save the plan with `create_client_portfolio` and offer `generate_branded_pdf`.

## Output format

1. Best option: flight, seats available, points per person, total points, taxes.
2. Split: who pays what from which balance.
3. Booking steps.
4. Fallback split option.
