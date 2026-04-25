# AliExpress Israel Skill

Planning repo for a Claude Code skill that handles AliExpress shopping from an Israel-based buyer's perspective.

> **Status**: notes / requirements only. Eventual aim is to roll this into the **Israel shopping plugin**. This repo just captures what the skill needs to do before it gets folded in.

## Purpose

A skill that helps evaluate, search, and buy from AliExpress with Israel-specific considerations baked in:

- Shipping availability and realistic delivery windows to Israel
- Customs / VAT thresholds (currently 75 USD de minimis for VAT, 500 USD for customs duty — verify at runtime)
- ILS conversion for listed prices
- Vendor reliability filtering (ratings, store age, dispute rates)
- Local-vs-import comparison hooks (tie into the Israel shopping plugin's local market check)

## Intended scope

- `find-product` — search AliExpress for a product spec, return Israel-shippable results
- `evaluate-listing` — given a URL, summarise: total landed cost in ILS (item + shipping + likely tax), seller trust signals, delivery ETA to IL
- `compare-to-local` — diff against local IL retailers (Ksp, Bug, Ivory, etc.) once integrated with the Israel shopping plugin
- `track-order` — pull order/tracking status (if API access available)

## Open questions / what's needed

- AliExpress API access — affiliate/open platform vs. scraping (Playwright fallback)
- Currency rate source (already have one in shared MCP/skills)
- Whether to share a vendor-trust schema with the broader shopping plugin
- IL customs threshold values should be a single source of truth — pull from kol-zchut or hardcode + dated review

## Integration target

Will be merged into the Israel shopping plugin (sibling to existing Israel agent skills). Keep skill names/paths plugin-friendly from the start.
