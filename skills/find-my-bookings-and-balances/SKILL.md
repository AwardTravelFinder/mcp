---
name: find-my-bookings-and-balances
description: "Uses your own email and browser connectors together to find booking confirmations and points balances, then saves them to Award Travel Finder. Use when the user asks to find, sync or set up all their trips and balances and an email connector (Gmail, Outlook, Apple Mail) or browser control (Claude in Chrome, computer use) is available. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Find my bookings and points balances

> **Plan:** needs Premium: `import_booking`, `update_points_balance`, `import_balances_from_image`, `list_hotel_bookings`. If the user is on the free plan, tell them before you search.

> **Do not over-promise:** do not offer to schedule or do something later unless a real tool exists for it. Call `list_points_balances` before `update_points_balance` and reuse its program slugs.

Award Travel Finder cannot read the user's email or browser. You use the user's own connectors, then pass what you find to the tools below.

## Hard rules

- Never ask the user for a password, a one-time code, or security answers. Never type credentials into any page.
- The user signs in themselves. If a page shows a login form, skip that programme and tell the user.
- Read only. Do not book, cancel, change, redeem, transfer, buy, reply to, delete or move anything, in email or on any site.
- Do not paste email bodies or page text back to the user. Pass them only to the tools below.

## 1. Check what you can use

1. Look at your available tools. Note each email connector (Gmail, Outlook, Apple Mail) and each browser or computer control tool.
2. If you have neither, stop. Tell the user to connect an email or browser connector, or to upload screenshots for `import_balances_from_image`.
3. Call `list_points_balances`, `list_loyalty_statuses`, `list_flight_bookings` and `list_hotel_bookings`. This is the baseline. You report changes against it at the end.

## 2. Email pass (do this first: it is fast and needs no sign-in)

**Bookings.** Follow the search and import steps in the `import-bookings-from-inbox` skill. Import oldest first with `import_booking`.

**Balances.** Programmes send a balance in their monthly statement and account emails. Search the last 3 months, for example in Gmail:

- `subject:(statement OR "account summary" OR "your balance" OR "points balance" OR "miles balance" OR "activity") newer_than:3m`
- `from:(ba.com OR united.com OR aa.com OR delta.com OR virginatlantic.com OR flyingblue.com OR aircanada.com OR marriott.com OR hilton.com OR hyatt.com OR ihg.com OR americanexpress.com OR chase.com) subject:(balance OR statement) newer_than:3m`

For each programme, keep only the newest email that states a balance. Read the balance, the "as of" date, and the elite tier if the email shows them. Note the email date. Do not save yet.

## 3. Browser pass (fills the gaps)

Open the browser only for:

- Programmes with no balance email in the last 3 months.
- Programmes where the email balance is more than 30 days old.
- Elite status and tier progress, which emails rarely show.

Follow the steps and the account URLs in the `capture-from-airline-hotel-account` skill. A balance read on the live account page wins over an email balance.

## 4. Save

- Balance: `update_points_balance` with the newest value from step 2 or 3, and the expiry date when shown.
- Status tier: `set_loyalty_status`. Tier progress: `set_status_progress`.
- Bookings seen only in the browser: `import_booking` with the booking page text.
- If a page cannot be read as text, take a screenshot and call `import_balances_from_image`. Show the preview, then apply it only after the user agrees.

## 5. Summarise

Show one short table, then lists:

| Programme | Old balance | New balance | Source (email date or "account page") |
|---|---|---|---|

- Bookings added, grouped by trip.
- Changes and cancellations that wait for confirmation. Ask the user before you confirm any.
- Programmes you skipped and why (no email, not signed in, page unreadable).

Suggest next steps with real tools: `get_portfolio` for the total value, and `create_route_monitor` for an upcoming trip.
