# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this workspace is

This is **not a software codebase** — there is no source code, build system, or tests. It is a
research/analysis workspace for producing probability-based forecasts of **GOLD (XAU/USD)**.

The entire workflow is driven by one artifact:

- `weekly_analyze_gold_market.md` — the **master analyst prompt/spec**. It is not documentation to
  read passively; it is the instruction set that defines exactly how every forecast in this
  workspace must be produced and structured. Treat it as the source of truth for scope and output
  format. When asked to "analyze the gold market" or produce a forecast, follow this file's 14
  sections in order.

## The task (what "run" means here)

There is nothing to build, lint, or test. "Doing work" = gathering the latest market data with the
web tools, then writing a forecast that conforms to `weekly_analyze_gold_market.md`. Concretely:

1. Pull current data via `WebSearch` / `WebFetch`: XAU/USD spot (+ today's high/low, daily/weekly/
   monthly change), DXY, US 2Y & 10Y Treasury yields, real yields, Fed rate-decision odds
   (CME FedWatch), CPI/PPI/NFP/PCE, crude oil, central-bank buying, gold ETF flows, COMEX
   positioning, and live geopolitical developments (US–Iran/Gulf/Hormuz, Russia/Ukraine, China/Taiwan).
2. Produce the forecast covering all 14 sections of the spec.

## Non-negotiable conventions (from the spec)

- **Gold unit = GRAM.** Quote all gold prices in grams as the primary unit (USD/g and SAR/g).
  XAU/USD is quoted per troy ounce, so convert with `1 ozt = 31.1034768 g`; the per-ounce figure
  may appear in parentheses for reference.
- **Second deliverable — `investment suggestion.md`.** Every forecast must also produce this file:
  actionable buy/sell recommendations broken down **by date** and **by week** (action ratings,
  entry/stop/target, a staged accumulation ladder, risk management), prices in grams, with separate
  notes for a long-term physical buyer vs. a short-term trader. See spec §15.
- **Probabilities, not certainty.** Never write "gold will definitely rise/fall." Give
  bullish/neutral/bearish probabilities for 24h / 7d / 30d, and always state what would invalidate
  the forecast.
- **Facts vs. assumptions must be explicitly separated**, and every important data point must cite
  its **source + date/time**. If sources disagree, call out the discrepancy rather than averaging it.
- **Don't reason mechanically.** E.g. do not assume geopolitical tension ⇒ gold up; weigh
  safe-haven demand against USD strength, higher yields, and inflation, and state which effect
  currently dominates.
- **Required closings:** end with a `MY BASE CASE` block (direction / confidence / ranges / key
  catalysts & levels / invalidators) followed by a `TRADER'S SUMMARY` of **no more than 10 bullets**.
- **Saudi physical-gold section** uses fixed constants: `USD/SAR ≈ 3.75`, `1 troy ounce =
  31.1034768 grams`. Convert 1 oz / 10 g / 20 g / 1 tola to SAR and note dealer premium/spread.

## Tooling & permissions

- `.claude/settings.local.json` pre-authorizes `WebSearch` and `WebFetch` for `litefinance.org` and
  `vantagemarkets.com`. Fetching other domains will prompt for permission — expect that, and add
  domains there if a source becomes routine.
- `litefinance.org` has returned HTTP 403 to `WebFetch` in the past; prefer `WebSearch` snippets or
  alternate sources (Kitco, Investing.com, FXStreet, TradingView, World Gold Council, RoboForex)
  when a direct fetch is blocked.
- This directory is **not a git repository**. There is no commit/PR workflow unless the user
  initializes one.
