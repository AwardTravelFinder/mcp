---
name: set-up-alerts
description: "Sets up award-seat monitors, hotel price-drop tracking and recurring standing orders in one pass. Use when the user wants to be told when seats open, prices drop, or deals appear. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Set up alerts

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `list_standing_orders`, `list_hotel_bookings`, `monitor_hotel_price`, `create_standing_order`, `cancel_standing_order`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Check what exists

- Call `list_route_monitors`, `list_standing_orders` and `list_hotel_bookings`.
- Do not create a duplicate. Offer `update_route_monitor` to change an existing one.

## 2. Pick the alert type

- Award seats on a route: `create_route_monitor` with origin, destination, cabin, earliest date, latest date, passengers and an optional points cap.
- Price drop on a booked hotel: `monitor_hotel_price`. Follow the hotel-price-drop-rebook skill if the booking is not saved.
- Anything recurring or open-ended (e.g. "every Sunday, check transfer bonuses for my currencies"): `create_standing_order`. Write its prompt as a full instruction that names the tools to run.
- A better seat on a booked flight: `fs_create_seat_alert`.

## 3. Check the booking window

- For dates more than 300 days away, call `get_award_release_window` and tell the user when seats open.

## 4. Confirm limits

- Accounts have a limit on route monitors. If the call reports the limit, tell the user.

## Output format

A list of the alerts now active: type, what it watches, how often it checks, where alerts go (email). Offer to cancel with `cancel_route_monitor` or `cancel_standing_order`.
