# Model Comparison

All models predict **`monthly_sales`** on 320 store-month records (80 stores × 4 months). Simple models use one predictor; the multiple model (M1) uses five numeric predictors plus region dummies.

See `screenshots/model_comparison_preview.png` and `outputs/regression_summary.xlsx` for the tabular evidence.

---

## Comparison table

| Model name | Variables used (DV = monthly_sales) | R² | Significant variables (p<0.05) | Business usefulness | Limitations |
|---|---|---|---|---|---|
| **S1 — Simple: footfall** | IV: footfall | **0.736** | footfall (p<0.001) | High. Footfall alone explains ~74% of sales variation — a strong, intuitive headline driver. | Single factor; ignores marketing, stock, region. Can't separate footfall from things that drive footfall. |
| **S2 — Simple: marketing_spend** | IV: marketing_spend | 0.167 | marketing_spend (p<0.001) | Moderate. Confirms marketing is positively linked to sales, useful for budget arguments. | Explains only ~17%; omitted variables (footfall) likely inflate the simple slope. |
| **S3 — Simple: avg_discount_pct** | IV: avg_discount_pct | 0.008 | none (p=0.10) | Low. No reliable linear link to sales. | Near-zero R²; not significant. |
| **S4 — Simple: inventory_availability_pct** | IV: inventory_availability_pct | 0.013 | inventory_availability (p=0.04) | Low on its own, but the variable becomes clearly useful inside M1. | Tiny R² alone; effect only visible once other factors are controlled. |
| **S5 — Simple: customer_rating** | IV: customer_rating | 0.001 | none (p=0.64) | Very low. No measurable linear link to sales. | Effectively no explanatory power. |
| **M1 — Multiple (final)** | IVs: footfall, marketing_spend, avg_discount_pct, inventory_availability_pct, competitor_distance_km + region dummies (East ref) | **0.829** (adj 0.825) | footfall, marketing_spend, inventory_availability, competitor_distance, region_North/South/West (all p<0.05) | **Highest.** Multiple significant levers, each interpreted "all else equal." Best tool for decision-making. | Observational (association ≠ causation); under-/over-predicts by store type (see residual analysis); only 4 months of data. |

---

## How the models compare

**Explanatory power (R²).** M1 is clearly strongest at **0.829**, beating the best single-variable model (S1 footfall, 0.736) by about 9 percentage-points and dwarfing S2–S5. Adjusted R² for M1 (0.825) barely differs from raw R², confirming the extra predictors earn their place rather than just inflating the fit.

**Significant variables.** In M1, **footfall, marketing_spend, inventory_availability_pct, competitor_distance_km** and all three **region** dummies are statistically significant (p<0.05). The one weak predictor is **avg_discount_pct** (p=0.20). Notably, `inventory_availability_pct` looks almost useless as a *simple* model (R²=0.013) but becomes a clear, significant driver once other factors are held constant in M1 — a good example of why the multiple model is more informative.

**No multicollinearity in M1.** Variance Inflation Factors for the numeric predictors are all ≈1.0–1.1 (well under the rule-of-thumb threshold of 5). This was achieved by excluding `staff_count`, which correlates 0.92 with footfall; keeping both would have made coefficients unstable.

**Business usefulness.** Simple models answer "is X linked to sales?" one factor at a time and are easy to communicate, but they confound effects — S2's marketing slope (£2.13/£1) is larger than M1's (£1.21/£1) precisely because the simple model lets marketing absorb credit that really belongs to footfall. M1 separates the effects and gives leadership several independent levers, so it is the better basis for decisions.

**Limitations (what the models do not capture).**
- **Association, not causation** — none of these models proves that changing a driver will change sales (see `residual_analysis.md` and `final_recommendation.md`).
- **Store-type structure is unmodelled in M1**, which leaves a visible pattern in the residuals: the model under-predicts Airport/Mall stores and over-predicts Residential ones.
- **Short window** — only 4 months (Jan–Apr 2025); no full-year seasonality.
- **Panel dependence** — the same 80 stores repeat 4 times, so standard errors from plain OLS are slightly optimistic.

---

## Verdict

**M1 (multiple regression) is the recommended model.** It has the highest explanatory power, no multicollinearity, and multiple actionable significant drivers. S1 (footfall) is retained as a simple, communicable benchmark. The remaining simple models (S3, S5 especially) add little and should not be used on their own.
