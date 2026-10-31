---
name: plan-award-trip
description: "From a route and dates to a bookable award, with the cheapest transfer path, a hold plan and booking steps. Use when the user names a trip (route, month or dates) and wants to know how to fly it on points. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Plan an award trip

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `search_monthly_availability`, `search_multi_passenger`, `search_hybrid`, `plan_trip`, `compare_transfer_options`, `add_flight_booking`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

Goal: one recommended award the user can book today, plus one fallback.

## 1. Collect the inputs

You need origin, destination, dates, cabin and passengers.

- Read the session preamble first. If it is empty or stale, call `get_portfolio`.
- Use `list_points_balances` to learn which currencies the user holds.
- Ask the user only for inputs that are still unknown. Ask all of them in one message.
- If the user does not give a passenger count, use 1.
- Cabin: if the user does not name one, ask. If the user says "any cabin" or "cheapest", search economy and show business as a second option.

## 2. Check the booking window

- If the date is more than 300 days away, call `get_award_release_window` for the likely carriers.
- If seats are not loaded yet, say when they open and offer a monitor (step 7).

## 3. Search

- Exact date: call `search_all_airlines` with flex_days 3.
- Month or flexible dates: call `search_monthly_availability` for the 2-3 most likely airlines.
- 2 or more passengers: use `search_multi_passenger` instead, so single-seat results drop out.
- Premium cabin with no good award: call `search_hybrid` for a cash + award split.
- If `plan_trip` is available for the account, it runs search + ranking in one call. Use it first, then verify with the tools above.

## 4. Pick the best option

- Rank by total cost: points x cents-per-point + taxes. Call `get_points_valuation` for each currency you compare.
- Flag a known sweet spot. Load `get_skill` with key award-sweet-spots when you are not sure.
- For a partner flight, call `get_partner_award_options` to find every programme that can ticket it.
- When the search result has no taxes, call `estimate_award_fees` for the route and cabin. Say the taxes are estimates.
- Call `compare_cash_vs_points` for the best option. If cash is cheaper than the points value, say so.

## 5. Find the transfer path

- Call `compare_transfer_options` with the target programme and the points needed.
- If the user has no direct partner, call `find_transfer_paths`.
- Check `whats_on_sale` for an active transfer bonus to that programme.

## 6. Plan the hold and booking

- Load `get_skill` with key award-holds. Say if the programme can hold the seat.
- Never tell the user to transfer points before they see the seat live on the programme website.
- For booking links and flight numbers, call `get_award_trip_details` when a trip id is present.

## 7. Offer follow-ups

- Seats are tight or not open: offer `create_route_monitor`.
- The user books: offer `add_flight_booking` so the trip is tracked.

## Output format

1. **Best option**: airline, flight, cabin, date, points + taxes, programme to book with.
2. **How to pay**: transfer path, ratio, bonus, transfer time.
3. **Booking steps**: numbered, with the hold rule.
4. **Fallback**: one other option.
5. **Next step**: one offer (monitor or save booking).
