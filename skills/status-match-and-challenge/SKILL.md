---
name: status-match-and-challenge
description: "Finds the cheapest way to an elite tier - match, challenge, earn or run - from the status the user already holds. Use when the user wants elite status, asks about a status match, or holds status that is about to expire. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Status match and challenge

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `recommend_status_path`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Know the current status

- Call `list_loyalty_statuses`. If it is empty, ask the user for their airline and hotel statuses and save each with `set_loyalty_status`.
- Call `get_status_progress` for tier progress and requalification dates.

## 2. Find matches

- Call `get_status_matches`. Keep offers where the user holds an eligible source status.
- If the user named a target, call `recommend_status_path` with the target programme and tier.

## 3. Compare paths

For each path, give cost, time and risk:

- Match: free or a fee, fast, often one per lifetime.
- Challenge: fly X segments or earn Y points in Z days.
- Earn: planned flying. Call `calculate_flight_earnings` for booked or planned trips.
- Run: a tier-point run. Say the cost in cash.

## 4. Timing

- Tell the user to match close to the start of a qualification year, so the status lasts longer.
- Warn that most programmes allow one match per lifetime.

## Output format

1. Recommended path in one line.
2. Table: path, cost, time, what the user gets.
3. Steps to apply, with the apply link from the match offer.
