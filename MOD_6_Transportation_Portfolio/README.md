# MOD6 — Transportation & Portfolio Optimization

**Techniques:** Transportation LP + Quadratic Portfolio Optimization  
**Solver Engine:** Simplex LP (Part 1) | GRG Nonlinear (Part 2)  
**Objectives:** Minimize waste shipping cost | Minimize portfolio risk at a target return

---

## Part 1: Waste Transportation Network Optimization

### Problem Statement

Six manufacturing plants generate hazardous waste and must ship it to three disposal sites. The goal is to find the minimum-cost shipping plan under two routing strategies: **direct shipping** (plant → site only) and **indirect shipping** (transshipment allowed through intermediate nodes).

---

### Network Parameters

**Plants (Supply)**

| Plant | Waste Generated |
|-------|----------------|
| Denver | 45 units |
| Morganton | 88 units |
| Morrisville | 42 units |
| Pineville | 53 units |
| Rockhill | 65 units |
| Statesville | 38 units |
| **Total** | **331 units** |

**Disposal Sites (Demand/Capacity)**

| Site | Capacity |
|------|----------|
| Orangeburg | 65 units |
| Florence | 80 units |
| Macon | 105 units |
| **Total** | **250 units** |

**Cost Matrix ($/unit shipped)**

| From ↓ / To → | Orangeburg | Florence | Macon |
|---------------|-----------|---------|-------|
| Denver | $12 | $15 | $17 |
| Morganton | $14 | $9 | $10 |
| Morrisville | $13 | $20 | $11 |
| Pineville | $17 | $16 | $19 |
| Rockhill | $7 | $14 | $12 |
| Statesville | $22 | $16 | $18 |

---

### Part 1A: Direct Transportation

**Objective:** `Minimize Z = Σ Cᵢⱼ × Xᵢⱼ` (plant i → site j only)

**Optimal Routing Plan**

| From | To | Units |
|------|----|-------|
| Denver | Orangeburg | 36 |
| Denver | Florence | 9 |
| Morganton | Macon | 26 |
| Morrisville | Macon | 42 |
| Pineville | Florence | 53 |
| Rockhill | Orangeburg | 29 |
| Statesville | Florence | 18 |
| Statesville | Macon | 20 |

**Minimum Direct Shipping Cost: $2,988**

---

### Part 1B: Indirect Transportation (Transshipment)

Waste can now route through intermediate nodes (plant → plant → site), expanding the feasible solution space.

**Optimal Routing Plan**

| From | To | Units |
|------|----|-------|
| Denver | Morganton | 45 |
| Morganton | Florence | 80 |
| Morganton | Macon | 8 |
| Morrisville | Macon | 42 |
| Pineville | Morrisville | 53 |
| Rockhill | Orangeburg | 65 |
| Statesville | Denver | 38 |
| Orangeburg | Macon | 55 |

**Minimum Indirect Shipping Cost: $2,713**

---

### Cost Comparison

| Method | Total Cost | Savings |
|--------|-----------|---------|
| Direct Shipping | $2,988 | — |
| Indirect Shipping | $2,713 | **$275 (9.2%)** |

---

### Shadow Price Analysis (Indirect Model)

| Constraint | Shadow Price | Interpretation |
|-----------|-------------|----------------|
| Orangeburg capacity (65) | $9/unit | Each additional unit of capacity at Orangeburg saves $9 |
| Florence capacity (80) | $9/unit | Each additional unit of capacity at Florence saves $9 |
| Macon capacity (105) | $10/unit | Each additional unit of capacity at Macon saves $10 |
| Denver supply (45) | $3/unit | Reducing Denver's waste generation by 1 unit saves $3 |
| Rockhill supply (65) | −$2/unit | Rockhill has a negative shadow price — reducing its waste *increases* cost (it's the cheapest source to Orangeburg) |

---

### Key Insights — Transportation

1. **Indirect routing saves 9.2% ($275)** by leveraging cheaper intermediate legs. Denver's waste routes through Morganton (cost $3/unit) rather than directly to disposal sites (min direct cost $12/unit to Orangeburg).

2. **Rockhill is the network's most valuable plant** — it sits closest to Orangeburg (cost $7/unit, the cheapest direct link in the entire matrix). Its negative shadow price (−$2) confirms that *more* waste from Rockhill is actually preferred.

3. **All disposal sites are at or near capacity** — the network is fully utilized. The high shadow prices ($9–$10) on disposal site capacities indicate that expanding site capacity (or finding new sites) would yield significant savings.

4. **Statesville's routing flips completely** — in direct shipping, Statesville splits between Florence and Macon. In indirect routing, it routes entirely through Denver. This counterintuitive result occurs because Statesville→Denver ($4) + Denver→Morganton→Florence is cheaper than Statesville→Florence ($16) directly.

5. **Morganton becomes a transshipment hub** — it absorbs Denver's 45 units on top of its own 88, shipping 80 to Florence and 8 to Macon. This is driven by Morganton→Florence being the cheapest available Florence link at $9/unit.

---

## Part 2: Portfolio Risk-Return Optimization

### Problem Statement

Optimize a $10,000 investment portfolio across 6 asset classes to **minimize portfolio variance** (risk) while achieving an **11% expected return**, using a covariance matrix to model asset relationships.

---

### Asset Classes & Expected Returns

| Asset | Expected Return |
|-------|----------------|
| Bonds | 7% |
| High-tech Stocks | 12% |
| Foreign Stocks | 11% |
| Call Options | 14% |
| Put Options | 14% |
| Gold | 9% |

---

### Covariance Matrix

|  | Bonds | Hi-Tech | Foreign | Calls | Puts | Gold |
|--|-------|---------|---------|-------|------|------|
| **Bonds** | 0.001 | 0.0003 | −0.0003 | 0.00035 | −0.00035 | 0.0004 |
| **Hi-Tech** | 0.0003 | 0.009 | 0.0004 | 0.0016 | −0.0016 | 0.0006 |
| **Foreign** | −0.0003 | 0.0004 | 0.008 | 0.0015 | −0.0055 | −0.0007 |
| **Calls** | 0.00035 | 0.0016 | 0.0015 | 0.012 | −0.0005 | 0.0008 |
| **Puts** | −0.00035 | −0.0016 | −0.0055 | −0.0005 | 0.012 | −0.0008 |
| **Gold** | 0.0004 | 0.0006 | −0.0007 | 0.0008 | −0.0008 | 0.005 |

---

### Optimal Portfolio Allocation

**Target Return: 11% | Minimized Risk (Variance): 0.000779**

| Asset | Weight | Allocation ($10,000) |
|-------|--------|---------------------|
| Bonds | 21.75% | $2,175 |
| High-tech Stocks | 11.53% | $1,153 |
| Foreign Stocks | 26.55% | $2,655 |
| Call Options | 5.49% | $549 |
| Put Options | 25.68% | $2,568 |
| Gold | 9.01% | $901 |
| **Total** | **100%** | **$10,000** |

---

### Efficient Frontier Analysis

The model was run across multiple return targets to map the risk-return tradeoff:

| Target Return | Portfolio Risk (Variance) |
|--------------|--------------------------|
| 10.0% | 0.000514 |
| 11.0% | 0.000736 |
| 11.2% | 0.000603 |
| 12.0% | 0.001129 |
| 12.5% | 0.001463 |
| 13.0% | 0.002098 |
| 13.5% | 0.003496 |

Risk accelerates non-linearly beyond 12% — consistent with the efficient frontier concept where higher returns require disproportionately higher risk.

---

### Key Insights — Portfolio

1. **Foreign Stocks (26.55%) and Put Options (25.68%) dominate the allocation.** Put Options provide a natural hedge — their strong negative covariance with Foreign Stocks (−0.0055) and High-tech Stocks (−0.0016) reduces overall portfolio variance significantly, making them essential even at an 11% return target.

2. **Call Options are minimally weighted (5.49%)** despite their 14% return — their high variance (0.012) and positive correlation with most risky assets makes them expensive to include from a risk perspective.

3. **Bonds serve as a stabilizer (21.75%)** — their low variance (0.001) and negative covariance with Foreign Stocks and Put Options makes them a key diversifier, even with only a 7% return.

4. **Risk accelerates sharply above 12%.** Moving from 12% to 13.5% target return nearly triples portfolio variance (0.001129 → 0.003496). An investor seeking 13.5% return takes on 6.8× the risk of a 10% return portfolio — rarely a worthwhile tradeoff without a specific high-risk mandate.

5. **Gold is underweighted (9%)** despite moderate return (9%) because its covariance with Bonds and High-tech Stocks is positive — it adds risk without adding enough return to justify a larger position at the 11% target.

---

## File

`Transportation_Portfolio.xlsx` — contains Part 1 (direct + indirect transportation models), Part 2 (portfolio optimization with efficient frontier), and Sensitivity Report.
