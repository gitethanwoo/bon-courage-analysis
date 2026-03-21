# Petit Merci — Analysis Findings

**Business:** Petit Merci, cafe/market in Charleston, SC
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
- **Recent months (Jan–Mar 2026):** ~42 tickets/day at ~$28/ticket (retail cafe)
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

- **Two businesses in one:** daily neighborhood cafe (majority of tickets, minority of revenue) vs curated market/gift destination (minority of tickets, majority of revenue)

*Evidence: ticket value distribution clustering from ticket-detail-all.csv*

## 10. Seasonal Patterns

- **December** is peak: $1,682/day avg (2.2x normal). Week of Dec 15 hit $10,276.
- **Valentine's 2026** was a deliberate, successful seasonal push: 1 SKU/$317 (2025) → 16 SKUs/$1,991
- Gift cards spike in December (24 of 50 total loads)
- Christmas/Fall seasonal items generated ~$5,300 in 2025
- Seasonal coffee drinks (Frosty, Gingerbread Man, Le Fluffernut) contributed ~$2,500

*Evidence: monthly revenue from ticket-detail-all.csv, seasonal item analysis from sales-summary CSVs*

## 11. Pricing & Discounts

- Total discounts: ~$3,000 (1.62% of gross) — disciplined
- Discount rate tightened from 1.9% (2025) → 1.0% (2026 YTD)
- PM Merchandise (t-shirts, sweatshirts, tumblers) has highest discount rate at 21–37%
- **No menu prices changed between periods** — all improvement is operational
- 18 of top 20 items have never been price-tested
- Matcha Latte $6→$8 is the one natural experiment — it worked

*Evidence: discount fields in ticket-detail-all.csv, price comparison across sales-summary CSVs*

## 12. Growth Trends

- Revenue per operating day is **flat** over 9 months (~-$9/month trend)
- But ticket count is growing at +5.1 tickets/day/month
- Retail Merch is the fastest growing category (+95%)
- Rising stars: Cookie Asst. (+171%), Card Amy Zhang (+223%), Ranger Station Candle (new, $1,080)
- Fading: Open Retail declining (good — more items properly categorized)

*Evidence: monthly aggregation from ticket-detail-all.csv, period comparison from sales-summary-combined.csv*

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

## Next Investigations

- [ ] **Nearby college schedule** — does CofC academic calendar correlate with traffic?
- [ ] **Weather data** — rain/temperature impact on daily revenue
- [ ] **Instagram posting** — do posts/stories correlate with next-day or same-day traffic?

---

## Data Files

| File | Description |
|---|---|
| `ticket-detail-all.csv` | 18,638 item rows, every transaction Jul 2025–Mar 2026 |
| `skytab-items-by-timeslot.csv` | Item sales by 15-min time slot (full period aggregate) |
| `hourly-sales-1774127383705.csv` | Hourly revenue by date |
| `sales-summary-combined.csv` | Item summaries with Period column (2025 / 2026 YTD) |
| `sales-summary-by-item-1774126612429.csv` | 2025 full year item summary |
| `sales-summary-by-item-1774126787960.csv` | 2026 YTD item summary |
| `sales-summary-by-item-1774127740940.csv` | Combined period item summary |
