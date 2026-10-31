---
name: missing-points-claim
description: "Audits a hotel folio or stay for missing points, elite benefits and bad charges, then writes the claim. Use when the user shares a hotel bill, or says points or benefits are missing after a stay. Requires the Award Travel Finder MCP connector (https://mcp.awardtravelfinder.com/mcp)."
version: 1.0.0
---

# Claim missing points and dispute a folio

The tools named below are on the Award Travel Finder MCP connector. If they are not available, tell the user to connect https://mcp.awardtravelfinder.com/mcp (setup: https://awardtravelfinder.com/developers/mcp).

> **Plan:** may not be included in every account plan: `list_hotel_bookings`, `audit_marriott_folio`, `audit_hotel_folio`. If a tool reports that it is not included in the account's plan, tell the user and use the other steps where the skill gives one.

## 1. Collect the folio

- Ask the user to paste the folio text if it is not in the chat.
- Call `list_loyalty_statuses` for the user's elite tier with the chain. Ask only if it is missing.
- Call `list_hotel_bookings` to match the stay and confirmation number.

## 2. Audit

- Marriott: call `audit_marriott_folio` with folio text, stay type, status tier and confirmation number.
- Hilton, Hyatt or IHG: call `audit_hotel_folio` with the chain.
- Keep only findings the tool flags. Do not invent charges.

## 3. Missing points

- The typical posting delay varies by programme; many say 2-6 weeks. Check the chain's own rule before you call the points missing, and say when the user can file a claim.
- Estimate the missing points from the paid room rate and the chain's base earn plus the elite bonus. Say it is an estimate.

## 4. Write the claim

- Use the dispute template from the audit result.
- Add the missing-points request with dates, confirmation number and the folio total.

## Output format

1. Flagged lines: charge, amount, why it is disputable.
2. Missing points estimate.
3. The claim message, ready to send, in a code block.
4. Where to send it (chain's missing-stay form or the hotel).
