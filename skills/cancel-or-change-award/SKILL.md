---
name: cancel-or-change-award
description: "Works out the cancel or change fee, where the points go back, and if a rebook is cheaper than keeping the ticket. Use when the user wants to cancel, change or rebook an award ticket. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Cancel or change an award

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `search_monthly_availability`, `update_flight_booking`, `delete_flight_booking`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Find the booking

- Call `list_flight_bookings`. Match by confirmation number, route or date.
- Call `get_flight_booking` for the programme, points spent and taxes.
- Ask only for what is missing: the booking programme is the most important fact.

## 2. Fee

- Call `get_cancellation_fee` for the airline.
- The fee is for the airline's own programme. If the ticket was booked through a partner programme, say that the partner's rules apply and the fee can differ.
- Check the elite exemptions in the result against `list_loyalty_statuses`.

## 3. Where the points go

- Points return to the programme that issued the ticket, not to the bank currency they came from.
- Taxes are refunded to the card, less the fee, when the fee is taken from taxes.

## 4. Change instead of cancel

- If the user wants new dates, call `search_all_airlines` or `search_monthly_availability` for the new dates.
- Compare: fee + new points cost vs. keeping the ticket.
- If the new award is cheaper than the old one, rebooking can save points even after the fee.

## 5. Update records

- After a change, call `update_flight_booking`. After a cancel, call `delete_flight_booking`.

## Output format

1. Fee and who is exempt.
2. Points and taxes returned, and to where.
3. Rebook option with the net cost, if the user wants new dates.
4. Steps to cancel or change (online or by phone).
