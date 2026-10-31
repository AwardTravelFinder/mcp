---
name: setup
description: "Connects the user's Award Travel Finder account, saves their home airport, points and cards, and runs a first live award search. Use when the user says \"set up award travel finder\", \"get started\", \"connect my points\" or is new to the plugin. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Set up Award Travel Finder

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `update_points_balance`, `create_route_monitor` beyond the first monitor. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

Goal: a connected account, saved profile data and one real award result in a few minutes.

## Rules

- The user signs in themselves. Never ask for a password, token or API key.
- Never open a sign-in link for the user.
- Ask one question set, in one message.

## 1. Check the connection

- Call `get_profile`.
- If the call fails, or the user is signed out, tell the user to approve the sign-in in their browser. Say that the sign-in takes about 30 seconds and needs no card.
- Wait for the user to confirm. Then call `get_profile` again.
- If the call still fails, point the user to https://awardtravelfinder.com/developers/mcp and stop.
- Call `list_points_balances` and `list_my_cards`. Do not ask for data that is already saved.

## 2. Ask one question set

Send one message that asks for:

- Home airport (name or code).
- The points programmes they hold, with balances.
- The credit cards they hold.
- A destination, if they have one (optional).
- Whether they want an alert when award seats open on a route.

Tell the user they can answer in part, and that screenshots work.

## 3. Save the answers

- Save each balance with `update_points_balance`.
- Save each card with `add_card`. If the card name is not clear, call `list_catalogue_cards` to find it.
- If the user has screenshots, use `import_balances_from_image` for balances and `import_cards_from_image` for cards.
- If the user has a browser agent, offer the read-only capture skills. Call `get_skill` with `capture-from-airline-hotel-account` for balances and status. Call `get_skill` with `capture-cards-and-bonuses` for cards and bonuses. The user signs in themselves.
- Say what you saved. Do not guess a value the user did not give.

## 4. Run one first win

- Use the home airport as the origin.
- If the user named a destination, call `search_availability` for that route.
- If not, follow the where-can-my-points-take-me skill. Call `explore_destinations` from the home airport, then confirm live seats for the best reachable destination with `search_all_airlines`.
- Show the best result: route, date, programme, cabin, points and taxes.
- Say how the user pays. Call `find_transfer_paths` if the points sit in a transferable currency.
- If there are no seats, say so. Try the next destination. Do not invent a result.

## 5. Offer an alert

- If the user wants alerts, ask once to confirm the route, cabin and date range.
- Call `list_route_monitors` first. Do not create a duplicate.
- Call `create_route_monitor` for the route.
- If the call reports a limit, tell the user.

## Output format

End with exactly 3 lines:

1. Connected: the signed-in account and plan.
2. Saved: the balances and cards you saved, and the home airport you searched from.
3. Next: the one thing to ask now, for example "Say a destination and I will run the full plan."
