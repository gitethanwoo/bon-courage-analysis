# Petit Merci — POS Data Analysis

A real-world sales analysis of [Petit Merci](https://www.instagram.com/petitmercichs/), an independent provisions store in Charleston, SC. Built in a single session using Claude Code to reverse-engineer POS APIs, extract transaction data, and generate strategic insights.

## What happened here

Petit Merci runs on Shift4's SkyTab POS, which has limited export options (some reports are PDF-only). We reverse-engineered the Lighthouse API from a browser HAR capture, wrote extraction scripts, and pulled 21,100 item-level transaction rows spanning 9 months. Then we ran 7 parallel analysis agents competing to find the best insight, layered in weather data, college schedules, and competitor research, and iterated on the findings until the numbers were honest.

The whole process — from "can you combine these two CSVs?" to a 16-section strategic analysis — took about 2 hours.

## Key findings

- **$398K annualized revenue**, ~$243K gross profit at ~61% blended margin
- **Coffee is the highest-margin product (75-83%)** but provisions (retail food, merchandise, wine, frozen meals) are 46% of revenue and the core business
- **A competitor opened one block away** in Dec 2025 — coffee and sandwich sales dropped 20-32%, but provisions grew because the competitor can't replicate that
- **Weather doesn't matter the way you'd think** — rainy/overcast days actually outperform clear sunny days (people go to the beach instead of shopping)
- **College students are a wash** — they add foot traffic but spend 18% less per ticket. Holiday gifting drives 2x more revenue than any academic period
- **New manager saved ~$2,900/yr** by cracking down on unauthorized friend/employee discounts
- **162 items sold exactly once** — half the catalog generates 7% of revenue

## Technical approach

1. **Data extraction**: Reverse-engineered Shift4 Lighthouse REST API (`ticket-detail-closed` endpoint) from HAR file. JWT auth, simple POST with date range. Also reverse-engineered SkyTab BI's `SlsMix.aspx` AJAX API for time-slot data.
2. **Analysis**: 7 parallel Claude Code subagents (basket analysis, time optimization, product performance, customer behavior, employee ops, pricing strategy, growth trajectory) competing for best insight.
3. **External data**: Open-Meteo weather API (free, no key), College of Charleston academic calendar, competitor web scraping.
4. **Iteration**: Multiple rounds of correcting our own mistakes — the +98% retail growth was really seasonal + 5 breakout products, the $30K competitor impact was really $25K, and the original dataset was missing 40,000 rows due to an API filter.

## Files

| File | Description |
|---|---|
| [`findings.md`](findings.md) | The full analysis — 16 sections with evidence citations |
| [`findings.html`](findings.html) | Formatted version for printing/PDF |
| [`ticket-detail-all.csv`](ticket-detail-all.csv) | 21,100 transaction rows (employee names anonymized, gift card numbers stripped) |
| [`daily-sales-weather.csv`](daily-sales-weather.csv) | Daily revenue merged with Charleston weather data |
| [`hourly-sales-1774127383705.csv`](hourly-sales-1774127383705.csv) | Hourly revenue by date |
| [`skytab-items-by-timeslot.csv`](skytab-items-by-timeslot.csv) | Item sales by 15-min time slot |
| `sales-summary-by-item-*.csv` | Item-level summaries for 2025 and 2026 YTD |

## What's interesting about the process

Most small businesses are sitting on transaction data they can't access because their POS vendor makes export difficult. The pattern here — HAR capture, API reverse-engineering, automated extraction, parallel analysis — is repeatable for any restaurant or retail business on Shift4/SkyTab (thousands of locations). The technical details for replicating this are documented in the Claude Code memory files used during this session.

The analysis also shows how initial findings need to be stress-tested. Several headline numbers were wrong on the first pass and got corrected through follow-up investigation:

- "Retail merch is growing +98%" → actually seasonal + 5 new products
- "Wednesday is the best day" → outlier buyout inflated the mean
- "18,638 transactions" → API filter was hiding 40% of the data
- "Lattes are 75% margin" → true, but only after confirming bean cost ($0.54/drink) with the owner

## Built with

[Claude Code](https://claude.ai/code) (Claude Opus 4.6) — the entire analysis, from API reverse-engineering to the final findings document, was done in a single conversational session.
