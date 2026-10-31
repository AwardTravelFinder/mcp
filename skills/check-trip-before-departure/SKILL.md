---
name: check-trip-before-departure
description: "A pre-flight briefing - aircraft and seat, lounges, wifi, airport delays and security waits, and a reprice check on the award. Use when the user has a flight in the next 2 weeks, or asks \"am I ready for my trip?\". Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Check my trip before departure

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `get_trip_briefing`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Find the flight

- If the user gave no flight, call `list_flight_bookings` and pick the next departure.
- Call `lookup_flight` with the flight number to confirm the airline. It returns carrier identity only, not times, aircraft or status.
- Get origin and destination from the booking. Ask the user only if they are still unknown.

## 2. Briefing

- Call `get_trip_briefing` with flight number, date, origin and destination. It returns aircraft and seatmap data, plus airport delay and security-wait snapshots.
- Live flight status (departure time, gate, delay for this flight) is not part of these tools. If a live flight status tool is available, use it. If not, tell the user to check the airline app on the day.

## 3. Seat

- Call `fs_find_best_seats` for the flight and cabin. Compare with the seat on the booking, if known.
- If the user wants a better seat, offer `fs_create_seat_alert`.

## 4. Lounges

- Call `list_loyalty_statuses` to learn the user's status.
- Call `lounge_get_airport_lounges` for the departure airport.
- Call `lounge_find_lounges_by_access` when the user has a card or status that grants access.

## 5. Wifi

- If the flight number and date are known, call `sw_get_wifi_facts` for the flight and date. Otherwise call `sw_get_flight_wifi` for the flight.
- Quote the answer sentence and the data_as_of date.
- Say a flight uses Starlink only when the verdict is starlink or mostly_starlink. The confidence_level field is data coverage, not certainty.
- If the verdict is unknown or confidence_level is none, say there is not enough data yet. Do not guess a provider.

## 6. Reprice check

- Call `search_all_airlines` for the same route, date and cabin.
- If the same award now costs fewer points, tell the user. Call `get_cancellation_fee` so they know the cost of rebooking.

## Output format

A short briefing with headings: Flight, Seat, Lounges, Wifi, Airport, Reprice. One or two lines each. Put any action (seat change, rebook) at the top.
