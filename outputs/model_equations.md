# Model Equations & Dummy Variable Approach

**Dependent variable (all models):** `monthly_sales` (in currency units)
**Data:** 320 store-month records (80 stores × 4 months, Jan–Apr 2025).
**Cleaning before modelling:** 6 missing `competitor_distance_km` and 8 missing `customer_rating` values were filled with each column's median (3.365 km and 3.9 respectively) so all 320 rows are retained.

---

## 1. Dummy variables — approach and reference categories

Two variables are categorical text and cannot enter a regression directly: **`region`** (East, North, South, West) and **`store_type`** (Airport, High Street, Mall, Residential). They are converted into **dummy (0/1) variables**.

**The rule we follow:** for a category with *k* levels we create only **k − 1** dummy columns and leave one level out. The left-out level becomes the **reference (baseline) category**. We deliberately omit one to avoid the *dummy-variable trap* — if we included all *k* dummies plus the intercept, the columns would add up to a constant and be perfectly redundant (perfect multicollinearity), and the model could not be estimated.

| Categorical variable | Reference (baseline) | Dummy columns created |
|---|---|---|
| `region` | **East** | `region_North`, `region_South`, `region_West` |
| `store_type` | **Airport** | `stype_High Street`, `stype_Mall`, `stype_Residential` |

**How to read a dummy coefficient:** each dummy's coefficient is the *average difference in monthly sales versus the reference category*, holding the other predictors constant. For example, `region_West = +22,474` means West-region stores sell about £22.5k/month more than otherwise-identical East-region stores (East = the baseline that is "built into" the intercept).

The final multiple model uses the **region** dummies (East as reference). `holiday_flag` is already a 0/1 indicator, so it needs no conversion.

---

## 2. Simple regression equations (one predictor each)

Each is `monthly_sales = intercept + slope × predictor`.

| Model | Equation | R² |
|---|---|---|
| **S1 footfall** | `sales = 446,411 + 35.68 × footfall` | 0.736 |
| **S2 marketing_spend** | `sales = 560,777 + 2.13 × marketing_spend` | 0.167 |
| **S3 avg_discount_pct** | `sales = 697,836 − 138,730 × avg_discount_pct` | 0.008 |
| **S4 inventory_availability_pct** | `sales = 484,814 + 2,218 × inventory_availability_pct` | 0.013 |
| **S5 customer_rating** | `sales = 701,367 − 5,285 × customer_rating` | 0.001 |

**Coefficient meaning, in business terms:**
- **S1:** each additional visitor (footfall) is associated with about **£35.7 more** monthly sales. Footfall alone explains ~74% of the variation in sales — by far the strongest single driver.
- **S2:** each extra £1 of marketing spend is associated with about **£2.13 more** sales. Statistically very strong (p<0.001) but explains only ~17% on its own.
- **S3:** higher discounting is associated with *lower* sales, but the relationship is not statistically reliable (p=0.10) and explains <1%.
- **S4:** better inventory availability is associated with higher sales (≈£2,218 per percentage-point), marginally significant (p=0.04) but tiny explanatory power.
- **S5:** customer rating shows essentially no linear relationship with sales (p=0.64).

---

## 3. Multiple regression equation (final model, M1)

```
monthly_sales =  157,906
              +     33.90 × footfall
              +      1.21 × marketing_spend
              −  46,856   × avg_discount_pct
              +   2,704   × inventory_availability_pct
              −   3,172   × competitor_distance_km
              +  14,964   × region_North
              +  21,588   × region_South
              +  22,474   × region_West
```
*(Reference region = East. `staff_count` was deliberately left out — it correlates 0.92 with footfall and would duplicate the same signal.)*

**R² = 0.829, Adjusted R² = 0.825** — the model explains about **83%** of the variation in monthly sales.

**Explanation of each coefficient (business language):**
- **Intercept (157,906):** the model's baseline — predicted sales for a hypothetical East-region store with all numeric predictors at zero. It is an anchoring constant, not a realistic store, so it is not interpreted directly.
- **footfall (+33.90):** holding everything else equal, one extra visitor is worth ~£34 in monthly sales. The dominant driver.
- **marketing_spend (+1.21):** each extra £1 of marketing is associated with ~£1.21 more sales, after accounting for footfall and the rest. A positive but modest return.
- **inventory_availability_pct (+2,704):** each extra percentage-point of stock availability adds ~£2,700 — keeping shelves stocked matters.
- **competitor_distance_km (−3,172):** counter-intuitively, stores *farther* from a competitor sell slightly *less*; this likely reflects that close-competition stores sit in busy commercial areas. Treat as a correlation, not a lever to pull.
- **region_North / South / West (+14,964 / +21,588 / +22,474):** these regions out-sell the East baseline by roughly £15k–£22k/month for comparable stores.
- **avg_discount_pct (−46,856):** negative direction, but **not statistically significant** (p=0.20) — see "weak variables" below.

---

## 4. Variables that are statistically weak or hard to interpret

- **`avg_discount_pct`** is not significant in the multiple model (p=0.20); its coefficient should not be relied on.
- **`competitor_distance_km`** is significant but its *negative* sign is counter-intuitive and probably reflects location/footfall confounding rather than a real causal effect.
- In the simple models, **`customer_rating`** and **`avg_discount_pct`** are statistically weak and should not be over-interpreted.

---

## 5. Final model selected & why

**Final model: M1 — the multiple regression.**

Reasons, in plain terms:
1. **Best explanatory power:** R² of 0.829 vs 0.736 for the best single-variable model — it explains an extra ~9 percentage-points of sales variation.
2. **No multicollinearity problem:** every predictor's VIF is below 2 (all ≈1.0–1.1), so the coefficients are stable and individually interpretable. This is why `staff_count` was excluded.
3. **Multiple actionable, significant drivers:** footfall, marketing_spend, inventory_availability, and regional effects are all statistically significant, giving leadership several levers rather than one.
4. **It reflects how the business really works:** sales depend on many things at once, and the multiple model holds other factors constant so each effect is "all else equal," which a simple model cannot do.

The simple footfall model (S1) remains a useful *quick* benchmark, but M1 is the recommended model for understanding and decision-making.
