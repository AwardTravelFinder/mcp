---
description: Compare loyalty program award charts and rates
argument-hint: "[program1] [program2] ..."
allowed-tools:
  - mcp__awardtravelfinder__get_program_rates
  - mcp__awardtravelfinder__get_pricing
---

# Compare Loyalty Programs

The user wants to compare loyalty program award charts across the 23 programs Award Travel Finder supports.

## Arguments

The user invoked this command with: $ARGUMENTS

Parse arguments for program names or slugs. If none provided, suggest the most common comparisons (Avios vs Aeroplan, Flying Blue vs Virgin, etc.) and ask which to compare.

Available program slugs:
`british-airways`, `iberia`, `qatar-airways`, `aer-lingus`, `virgin-atlantic`, `aeroplan`, `flying-blue`, `cathay-pacific`, `singapore-airlines`, `emirates`, `etihad`, `turkish-airlines`, `ana`, `jetblue`

## Process

1. Use `get_program_rates` for each requested program.
2. If a specific route is mentioned, also run `get_pricing` to anchor the comparison to real numbers for that route.
3. If a region is mentioned, filter results to that area.
4. Present a comparison table: Destination/Region | Cabin | Program A points + taxes | Program B points + taxes
5. Highlight which program offers the best value for each cabin class.
6. Note off-peak vs peak pricing differences and fuel surcharge gotchas (BA and Virgin tend high; Aeroplan, Asia Miles, Flying Blue tend lower or zero on partner metal).

## Examples

```
/atf:compare-programs british-airways emirates
/atf:compare-programs aeroplan flying-blue virgin-atlantic
/atf:compare-programs cathay-pacific ana for HKG-NRT
```
