---
description: Search award flight availability across airlines
argument-hint: "[airline] [from] [to] [date-or-month]"
allowed-tools:
  - mcp__awardtravelfinder__search_availability
  - mcp__awardtravelfinder__search_monthly_availability
  - mcp__awardtravelfinder__search_all_airlines
  - mcp__awardtravelfinder__search_hybrid
  - mcp__awardtravelfinder__get_pricing
---

# Search Award Flights

The user wants to search for award flight availability across the 28 airlines supported by Award Travel Finder.

## Arguments

The user invoked this command with: $ARGUMENTS

Parse the arguments to extract:
- **Airline (optional)**: name or IATA code (e.g. BA, QR, CX, VS, IB, EK, QF, AS, AA, B6, EY, JL, AC, NH, SQ, TK, AF, KE, F9). If omitted, use `search_all_airlines` to fan out.
- **From**: Departure airport (city name or IATA code)
- **To**: Arrival airport (city name or IATA code)
- **Date**: Specific date (YYYY-MM-DD) or month (YYYY-MM)

Map common airline names to slugs (e.g. `british_airways`, `qatar_airways`, `cathay_pacific`, `virgin_atlantic`, `iberia`, `emirates`, `qantas`, `alaska_airlines`, `american_airlines`, `jetblue`, `etihad`, `frontier`).

## Process

1. If the user didn't specify an airline, run `search_all_airlines` for the date and route to discover what's bookable.
2. Run `get_pricing` to show the points cost across cabin classes.
3. If a specific date is given, use `search_availability`. If a month is given, use `search_monthly_availability`. If no date, ask the user.
4. For premium-cabin long-haul where cash might beat points, optionally suggest `search_hybrid` to compare cash vs split-ticket award options.
5. Present results in a clean table: Date | Airline | Cabin | Seats | Points Required | Taxes/Fees
6. Highlight the best value options.

## Examples

```
/atf:search-flights BA LHR JFK 2026-04-15
/atf:search-flights qatar DOH SIN 2026-05
/atf:search-flights JFK NRT 2026-11-14         # no airline → search_all_airlines
/atf:search-flights virgin LHR JFK 2026-06-15
/atf:search-flights qantas SYD LAX 2026-07
```
