# MOD4 — EOQ Inventory Optimization

**Technique:** Economic Order Quantity (EOQ) Model  
**Solver Engine:** GRG Nonlinear  
**Objective:** Minimize Total Annual Inventory Cost (Ordering Cost + Holding Cost)

---

## Problem Statement

A business needs to determine the optimal number of units to order per cycle to minimize total annual inventory costs, given fixed annual demand, unit cost, holding cost rate, and order cost.

---

## Model Parameters

| Parameter | Value |
|-----------|-------|
| Annual Demand (AD) | 15,000 units |
| Unit Cost (UC) | $80.00 |
| Holding Cost Rate (HC) | 18% per year |
| Order Cost (OC) | $220 per order |

---

## Formulas

| Metric | Formula |
|--------|---------|
| EOQ | `SQRT((2 × AD × OC) / (UC × HC))` |
| Reorder Point (RP) | `2 × EOQ` |
| Annual Ordering Cost | `OC × (AD / Q)` |
| Annual Holding Cost | `HC × UC × ((RP + Q) / 2)` |
| Total Cost | `AOC + AHC` |

---

## Results

| Metric | Value |
|--------|-------|
| Optimal Order Quantity (EOQ) | **677 units** |
| Reorder Point | 1,354 units |
| Total Orders per Year | 22.16 |
| Annual Ordering Cost | $4,874.42 |
| Annual Holding Cost | $14,623.27 |
| **Minimized Total Annual Cost** | **$17,060.48** |

Solver verified that the GRG Nonlinear solution converged with a reduced gradient of **0** at the optimal point — confirming global optimality for this unconstrained smooth function.

---

## Sensitivity Analysis (Data Table)

A two-variable data table was built to map total cost across a range of order quantities (599–746) and order prices ($200–$4,250), producing a cost surface that shows:

- **Cost is convex** — increasing steeply on both sides of the optimal EOQ
- **Lower order prices** (closer to $220) flatten the cost curve, giving more flexibility
- **Higher order prices** sharply penalize over/under-ordering, requiring tighter adherence to EOQ

### Sample cost surface (order qty × order price)

| Order Qty ↓ / Order Price → | $1,000 | $1,500 | $2,000 | $2,750 |
|-----------------------------|--------|--------|--------|--------|
| 599 | $14,919 | $15,066 | $15,214 | $15,436 |
| 677 | $15,509 | $15,731 | $15,952 | $17,060 |
| 737 | $15,657 | $15,916 | $16,174 | $17,061 |

---

## Key Insights

1. **Ordering 677 units per cycle** balances ordering frequency (22 orders/year) against holding cost — the classic EOQ tradeoff.

2. **Holding cost dominates** the total cost structure (85.6% of total) vs. ordering cost (14.4%). This means reducing the holding cost rate (e.g., renegotiating storage fees) would have a greater impact than reducing per-order costs.

3. **The reorder point of 1,354 units** represents a safety buffer of exactly 2× the order quantity — meaning the business maintains roughly a 33-day inventory buffer at current demand rates (15,000/365 ≈ 41 units/day → 1354/41 ≈ 33 days).

4. **Sensitivity to order price**: At the current cost structure, the EOQ is robust. Even if order cost doubled to $440, the optimal quantity only shifts to ~957 units — a proportional relationship since EOQ scales with `SQRT(OC)`.

---

## File

`EOQ_Inventory0_MOD4Project_PandyaK.xlsx` — contains Sheet1 (model + data tables), Answer Report, Sensitivity Report, and Limits Report.
