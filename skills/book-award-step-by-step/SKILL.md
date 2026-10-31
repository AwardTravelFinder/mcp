---
name: book-award-step-by-step
description: "A safe booking checklist - confirm live space, hold if possible, transfer, then ticket. Never transfer before you see the seat. Use when the user found an award and is ready to book, or asks \"how do I book this?\". Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Book an award step by step

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `compare_transfer_options`, `add_flight_booking`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

Rule 1: never tell the user to transfer points before the seat shows live on the booking programme's website. Transfers cannot be reversed.

## 1. Confirm the award

- Identify the flight, date, cabin, passengers and the booking programme.
- If a trip id from a search is present, call `get_award_trip_details` for flight numbers and the booking link.
- If the programme is unknown, call `get_partner_award_options` and pick the cheapest one the user can reach.

## 2. Check the points

- Call `list_points_balances`.
- If the programme balance is short, call `compare_transfer_options` with the target programme and points needed.
- Check `whats_on_sale` for a transfer bonus. Do not wait for a bonus if seats are scarce.

## 3. Hold rules

- Load `get_skill` with key award-holds.
- Programme can hold: tell the user to place the hold first, then transfer.
- Programme cannot hold: tell the user to search the seat on the programme site, then transfer only instant-transfer currencies. For slow transfers, warn that the seat can disappear.

## 4. Pre-transfer checklist

Tell the user to check each item:

1. The seat shows on the programme website for all passengers.
2. The name on the loyalty account matches the passport.
3. The transfer ratio and time (from step 2).
4. Taxes and carrier surcharges are acceptable.
5. The cancellation fee. Call `get_cancellation_fee` for the airline.

## 5. Transfer and ticket

- Transfer the exact points needed, plus a small buffer.
- Book as soon as the points post.

## 6. After booking

- Offer `add_flight_booking` with the confirmation number.
- Offer `fs_find_best_seats` to pick a seat.

## Output format

A numbered checklist the user can follow. Put the hold rule and the "do not transfer yet" warning at the top in bold.
