# maheshfarkande_2511463_part3_regression_insights

# Business Regression Dataset — Data Understanding

**Source file:** `business_regression_data.xlsx` (sheet: `store_performance`)
--
## 1. Dependent variable

There are **two plausible targets**, and the choice depends on the business question:

- **`monthly_sales`** — the most natural primary target for a "what drives revenue" model. It is strongly and intuitively related to operational drivers (footfall, staffing, marketing).
- **`monthly_profit`** — appropriate if the goal is profitability rather than top-line revenue. It behaves differently from sales (e.g. discounts hurt profit more than sales).


## 2. Potential independent variables

Operational and contextual drivers available for prediction:

- `marketing_spend`
- `footfall`
- `avg_discount_pct`
- `staff_count`
- `inventory_availability_pct`
- `competitor_distance_km`
- `holiday_flag`
- `customer_rating`
- `region` (categorical → dummy encode)
- `store_type` (categorical → dummy encode)

Correlations with `monthly_sales` give a first signal of strength: `footfall` (0.86) and `staff_count` (0.81) are the strongest, `marketing_spend` (0.41) moderate, the rest weak. Against `monthly_profit`, `avg_discount_pct` becomes meaningfully negative (−0.34), which makes business sense.

## 3. Numerical variables

Continuous / count predictors suitable for direct use (after the cleaning notes below):

`marketing_spend`, `footfall`, `avg_discount_pct`, `staff_count`, `inventory_availability_pct`, `competitor_distance_km`, `customer_rating`

Targets `monthly_sales` and `monthly_profit` are also numeric.

## 4. Categorical variables

- `region` — 4 levels (nominal) → one-hot / dummy encode (drop one level as baseline).
- `store_type` — 4 levels (nominal) → one-hot / dummy encode. Note **Airport is small** (only 28 rows), so its coefficient will be less stable.
- `holiday_flag` — already a 0/1 binary indicator; usable as-is, no encoding needed.
- `store_id` — categorical identifier ; not a normal predictor.

## 5. Variables that may need cleaning or transformation

- **Missing values:** `competitor_distance_km` (6 missing, ~1.9%) and `customer_rating` (8 missing, ~2.5%). Impute (median is reasonable given mild skew) 
- **`avg_discount_pct` units:** stored as a fraction (0–0.292), not whole percentages. 
- **`marketing_spend` skew / outliers:** right-skewed with a few large values (max 172k vs. median ~55k).
- **`month` encoding:** currently a datetime. For regression, treat it as a categorical seasonality factor (month dummies) or a simple time index, rather than feeding the raw datetime in.
- **Multicollinearity:** `footfall` and `staff_count` are highly correlated (r ≈ 0.92) — they carry overlapping information. Including both inflates variance (high VIF); 

## 6. Variables that may not be useful for regression

- **`store_id`** — a unique identifier with no intrinsic predictive meaning. . (It is still valuable for understanding the panel structure or, in a more advanced model, as a grouping variable for fixed/random effects.)
- **`customer_rating`** — shows essentially no linear correlation with either target (≈ −0.03 to −0.09). It may be weak or irrelevant for a linear model; 
- **The non-target sales/profit column** — whichever of `monthly_sales` / `monthly_profit` is *not* chosen as the dependent variable should not be used as a predictor of the other (outcome leakage).
- **`month` raw datetime** — not useful in raw form; only useful once transformed.

---



Drop `staff_count` (or `footfall`) to manage multicollinearity, handle the missing values first, and consider log-transforming `marketing_spend`. Validate residuals and check VIFs before trusting coefficients.
