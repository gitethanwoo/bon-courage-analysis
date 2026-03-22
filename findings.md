# Petit Merci — Analysis Findings

**Business:** Petit Merci, provisions store on Montagu St in Charleston, SC (retail-first — no seating, no cafe permit)
**Data:** 18,638 item-level transaction rows, 6,809 tickets, 137 operating days (Jul 2025 – Mar 2026)
**Source:** Shift4 Lighthouse API, ticket-detail-closed endpoint
**Primary data file:** `ticket-detail-all.csv`

---

## 1. Business Profile

- **Operating days:** Wed–Sat (Tuesdays added Nov 2025, limited data so far)
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

Categorized to reflect Petit Merci's identity as a provisions store:

| Category | Revenue | % of Total | SKUs |
|---|---|---|---|
| Prepared Food & Bakery | $49,303 | 27.0% | 68 |
| **Provisions — Food & Merchandise** | **$41,103** | **22.5%** | **332** |
| Coffee Program | $37,928 | 20.7% | 24 |
| Frozen Meals | $14,799 | 8.1% | 16 |
| Open Items (uncategorized) | $13,419 | 7.3% | 5 |
| Surcharges & Gift Cards | $12,523 | 6.8% | 53 |
| Provisions — Wine | $10,302 | 5.6% | 92 |
| Provisions — Beverages | $2,473 | 1.4% | 30 |
| Provisions — Beer | $661 | 0.4% | 15 |

**All provisions combined** (food, merchandise, wine, beer, beverages, frozen meals) = **$69,737 or 38.1%** of total revenue. This is the core business. Add frozen meals and it's 46.2%.

Provisions includes: Benton's Bacon, MASA chips, Bon Bons, Edisto Honey, Date Better bars, Ranger Station Candle, Card Amy Zhang, Vandy chips, sauces, crackers, eggs, sausage, branded merch (PM tumbler, t-shirts), wine, and 300+ other SKUs.

*Evidence: item-level categorization from ticket-detail-all.csv*

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
- **Coffee:** 75-83% gross margin (see COGS breakdown below)
- **Surcharges/gift cards:** 100% margin (no COGS)

### Estimated Gross Profit

| Category | Revenue | Est. COGS | Gross Profit | GP % |
|---|---|---|---|---|
| All products (100% markup) | $131,829 | $65,915 | $65,915 | 50% |
| Coffee (75-83% margin) | $37,638 | ~$5,600 | ~$32,000 | 75-83% |
| Beer (60% markup) | $921 | $575 | $345 | 37.5% |
| Surcharges/Gift Cards | $12,523 | $0 | $12,523 | 100% |
| **Total** | **$182,911** | **~$72,000** | **~$111,000** | **~61%** |

**Annualized gross profit: ~$168K** (208 operating days/yr).

### Coffee COGS Breakdown

Bean cost is known: $68 for a 5lb bag, 18g per double shot = **$0.54 per drink in beans**. Milk adds $0.27-$0.78 depending on type (10oz for a latte, 7oz for a cappuccino). Cup/lid ~$0.15.

| Drink | COGS | Sale Price | Gross Margin |
|---|---|---|---|
| Latte | $0.96–$1.47 | $6.00 | 75–84% |
| Cappuccino | $0.88–$1.24 | $5.50 | 78–84% |
| Espresso/Americano | $0.69 | $3–4 | 77–83% |
| Add Syrup | ~$0.12 | $1.00 | ~88% |

Coffee is the highest-margin category at 75-83%, vs 50% on provisions and 37.5% on beer. Every $1 of provisions revenue is worth $0.50 in gross profit; every $1 of coffee is worth $0.75-$0.83.

*Evidence: bean cost ($68/5lb bag) and pour weight (18g/shot) confirmed by owner. Milk cost estimated across scenarios ($3.50-$10/gallon).*

### New Manager Discount Impact

A new manager started ~January 2026 and cracked down on unauthorized friend/employee discounts. Excluding seasonal items that ended naturally and intentional employee merch giveaways (PM Sweatshirt, Merci t-shirt, PM Tumbler — those are deliberate perks):

| Metric | Pre-Manager (Oct-Dec) | Post-Manager (Jan-Mar) |
|---|---|---|
| Discount rate | 1.71% | **0.90%** |
| Discounts/operating day | $24.47 | **$10.42** |

**Where the savings came from:**

| Item | Pre Discounts | Post Discounts | Saved/Day |
|---|---|---|---|
| Open Retail | $211 | $80 | $2.02 |
| Latte | $135 | $47 | $1.37 |
| Le Bon Garcon P.O.G. Caramels | $67 | $0 | $1.24 |
| Open Wine | $54 | $0 | $1.00 |
| Maeve Bon Bons | $43 | $0 | $0.80 |
| Open Food | $44 | $2 | $0.76 |

**Annualized savings: ~$2,922/year** in recovered margin. At 50% gross margin on provisions, that's equivalent to ~$5,800 in revenue you'd otherwise need to generate.

A few items show newly increased discounting post-manager (Cirelli wine 100% discounted, Food You Want to Eat cookbook at 50% off) — likely intentional markdowns on slow movers, but worth confirming.

*Evidence: item_discount field in ticket-detail-all.csv, Oct-Dec 2025 vs Jan-Mar 2026 comparison, excluding seasonal items and intentional employee merch*

## 12. Growth Trends

- Revenue per operating day is **flat** over 9 months (~-$9/month trend)
- But ticket count is growing at +5.1 tickets/day/month
- Rising stars: Cookie Asst. (+171%), Card Amy Zhang (+223%), Ranger Station Candle (new, $1,080)
- Fading: Open Retail declining (good — more items properly categorized)

### Provisions Growth: Corrected Analysis

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

**Corrected interpretation:** The "retail is the moat" thesis is still directionally right — it's the one category Babas can't touch. But the growth engine is **new product introductions**, not organic demand for existing products. Keep launching new merch aggressively — that's what works.

*Evidence: item-level phase analysis from ticket-detail-all.csv, controlling for 5 breakout items and seasonal patterns*

## 13. Operational Notes

- Void rate: 0.72% — exemplary, no waste/training issue
- "Open" items (Open Food, Open Retail, etc.) declining from 11.5% → 5.7% of revenue — better POS discipline
- Surcharge/dual pricing steady at ~4% of revenue, avg surcharge rising ($1.14 → $1.23)
- No menu prices changed between periods — all improvement is operational
- 18 of top 20 items have never been price-tested

*Evidence: item_status field in ticket-detail-all.csv, Open item trends from sales-summary CSVs*

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

**Verdict:** Petit Merci is not a student-dependent business. It's a provisions store serving locals, tourists, and seasonal gift shoppers.

*Evidence: CofC 2025-2026 academic calendar from charleston.edu/registrar, daily revenue from ticket-detail-all.csv*

## 15. Weather Impact

Weather data from Open-Meteo archive API for Charleston, SC (CHS) merged with daily sales.

**Finding: Weather matters, but backwards from what you'd expect. Petit Merci is a bad-weather business.**

- **Rain has zero impact on revenue** (t-test p = 0.74). Rainy Saturdays actually outperform dry Saturdays ($1,815 vs $1,711).
- **Clear/sunny days are the worst** ($1,336 avg) — people go to the beach, not the provisions store.
- **Drizzle/overcast days are the best** ($1,608 avg) — nesting weather drives people to a cozy store.
- **Below 50F is the only danger zone** ($953/day) — too cold for walk-in traffic.
- **50-60F is the sweet spot** ($1,666/day, $40/ticket) — jacket weather, not beach weather.

Controlling for day-of-week and seasonality, weather explains essentially none of the revenue variation (r = -0.012 for temperature residuals).

*Evidence: Open-Meteo historical API, merged dataset at daily-sales-weather.csv*

## 16. Competitor Impact — Babas on Wentworth

**Babas Charleston** (https://babascharleston.com/) opened their third location at 115½ Wentworth St — one block from Petit Merci — on **December 16, 2025**.

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
- **Retail/Market goods:** merch share of total revenue rose from 6.8% → 13-14%. However, this is driven primarily by 5 breakout product introductions and seasonal holiday buying — not organic growth of existing merch. See Section 12 for corrected analysis. Babas does not sell retail goods.

**Monthly trajectory:**
- January 2026 was the shock month ($1,155/day, worst month) — first full month after Babas opened.
- February 2026 recovered to $1,560/day — customers settling into visiting both, not fully defecting.

**Verdict:** Babas is stealing coffee and sandwich traffic but cannot compete on provisions — curated retail food, merchandise, frozen meals, wine. That's Petit Merci's core business and moat.

*Evidence: Babas website, Post and Courier opening announcement, Yelp reviews, daily revenue split from ticket-detail-all.csv*

---

## Priority Actions

| # | Action | Rationale | Impact |
|---|---|---|---|
| 1 | **Accelerate new merch launches** | Growth comes from new product introductions, not existing catalog. Keep the pipeline moving every 4-6 weeks. | +$10-15K/yr |
| 2 | **Differentiate food menu from Babas** | They're stealing coffee (-$99/day) and sandwiches (-$20/day). Total revenue loss ~$25K/yr, ~$18.5K in profit (coffee is 80% margin). Compete on items they DON'T have: frozen meals, specialty retail food, French-specific items. | Defend ~$25K/yr revenue |
| 3 | **Own the holiday/gifting calendar** | Dec is 2.2x normal revenue. Thanksgiving Eve = $4,779. Plan 5-6 seasonal moments/year. January is a cliff — plan a post-holiday promotion. | +$15-20K/yr |
| 4 | **Food upsell on coffee tickets** | 64% leave without food. Prompt at register. | +$15K/yr |
| 5 | **Price test staples** | Matcha $6→$8 worked. Don't compete on price with Babas — compete on experience + quality. | +$3-5K/yr |
| 6 | **Prune 150+ zombie SKUs** | 50% of catalog = 7% of revenue. Free shelf space for new launches. | Save $5-9K/yr |

---

## Next Investigations

- [ ] **Instagram posting** — do posts/stories correlate with next-day or same-day traffic?

---

## Data Files

| File | Description |
|---|---|
| `ticket-detail-all.csv` | 18,638 item rows, every transaction Jul 2025–Mar 2026 |
| `daily-sales-weather.csv` | Daily revenue merged with weather data (137 days) |
| `skytab-items-by-timeslot.csv` | Item sales by 15-min time slot (full period aggregate) |
| `hourly-sales-1774127383705.csv` | Hourly revenue by date |
| `sales-summary-combined.csv` | Item summaries with Period column (2025 / 2026 YTD) |
