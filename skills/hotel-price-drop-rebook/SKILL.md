---
name: hotel-price-drop-rebook
description: "Tracks a booked award hotel stay, and rebooks when the points rate falls below what the user paid. Use when the user has a refundable award hotel booking, or asks \"can I save points on my hotel?\". Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Rebook a hotel when the points price drops

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `list_hotel_bookings`, `list_hotels`, `get_hotel`, `monitor_hotel_price`, `create_standing_order`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

Tracking covers points rates only. Do not promise to track a cash rate.

## 1. Find the booking

- Call `list_hotel_bookings`. If the stay is there and monitoring is on, it is already tracked. Go to step 2 and do not create it again.
- If the stay is not there, collect: hotel, check-in and check-out dates, total points paid, room type, rate plan (e.g. standard award) and confirmation number. Ask only for what is missing.
- Call `list_hotels` or `get_hotel` to resolve the hotel code.

## 2. Check the current points price

- Call `get_hotel` for the dates and compare with the points paid.
- A drop is real only for the same room type, rate plan and cancellation terms.

## 3. Decide

- Fewer points now and the booking is refundable: tell the user to book the new rate first, then cancel the old one.
- Non-refundable: tell the user a rebook is not possible, and do not start tracking.
- Same price: start tracking (step 4).

## 4. Track

- Call `monitor_hotel_price` once, with hotel code, check-in date, check-out date, original points (total for the stay), room type, rate plan and confirmation number. It saves the booking and starts daily checks, so do not add the same stay again with a separate booking tool.
- Offer `create_standing_order` for a weekly summary of all tracked stays.

## Output format

1. Current points rate vs. points paid, and the saving.
2. Action: rebook now / tracking started / not possible.
3. Rebook steps: book new, confirm, then cancel old.
