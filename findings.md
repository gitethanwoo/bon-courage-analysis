# Petit Merci — Analysis Findings

**Business:** Petit Merci, provisions store on Montagu St in Charleston, SC (retail-first — no seating, no cafe permit)
**Data:** 18,638 item-level transaction rows, 6,809 tickets, 137 operating days (Jul 2025 – Mar 2026)
**Source:** Shift4 Lighthouse API, ticket-detail-closed endpoint
**Primary data file:** `ticket-detail-all.csv`

---

## 1. Business Profile

- **Operating days:** Wed–Sat only (Mon/Tue/Sun closed)
- **Hours:** ~7:45am–3:30pm (virtually zero transactions after 4pm on this POS)
- **Total revenue (period):** ~$183K across 137 days
- **Annualized run rate:** ~$278K (based on 4-day operating week)
- **Average revenue/operating day:** ~$1,335
- **Revenue by day (median):** Sat $1,685 > Fri $1,617 > Wed $1,389 > Thu $1,212
- **POS login:** Single employee code ("Bryan") used for all transactions — does not reflect actual staffing

## 2. Business Model Pivot

The data reveals a structural shift mid-period:
- **Early months (Jul–Sep 2025):** ~10.5 tickets/day at ~$160/ticket (bulk/wholesale-style)
- **Recent months (Jan–Mar 2026):** ~42 tickets/day at ~$28/ticket (individual retail customers)
- Traffic grew ~300% while ticket values dropped ~82%
- Daily revenue stayed roughly flat through this transformation

*Evidence: ticket count and avg ticket value by month from ticket-detail-all.csv*

## 3. Revenue Breakdown by Category

| Category | Revenue | % of Total |
|---|---|---|
| Coffee/Espresso | ~$34,500 | 17.2% |
| Retail Food & Pantry | ~$24,000 | 12.0% |
| Frozen/Prepared Meals | ~$21,700 | 10.9% |
| Bakery (Focaccia, Cookies, Muffins) | ~$21,300 | 10.7% |
| Sandwiches | ~$18,900 | 9.4% |
| Wine & Beer | ~$13,600 | 6.8% |
| Retail Merchandise | ~$11,700 | 5.8% |
| Surcharges/Dual Pricing | ~$8,300 | 4.1% |
| Gift Cards | ~$6,000 | 3.0% |

*Evidence: item categorization from skytab-items-by-timeslot.csv (full-period aggregate)*

## 4. Peak Hours & Time Blocks

| Time Block | % of Revenue |
|---|---|
| Lunch (11am–1pm) | 33.0% |
| Morning (9am–11am) | 32.6% |
| Afternoon (1pm–4pm) | 27.4% |
| Early morning (before 9am) | 7.0% |

- **10am–12pm** is a flat revenue plateau generating 53.4% of daily revenue
- **8am hour** runs at less than half the peak rate
- **After 2pm** drops off steeply

*Evidence: skytab-items-by-timeslot.csv time slot aggregates, hourly-sales CSV*

## 5. Product Concentration

- **22 items (3.4% of 639 SKUs) = 50% of revenue**
- 143 items = 80% of revenue
- Bottom 320 items (50% of catalog) = only 7.4% of revenue ($13,556)
- **162 items sold exactly once** over the entire period
- 8 daily staples appear 80%+ of operating days: Latte, Add Syrup, Chocolate Chip Cookie, Matcha Latte, Cappuccino, Turkey Sandwich, Ham & Brie, Americano

*Evidence: item frequency and revenue analysis from ticket-detail-all.csv*

## 6. Top Items

| Item | Revenue | Daily Velocity |
|---|---|---|
| Latte | ~$17,540 | ~21 units/day |
| Surcharges/Dual Pricing | ~$8,300 | auto |
| Turkey Sandwich | ~$6,700 | consistent |
| Baked Ziti W/ Meatballs | ~$5,100 | take-home |
| Matcha Latte | ~$4,300 | ~3 units/day |
| Focaccia - Petit | ~$3,100 | ~2/day |
| Chocolate Chip Cookie | ~$3,100 | ~8/day |
| Ham & Brie | ~$3,000 | consistent |

*Evidence: ticket-detail-all.csv item aggregation*

## 7. Coffee Program

- 5,851 coffee drinks sold, avg $5.89/drink
- **Latte = 51% of all coffee revenue**
- Syrup attach rate: **45.1%** (Add Syrup on same ticket as coffee)
- Peak coffee hour: 9–10am
- Matcha Latte successfully repriced from $6 → $8 with minimal volume loss — **proves price elasticity tolerance**

*Evidence: ticket-detail-all.csv co-occurrence analysis, price history from item records*

## 8. Basket Analysis

- **64.4% of coffee tickets have NO food item**
- When food IS added to a coffee ticket, avg ticket jumps from $14.57 → $38.96 (+$24.40)
- 30% of all tickets are drinks-only, averaging $7.91
- Only 4.9% of tickets include a take-home frozen meal, but those tickets average $95.62 (3.1x store average)
- 73% of sandwich tickets have no cookie/dessert

*Evidence: ticket-detail-all.csv basket co-occurrence, ticket composition analysis*

## 9. Customer Segments (inferred from ticket composition)

| Segment | % of Tickets | % of Revenue | Avg Ticket |
|---|---|---|---|
| Quick Grab (drinks only) | ~30% | ~6% | $7.91 |
| Moderate (coffee + 1-2 items) | ~30% | ~13% | ~$17 |
| Big Basket (meal + retail) | ~27% | ~30% | ~$40 |
| Premium (events/bulk/gifting) | ~11% | ~51% | ~$150+ |

- **Two transaction profiles:** quick coffee/snack grab (majority of tickets, minority of revenue) vs intentional provisions shopping trip (minority of tickets, majority of revenue). The data confirms the provisions store model — coffee drives foot traffic, but retail is the business.

*Evidence: ticket value distribution clustering from ticket-detail-all.csv*

## 10. Seasonal Patterns

- **December** is peak: $1,682/day avg (2.2x normal). Week of Dec 15 hit $10,276.
- **Valentine's 2026** was a deliberate, successful seasonal push: 1 SKU/$317 (2025) → 16 SKUs/$1,991
- Gift cards spike in December (24 of 50 total loads)
- Christmas/Fall seasonal items generated ~$5,300 in 2025
- Seasonal coffee drinks (Frosty, Gingerbread Man, Le Fluffernut) contributed ~$2,500

*Evidence: monthly revenue from ticket-detail-all.csv, seasonal item analysis from sales-summary CSVs*

## 11. Pricing, Margins & Discounts

### Margin Structure
Petit Merci uses a simple **100% markup on all products** (buy at $30, sell at $60) except:
- **Beer:** ~60% markup ($5 cost → $8 retail)
- **Coffee:** margin unknown (likely ~70-80% gross margin, industry standard for espresso drinks)
- **Surcharges/gift cards:** 100% margin (no COGS)

### Estimated Gross Profit

| Category | Revenue | Est. COGS | Gross Profit | GP % |
|---|---|---|---|---|
| All products (100% markup) | $131,829 | $65,915 | $65,915 | 50% |
| Coffee (**margin unknown**) | $37,638 | ? | ? | ? |
| Beer (60% markup) | $921 | $575 | $345 | 37.5% |
| Surcharges/Gift Cards | $12,523 | $0 | $12,523 | 100% |
| **Total** | **$182,911** | | **$78,783 + coffee** | |

**Known gross profit (excluding coffee): ~$78,783** over the period, ~$120K annualized. Coffee adds $37,638 in revenue on top of that, but we don't know the margin — it depends on bean cost, milk, cups, labor intensity. Worth calculating: Diana knows what she pays per bag of beans and per gallon of milk, so the per-drink COGS is straightforward to estimate.

**What we do know:** Every $1 of incremental provisions revenue is worth $0.50 in gross profit. Surcharges and gift cards are pure margin. Beer is the worst margin at 37.5%.

### Discounts
- Total discounts: ~$3,000 (1.62% of gross) — disciplined
- Discount rate tightened from 1.9% (2025) → 1.0% (2026 YTD)
- PM Merchandise (t-shirts, sweatshirts, tumblers) has highest discount rate at 21–37% — at 50% base margin, a 20% discount wipes out nearly half the gross profit on these items
- **No menu prices changed between periods** — all improvement is operational
- 18 of top 20 items have never been price-tested
- Matcha Latte $6→$8 is the one natural experiment — it worked

*Evidence: margin structure confirmed by owner, discount fields in ticket-detail-all.csv, price comparison across sales-summary CSVs*

## 12. Growth Trends

- Revenue per operating day is **flat** over 9 months (~-$9/month trend)
- But ticket count is growing at +5.1 tickets/day/month
- Rising stars: Cookie Asst. (+171%), Card Amy Zhang (+223%), Ranger Station Candle (new, $1,080)
- Fading: Open Retail declining (good — more items properly categorized)

### Retail Merch Growth: Corrected Analysis

Earlier analysis claimed +98% retail merch growth post-Babas. **This is misleading.** Deep dive reveals:

**Core merch (excluding 5 breakout products) by phase:**

| Phase | Core Merch $/day | Breakout $/day | Total Merch $/day | Merch % of Total |
|---|---|---|---|---|
| Summer (Jul-Sep) | $90 | $0 | $90 | 6.8% |
| Oct (pre-Babas) | $124 | $0 | $124 | 8.3% |
| Nov (pre-Babas) | $215 | $0 | $215 | 16.3% |
| Dec (Babas opens mid-month) | $219 | $37 | $255 | 13.7% |
| Jan (post-holiday) | **$51** | $60 | $111 | 10.3% |
| Feb | $109 | $68 | $177 | 13.0% |
| Mar (partial) | $92 | $71 | $163 | 14.4% |

**What's actually driving the growth:**

1. **Seasonality is the primary driver.** Core merch surged Oct-Nov ($124→$215/day) BEFORE Babas opened, driven by holiday gift buying. January crashed to $51/day. This is a seasonal curve, not structural growth.

2. **Five breakout products mask the decline.** These items contribute $60-71/day post-Babas:
   - Ranger Station Candle: $1,080 (brand new, 27 units)
   - Card Amy Zhang: $655 (brand new, 131 units)
   - PM Tumbler: +$500 increase ($120→$620)
   - Merci t-shirt: +$388 increase ($24→$412)
   - PM Sweatshirt: $383 (new in post period)
   Without these, post-Babas merch would look *worse* than Oct-Nov baseline.

3. **Merch share of total IS genuinely rising** (6.8% summer → 13-14% Feb-Mar). But partly because coffee and sandwiches are shrinking (Babas stealing those), not because merch is booming organically.

**Corrected interpretation:** The "retail is the moat" thesis is still directionally right — it's the one category Babas can't touch. But the growth engine is **new product introductions**, not organic demand for existing products. The implication: keep launching new merch aggressively (that's what works), but don't assume the existing catalog will grow on its own.

*Evidence: item-level phase analysis from ticket-detail-all.csv, controlling for 5 breakout items and seasonal patterns*

## 13. Operational Notes

- Void rate: 0.72% — exemplary, no waste/training issue
- "Open" items (Open Food, Open Retail, etc.) declining from 11.5% → 5.7% of revenue — better POS discipline
- Surcharge/dual pricing steady at ~4% of revenue, avg surcharge rising ($1.14 → $1.23)

*Evidence: item_status field in ticket-detail-all.csv, Open item trends from sales-summary CSVs*

---

## Priority Actions (ranked)

| # | Action | Projected Impact | Cost |
|---|---|---|---|
| 1 | Food upsell prompt on coffee tickets (64% have no food) | +$15K/yr | $0 |
| 2 | Seasonal product drops (replicate Valentine's playbook 4-5x/yr) | +$15-20K/yr | Inventory |
| 3 | Price test $0.50 increases on 8 daily staples | +$3-5K/yr | $0 |
| 4 | Prune 150+ zombie SKUs (50% of catalog = 7% of revenue) | Save $5-9K/yr | $0 |
| 5 | Expand Retail Merch (fastest growing category, high margin) | +$5-10K/yr | Inventory |
| 6 | Investigate Thursday gap ($473/day below Saturday median) | TBD | TBD |

---

## 14. College of Charleston Impact

CofC is located adjacent to Petit Merci. Academic calendar 2025-2026:
- Fall classes: Aug 19 – Dec 1. Fall break Oct 13-14. Thanksgiving Nov 26-30. Finals Dec 3-8.
- Winter break: Dec 9 – Jan 3.
- Spring classes: Jan 7 – Apr 22. Spring break Mar 1-8.

**Finding: Students drive foot traffic, NOT revenue (r = -0.071, p = 0.42, not significant).**

| Period | Tickets/Day | Revenue/Day | Avg Ticket |
|---|---|---|---|
| Classes in session | 52.7 | $1,547 | $29.38 |
| Students off campus | 48.1 | $1,663 | $34.55 |

Students bring +9% more customers but spend -18% less per ticket. Net revenue impact is a wash.

**Calendar events that DO matter:**
- **Thanksgiving Eve (Nov 26):** $4,779 — 2nd best day ever. Holiday entertaining/gifting.
- **Pre-Christmas (Dec 10-23):** $2,365/day — best sustained stretch, zero students on campus.
- **Move-in Saturday (Aug 16):** $2,637 — families exploring the neighborhood.
- **Fall finals week:** $2,620/day avg but only 44 tickets (parents visiting, gift-buying).
- **Spring break:** Only -5% dip. Barely noticeable.

**Seasonal revenue ranking (avg daily):**
Fall Finals $2,620 > Thanksgiving $2,440 > Move-in $2,005 > Winter Break $1,758 > Fall Classes $1,532 > Summer $1,499 > Spring Classes $1,421 > Spring Break $1,323.

**Verdict:** Petit Merci is not a student-dependent business. It's a provisions store serving locals, tourists, and seasonal gift shoppers.

*Evidence: CofC 2025-2026 academic calendar from charleston.edu/registrar, daily revenue from ticket-detail-all.csv*

## 15. Weather Impact

Weather data from Open-Meteo archive API for Charleston, SC (CHS) merged with daily sales.

**Finding: Weather is not a primary revenue driver.**

- **Rain vs dry:** $1,556 vs $1,513 avg daily revenue. No significant difference (t-test p = 0.74).
- **Temperature vs revenue:** r = 0.042 (p = 0.63) — no linear correlation.
- **Temperature vs ticket count:** r = 0.385 (p < 0.0001) — warmer days bring more people.
- **Temperature vs avg ticket value:** r = -0.255 (p = 0.003) — cooler days drive higher spend per visit.
- Net effect: hot days = more people who spend less; cold days = fewer people who spend more. Cancels out.

**Temperature bands:**

| Band | Days | Avg Revenue | Avg Tickets | Avg Ticket $ |
|---|---|---|---|---|
| 40-50F | 6 | $953 | 25.7 | $30.42 |
| **50-60F** | **27** | **$1,666** | **42.4** | **$40.04** |
| 60-70F | 32 | $1,424 | 44.2 | $32.43 |
| 70-80F | 38 | $1,644 | 58.2 | $28.54 |
| 80-90F | 30 | $1,503 | 56.4 | $27.89 |
| 90F+ | 4 | $1,316 | 48.5 | $27.57 |

- **Below 50F is the only danger zone** — foot traffic drops enough to meaningfully hurt ($953/day).
- **50-60F is the sweet spot** — highest revenue because ticket values peak at $40/ticket.
- Rain shifts product mix toward prepared food (+2.8pp), pastries (+0.9pp), and retail/gifts (+1.5pp).

**Verdict:** Seasonality and day-of-week matter 10x more than weather. Don't cancel promotions for rain.

*Evidence: Open-Meteo historical API, merged dataset at daily-sales-weather.csv*

## 16. Competitor Impact — Babas on Wentworth

**Babas Charleston** (https://babascharleston.com/) opened their third location at 115½ Wentworth St — one block from Petit Merci — on **December 16, 2025** (not October as initially estimated).

**Babas profile:**
- European-inspired neighborhood cafe chain, 3 Charleston locations
- 24K Instagram followers, press in Conde Nast Traveler, Forbes, Charleston Magazine
- Hours: Tue-Thu 7a-8p, Fri-Sat 7a-10p, Sun 7a-6p
- Menu: espresso ($2), pastries, sandwiches (turkey, ham & butter), salads ($10-14), cocktails, wine
- Rating: ~4.6-4.7/5 across platforms
- **Near-total competitive overlap** on coffee, pastries, and sandwiches

**Revenue impact (controlling for PM's wholesale-to-retail pivot, retail-only orders <$100):**

| Metric | Pre-Babas (before Dec 16) | Post-Babas | Change |
|---|---|---|---|
| Avg daily revenue | $1,129 | $958 | **-15.2%** |
| Avg daily tickets | ~50 | ~39 | **-20%** |

**Categories hit hardest:**
- Sandwiches: **-32.4%** (-$92/day) — direct menu overlap
- Coffee/Espresso: **-20.0%** (-$52/day) — Babas undercuts on price ($2 espresso)
- Matcha Latte: **-37%** — Babas known for matcha with house-made peanut milk
- Turkey Sandwich: **-33%**, Ham & Brie: **-24%** — identical items on Babas menu
- Chocolate Chip Cookie: **-29%**

**Category GROWING despite Babas:**
- **Retail/Market goods:** merch share of total revenue rose from 6.8% → 13-14%. However, this is driven primarily by 5 breakout product introductions (Ranger Station Candle, Card Amy Zhang, PM Tumbler, Merci t-shirt, PM Sweatshirt) and seasonal holiday buying — not organic growth of existing merch. Core merch (excluding breakouts) fell from $215/day in Nov to $51-109/day in Jan-Mar. See Section 12 for full corrected analysis. Babas does not sell retail goods.

**Monthly trajectory:**
- January 2026 was the shock month ($1,155/day, worst month) — first full month after Babas opened.
- February 2026 recovered to $1,560/day — customers settling into visiting both, not fully defecting.

**Verdict:** Babas is stealing coffee and sandwich traffic but cannot compete on provisions — curated retail food, merchandise, frozen meals, wine. That's Petit Merci's core business and moat.

*Evidence: Babas website, Post and Courier opening announcement, Yelp reviews, daily revenue split from ticket-detail-all.csv*

---

## Revised Priority Actions

| # | Action | Rationale | Impact |
|---|---|---|---|
| 1 | **Accelerate new merch launches** | Core merch doesn't grow organically — growth comes entirely from new product introductions (Ranger Station Candle, Card Amy Zhang, PM Tumbler drove $2,700+ combined). Keep the pipeline moving: new candles, cards, branded items every 4-6 weeks. | +$10-15K/yr |
| 2 | **Differentiate food menu from Babas** | Babas is stealing sandwiches (-32%) and coffee (-20%). Compete on items they DON'T have: frozen meals, specialty retail food, French-specific items. Stop trying to win on commodity coffee/sandwiches. | Defend $30K+/yr |
| 3 | **Own the holiday/gifting calendar** | Dec is 2.2x normal revenue and student-independent. Thanksgiving Eve = $4,779. Nov merch was $215/day vs $90/day in summer — holiday buying is real and repeatable. Plan 5-6 seasonal moments/year. January is a cliff ($51/day core merch) — plan a post-holiday promotion. | +$15-20K/yr |
| 4 | **Food upsell on coffee tickets** | 64% leave without food. Prompt at register. | +$15K/yr |
| 5 | **Price test staples** | Matcha $6→$8 worked. Babas charges $2 for espresso so don't compete on price — compete on experience + quality. | +$3-5K/yr |
| 6 | **Prune 150+ zombie SKUs** | 50% of catalog = 7% of revenue. Free shelf space for new merch launches (the actual growth engine). | Save $5-9K/yr |

---

## Next Investigations

- [ ] **Instagram posting** — do posts/stories correlate with next-day or same-day traffic?
- [x] ~~College schedule~~ — not a significant driver (Section 14)
- [x] ~~Weather~~ — not a significant driver (Section 15)
- [x] ~~Competitor (Babas)~~ — significant impact on coffee/sandwiches (Section 16)

---

## Data Files

| File | Description |
|---|---|
| `ticket-detail-all.csv` | 18,638 item rows, every transaction Jul 2025–Mar 2026 |
| `daily-sales-weather.csv` | Daily revenue merged with weather data (137 days) |
| `skytab-items-by-timeslot.csv` | Item sales by 15-min time slot (full period aggregate) |
| `hourly-sales-1774127383705.csv` | Hourly revenue by date |
| `sales-summary-combined.csv` | Item summaries with Period column (2025 / 2026 YTD) |
| `sales-summary-by-item-1774126612429.csv` | 2025 full year item summary |
| `sales-summary-by-item-1774126787960.csv` | 2026 YTD item summary |
| `sales-summary-by-item-1774127740940.csv` | Combined period item summary |
