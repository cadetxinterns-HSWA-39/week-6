# CadetX — Heavy Supplier, Inventory & Warehouse Analytics

## Week 6 submission: Warehouse Space, Capacity, Throughput, Alignment, Bottlenecks & Performance Scoring

Phase 2 (Core Product, Inventory & Warehouse Analytics), sprint 3 of 3.

## Team

- Odetunde Olumide Temitope — Data Scientist
- Kukoyi Zainab Omotola — Data Analyst
- Nyong Asuabiat — Data Analyst

## What's in this folder

```
week-06/
├── Week6_Warehouse_Operations.ipynb   (full analysis, run end to end, outputs included)
├── README.md
├── SPRINT_NOTES_WEEK6.md
└── outputs/
    ├── space_utilisation.csv
    ├── branch_throughput_kpis.csv
    ├── product_warehouse_alignment.csv
    ├── bottleneck_checks.csv
    ├── branch_scorecard.csv
    ├── score_rank_stability.csv
    └── charts/ (5 PNG charts)
```

To re-run the notebook, open it in Google Colab and run all cells. The first cell mounts Google Drive, and the notebook then reads `sales_transactions.csv`, `inventory_position.csv` and `purchase_transactions.csv` from the project's `week-02/week-02/` folder and `stock_ledger.csv` from `week-3/` in Drive, and writes its outputs to `week-06/outputs/`.

## Ground rules

- Shipments and revenue = Delivered sales, last 12 months to 2024-12-31, revenue ex-GST.
- Receipts from `stock_ledger.csv`; branch size, staff and operating cost from `inventory_position.csv`.
- `branches.avg_monthly_revenue` is not used: annualised, it is only 5–11% of actual shipped revenue (the mismatch flagged in Week 3).
- Six branches only, so every comparison is between branches, not against an outside benchmark.

## Results

### 1. Space utilisation and capacity
Each product's `dimensions_cm` converts recorded stock into cubic metres (207,000 to 226,000 m³ per branch).
- To fit that stock, each branch would have to stack goods **52 m (Delhi) to 75 m (Kolkata) high** over its whole floor, with no aisles. At an assumed 6 m usable height (an assumption, not a data field) that is 862% to 1,243% of usable space.
- The recorded stock weighs 713,065 tonnes in total.
- Stock held at the policy max stock would fill only 2.0% to 5.3% of usable space.
- 2024 receipts alone were 147% to 217% of each branch's usable volume.

The recorded stock quantities cannot physically be inside these buildings. This supports the Week 3 `scale_mismatch_flag` and the Week 5 finding that recorded stock is hundreds of times its max stock. A physical count is needed before any space decision is made on these numbers.

### 2. Throughput (last 12 months)
| Branch | Units shipped | Shipped per employee | Cost per unit shipped (₹) | Revenue per sq ft (₹) |
|---|---|---|---|---|
| Kolkata East Depot | 36,956 | 754 | 566 | 23,736 |
| Ahmedabad West Hub | 31,524 | 500 | 768 | 16,855 |
| Chennai South Hub | 40,132 | 496 | 819 | 17,570 |
| Hyderabad Logistics | 27,917 | 490 | 811 | 14,381 |
| Pune Distribution | 30,703 | 415 | 1,037 | 14,286 |
| Delhi Central | 31,797 | 346 | 1,204 | 12,537 |

- Kolkata ships twice as much per employee as Delhi and at under half the cost per unit.
- Operating cost is 2.9% (Kolkata) to 6.7% (Delhi) of branch revenue.
- Receipts (509,000 to 582,000 units per branch) are **16.6 times shipments**, so inbound handling is most of the physical workload.
- The busiest month is 1.4 to 1.7 times an average month (Pune highest).

### 3. Product–warehouse alignment
- **Stock mix vs demand mix:** 0.945 to 0.955 (1.0 = identical), so each branch holds roughly the product mix it sells. This is mainly because demand is spread almost evenly across the 30 products. Max-stock settings are less well aligned (0.81 to 0.86).
- **Space and staff vs demand:** Delhi has 20.0% of capacity and 22.1% of staff for 16.0% of demand. Kolkata has 13.2% of capacity and 11.8% of staff for 18.6% of demand. Kolkata handles 1,237 units per 1,000 sq ft against Delhi's 703.

### 4. Bottleneck detection
Branches were compared on cancellation rate, order-to-delivery days and inbound lateness. All four differences are statistically detectable (large sample sizes) but small: the biggest gap is 17%, and delivery times differ by 0.15 days. **No branch is flagged as a bottleneck** (rule: p < 0.05 and a gap of at least 25%).

| Measure | Lowest branch | Highest branch |
|---|---|---|
| Order cancellation rate | 9.3% | 10.6% |
| Order-to-delivery days (mean) | 7.98 | 8.12 |
| Inbound receipts late | 14.3% | 15.7% |
| Inbound lateness (mean days) | 0.46 | 0.54 |

The common problem is network-wide rather than at one branch: delivery averages 8.1 days with a third of orders taking over 10 days, and about 1 in 10 order lines is cancelled.

### 5. Benchmarking and performance score
Five KPIs with a real gap between branches (at least 25%) are scored: shipped per employee, revenue per sq ft, cost per unit shipped, years of supply and stock vs max. The cancellation, delivery-time and inbound-lateness KPIs are left out because branches barely differ on them.

| Rank | Branch | Score (0–100) |
|---|---|---|
| 1 | Kolkata East Depot | 97.8 |
| 2 | Chennai South Hub | 58.1 |
| 3 | Ahmedabad West Hub | 49.9 |
| 4 | Hyderabad Logistics | 30.6 |
| 5 | Pune Distribution | 17.4 |
| 6 | Delhi Central | 16.5 |

Under 5,000 random KPI weightings, Kolkata ranks first 99.8% of the time, and Kolkata, Chennai and Ahmedabad are always the top three. The order of Pune and Delhi at the bottom is less certain, as their scores are close.

## Caveats

- **Scale flag.** Space results use recorded stock, which cannot physically fit (section 1). The 6 m stacking height is an assumption.
- **Reconciliation (31 Oct 2026).** The scale flag was traced through the purchasing and ledger data and is not a unit error. Received purchase orders (22.4 million units) and ledger receipts (20.4 million units) agree to within about 9%, and recorded stock is consistent with both, so the mismatch sits between purchasing and stock on one side and the sales and policy fields (max stock, reorder level) on the other. Stock-based figures stay as recorded.
- **Shared numerator.** Shipped per employee, revenue per sq ft and cost per unit shipped all divide by units shipped, so the score mostly reflects shipping volume relative to branch size.
- **No throughput ceiling.** The data has no maximum throughput per branch, so "capacity" is floor area, and headroom is judged only against the other branches.
- Staff numbers and operating costs are taken as recorded in the branch fields.
