# Final Recommendation

Based on the regression analysis of 320 store-month records (80 stores, Jan–Apr 2025), with `monthly_sales` as the outcome. The recommended model is **M1 (multiple regression, R² = 0.83)**. Supporting evidence: `model_equations.md`, `analysis/model_comparison.md`, `analysis/residual_analysis.md`, and the screenshots folder.

---

## 1. Which factors are most strongly associated with monthly sales?

In order of strength and reliability (from model M1, all "all else equal"):

1. **Footfall — by far the strongest.** Each extra visitor is associated with ~£34 in monthly sales, and footfall alone explains ~74% of sales variation. This is the single most important variable.
2. **Marketing spend — positive and highly significant.** ~£1.21 of additional sales per £1 spent, after controlling for footfall.
3. **Inventory availability — significant.** ~£2,704 more sales per percentage-point of stock availability. Easy to overlook, but it matters once other factors are held constant.
4. **Region — significant.** North, South and West regions out-sell the East baseline by roughly £15k–£22k/month for comparable stores.
5. **Competitor distance — significant but counter-intuitive** (stores farther from competitors sell slightly less; likely a location/footfall artefact, not a lever).

---

## 2. Which variables should leadership focus on?

**Footfall, marketing spend, and inventory availability** — these are the significant drivers that the business can actually influence:

- **Footfall** is the highest-leverage outcome. Anything that reliably increases store visits (location, opening hours, catchment marketing, events) should map most directly to sales.
- **Marketing spend** shows a positive, statistically robust return and is a direct budget lever.
- **Inventory availability** is an operational lever — keeping shelves stocked has a measurable, significant payoff.

Regional differences are useful for **benchmarking and target-setting** (comparing like-for-like stores), even though "region" itself isn't an action.

---

## 3. Which variables should *not* be over-interpreted?

- **`avg_discount_pct`** — not statistically significant in the multiple model (p=0.20). Do not conclude that discounting raises or lowers sales from this data.
- **`customer_rating`** — essentially no linear relationship with sales (p=0.64 simple). It may still matter for retention/brand, but this dataset gives no evidence it moves monthly sales.
- **`competitor_distance_km`** — significant but with a counter-intuitive sign; treat as correlation, not a lever.
- **The intercept** — a mathematical anchor, not a real "baseline store"; don't read meaning into it.
- **`marketing_spend`'s simple-model slope (£2.13)** is inflated by omitted variables; the more trustworthy figure is M1's **£1.21**.

---

## 4. Recommended business action

1. **Prioritise footfall.** Treat store visits as the primary sales engine; invest in the levers that drive traffic and track footfall as a leading indicator of sales.
2. **Maintain/continue marketing investment** where it demonstrably lifts footfall and sales, but evaluate it per-store rather than assuming the headline £2.13 return — the controlled estimate is ~£1.21 per £1.
3. **Tighten inventory availability**, especially in stores currently below the ~88% average; the analysis shows a real, significant sales payoff.
4. **Run a like-for-like benchmarking exercise by region and store type**, and **study the over- and under-performers** flagged in the residual analysis (e.g. STR-1058, STR-1003 as positive case studies; STR-1012, STR-1023 for diagnosis).
5. **Re-fit the model with `store_type` included** before using it for forecasting — the residuals show it currently under-predicts Airport/Mall and over-predicts Residential stores.

---

## 5. Risks and limitations leadership should keep in mind

- **Only 4 months of data** (Jan–Apr 2025): no full-year seasonality, and conclusions may not generalise across the year.
- **Repeated stores (panel data):** the same 80 stores appear 4 times each, so plain-OLS p-values are slightly optimistic; treat borderline significances cautiously.
- **Unexplained store-level variation:** ~17% of sales variation is not captured, and individual residuals reach ~£100k (~15% of average sales). The model is a guide, not a precise per-store predictor.
- **Systematic store-type bias** in the current M1 (see residual analysis) until `store_type` is added.
- **Counter-intuitive/weak coefficients** (competitor distance, discount) should not drive decisions.
- **Two missing-data columns were median-imputed**, which slightly dampens those variables' apparent effects.

---

## 6. Why does regression show association but not prove causation?

Regression measures **how variables move together**, not **what causes what**. Several reasons the associations here cannot be read as proof of cause-and-effect:

- **Confounding / omitted variables.** A driver may be standing in for something we didn't measure. For example, marketing spend and footfall both rise in busy commercial locations, so part of marketing's apparent "effect" may really be location.
- **Reverse causality.** The arrow can run the other way: high-selling stores may be *given* bigger marketing budgets and more staff, rather than the spend creating the sales.
- **Observational, not experimental, data.** Nobody randomly assigned discounts or marketing budgets to stores. Without a controlled experiment, we can't rule out that a third factor explains both the predictor and sales.
- **Correlation patterns can be coincidental or location-driven**, as the negative competitor-distance coefficient illustrates.

**Practical takeaway:** use the regression to *prioritise and form hypotheses* — footfall, marketing, and inventory are the strongest candidates — but **confirm the highest-stakes decisions with controlled tests** (e.g. an A/B marketing-spend pilot across matched stores) before assuming that pulling a lever will move sales by the modelled amount.
