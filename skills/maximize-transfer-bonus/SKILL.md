---
name: maximize-transfer-bonus
description: "Decides if a live transfer bonus is worth using now, what to book with it and how many points to move. Use when the user asks about a transfer bonus, or a bonus to one of their programmes is live. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Maximise a transfer bonus

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `search_monthly_availability`, `compare_transfer_options`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

Rule: a bonus is only good if there is a redemption to spend the points on. Never transfer speculatively into a programme with no use in sight.

## 1. Find the bonus

- Call `whats_on_sale`. Find the bonus the user named, or list all live transfer bonuses for currencies they hold.
- Call `list_points_balances` to see which bonuses apply to the user.

## 2. Find a use for the points

- Call `get_award_chart_faq` for the target programme to find its sweet spots.
- If the user has a trip in mind, call `search_monthly_availability` or `search_all_airlines` for it.
- Call `list_flight_bookings` to see upcoming trips the bonus can pay for.

## 3. Do the math

- Call `compare_transfer_options` with the target programme and the points the redemption needs. It includes active bonuses.
- Call `get_points_valuation` for the source currency and the target programme.
- Effective value = target value x (1 + bonus) compared with the source value. Recommend the transfer only if it is higher and there is a use.

## 4. Decide how much to move

- Move only what the identified redemption needs, rounded up to the transfer increment.
- If seats are not confirmed, follow the book-award-step-by-step skill before transferring.

## Output format

1. Verdict: use it now / wait / skip, in one line.
2. The redemption it pays for, with points and taxes.
3. The math: source points in, target points out, cents per point before and after.
4. Bonus end date and the steps to transfer.
