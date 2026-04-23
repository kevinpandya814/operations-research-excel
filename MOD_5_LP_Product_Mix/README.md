# MOD5 — LP Product Mix Optimization

**Technique:** Linear Programming (LP) — Product Mix  
**Solver Engine:** Simplex LP  
**Objective:** Maximize total profit subject to investment, storage, advertising, and selling constraints

---

## Problem Statement

A retailer must decide how many units of each product to stock in order to maximize profit, given a fixed investment budget, limited floor storage, and business policy constraints on advertising mix and product ratios.

---

## Products & Margins

| Product | Cost | Selling Price | Unit Margin |
|---------|------|--------------|-------------|
| Pressure Washer (X₁) | $330 | $499.99 | $169.99 |
| Go-Kart (X₂) | $370 | $729.99 | $359.99 |
| Generator (X₃) | $410 | $700.99 | $290.99 |
| Water Pump (X₄) | $127 | $269.99 | $142.99 |

**Objective Function:**  
`Maximize Z = 169.99X₁ + 359.99X₂ + 290.99X₃ + 142.99X₄`

---

## Constraints

| Constraint | Expression | Limit |
|-----------|-----------|-------|
| Total Investment | `330X₁ + 370X₂ + 410X₃ + 127X₄ ≤ 170,000` | $170,000 |
| Floor Storage | `25X₁ + 40X₂ + 25X₃ + 1.25X₄ ≤ 12,300` | 12,300 sq ft |
| Advertising mix | `0.7X₁ + 0.7X₂ − 0.3X₃ − 0.3X₄ ≥ 0` | Motorized items ≥ 30% of total |
| Selling ratio | `X₃ − 2X₄ ≥ 0` | Generators ≥ 2× Water Pumps |
| Non-negativity | `X₁, X₂, X₃, X₄ ≥ 0` | — |

---

## Optimal Solution

| Variable | Product | Quantity |
|----------|---------|----------|
| X₁ | Pressure Washer | **0 units** |
| X₂ | Go-Kart | **155 units** |
| X₃ | Generator | **238 units** |
| X₄ | Water Pump | **119 units** |

**Maximum Profit: $142,050.70**

The Pressure Washer (X₁) was excluded from the optimal mix — its reduced cost of **−$110.07** means its unit margin would need to increase by at least $110.07 (from $169.99 to $280.06) before it becomes worth stocking.

---

## Constraint Status

| Constraint | Status | Slack |
|-----------|--------|-------|
| Total Investment ($170,000) | **Binding** | $0 |
| Floor Storage (12,300 sq ft) | **Binding** | 0 |
| Advertising ratio | Not Binding | 1.63 |
| Selling ratio (X₃ ≥ 2X₄) | Not Binding | 1.63 |

Both the investment budget and storage capacity are fully utilized — these are the true bottlenecks of the business.

---

## Shadow Price Analysis

### Investment Constraint
- **Shadow Price: $0.558**
- Every additional dollar invested increases profit by **$0.558**
- Valid range: up to +$428.80 increase from current $170,000
- To increase profit by $50,000 → requires **$89,605 in additional investment**

### Storage Constraint
- **Shadow Price: $3.841**
- Every additional sq ft of storage increases profit by **$3.841**
- Expanding from 82 shelving units (12,300 sq ft) to 100 (15,000 sq ft):
  - Additional investment: $27,000
  - Additional profit generated: **$25,707**
  - Net benefit is positive — expansion is worth pursuing

---

## Key Insights

1. **Go-Karts and Generators are the profit engines.** Their high margins ($359.99 and $290.99 respectively) and favorable storage-to-margin ratios make them the dominant products in the optimal mix. Pressure Washers are simply not competitive given the binding constraints.

2. **Capital and space are the binding constraints, not policy.** Both the advertising and selling ratio constraints have slack — meaning the business's internal policies are not limiting profit. The real limiters are the $170K budget and 12,300 sq ft floor.

3. **The return on capital is 55.8 cents per dollar invested.** This is the shadow price of the investment constraint and is the single most important number for business planning. Any expansion or reallocation of capital should be evaluated against this benchmark.

4. **Storage expansion has a positive ROI.** Adding 18 shelving units (+2,700 sq ft) for $27,000 generates $25,707 in incremental profit — nearly breaking even in a single cycle. Over multiple inventory cycles, this expansion pays for itself quickly.

5. **Go-Kart margin has room to absorb cost increases.** The allowable decrease in Go-Kart profit coefficient is $76.74 — meaning even if the Go-Kart's cost rose by ~$77 per unit (to $447), it would still be in the optimal mix.

---

## File

`LP_Product_Mix.xlsx` — contains Sheet1 (LP model, constraints, additional investment analysis), Answer Report, Sensitivity Report, and Limits Report.
