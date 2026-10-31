# Sprint Notes — Week 6

## Team

- Odetunde Olumide Temitope — Data Scientist
- Kukoyi Zainab Omotola — Data Analyst
- Nyong Asuabiat — Data Analyst

**Scrum Master this week:** _(to be filled in by the team)_

**Sprint scope:** Warehouse Space Utilisation, Capacity Analysis, Throughput Analysis, Product–Warehouse Alignment, Operational Bottleneck Detection, Utilisation Benchmarking, Warehouse Performance Scoring

## What was completed

- Built `Week6_Warehouse_Operations.ipynb`, run end to end, producing six result tables and five charts.
- Converted recorded stock to cubic metres and compared it with each branch's floor area (implied stacking height 52–75 m).
- Measured last-12-month throughput per branch: units shipped and received, per employee, per 1,000 sq ft, cost per unit and revenue per sq ft.
- Compared stock mix, max-stock mix, capacity and staff against demand for each branch.
- Tested four candidate bottlenecks across branches (significance and size of gap); none flagged.
- Scored and ranked the six branches on five KPIs, with a rank-stability test under 5,000 random weightings.

## Decisions made

- Revenue comes from transactions only, because `branches.avg_monthly_revenue` is 5–11% of actual shipped revenue.
- Stacking height of 6 m is used as an assumption for utilisation, and the implied height (which needs no assumption) is reported alongside it.
- A difference is flagged as a bottleneck only if it is statistically significant (p < 0.05) and the gap is at least 25%.
- Only KPIs with at least a 25% best-vs-worst gap are scored, so near-identical KPIs do not add noise.

## Findings

- Recorded stock cannot physically fit in any branch, which supports the Week 3 scale flag; at policy max stock, space use would be 2–5%.
- Kolkata is the most productive branch (754 units per employee, ₹566 per unit shipped); Delhi is the least (346 and ₹1,204) despite the largest floor and staff.
- Delhi holds 20% of capacity for 16% of demand; Kolkata holds 13% of capacity for 19% of demand.
- No branch is an operational outlier on cancellations, delivery time or inbound lateness; the slow delivery (8.1 days on average) and 10% cancellation rate are network-wide.
- Inbound handling is 16.6 times outbound, so the over-ordering found in Week 5 is also the main workload driver.

## Open items

- A physical stock count is needed to confirm quantities before any space or capacity decision.
- Throughput capacity (maximum units per day per branch) is not in the data, so capacity is judged only on floor area and comparison between branches.

## Next steps

Phase 3 (Weeks 7–9): Supplier, Customer & Predictive Analytics.
