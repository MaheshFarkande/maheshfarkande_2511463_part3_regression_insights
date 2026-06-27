# Residual Analysis — Final Model (M1, Multiple Regression)

**Residual = Actual sales − Predicted sales.**
A **positive** residual means the store sold **more** than the model expected (model *under-predicted*). A **negative** residual means it sold **less** than expected (model *over-predicted*).

Predicted sales were computed for all 320 records using model M1. Evidence: `screenshots/residuals_preview.png`.

---

## Largest positive residuals (model under-predicted — these stores beat expectations)

| store_id | month | store_type | Actual | Predicted | Residual |
|---|---|---|---:|---:|---:|
| STR-1058 | 2025-02 | High Street | 870,937 | 754,386 | **+116,552** |
| STR-1028 | 2025-04 | Mall | 713,611 | 601,879 | **+111,733** |
| STR-1051 | 2025-02 | Airport | 787,716 | 679,221 | **+108,495** |
| STR-1026 | 2025-04 | Mall | 625,514 | 519,397 | **+106,117** |
| STR-1003 | 2025-01 | Airport | 930,582 | 837,118 | **+93,464** |

## Largest negative residuals (model over-predicted — these stores underperformed)

| store_id | month | store_type | Actual | Predicted | Residual |
|---|---|---|---:|---:|---:|
| STR-1012 | 2025-01 | Residential | 595,468 | 728,177 | **−132,710** |
| STR-1023 | 2025-02 | Mall | 627,172 | 755,019 | **−127,847** |
| STR-1017 | 2025-03 | High Street | 685,379 | 805,453 | **−120,074** |
| STR-1001 | 2025-04 | Residential | 658,920 | 759,021 | **−100,100** |
| STR-1035 | 2025-01 | Residential | 633,638 | 731,394 | **−97,756** |

---

## What these residuals mean in business terms

A residual is the slice of a store's sales the model **could not explain** from footfall, marketing, stock availability, competitor distance and region. Large residuals point to something happening on the ground that the available data does not capture.

- **Positive-residual stores (e.g. STR-1058, STR-1028, STR-1051)** are **over-performers**: they generated £90k–£117k *more* than comparable stores with similar inputs. That extra is likely driven by factors not in the dataset — strong local management, product mix, a premium catchment, a one-off promotion, or a transport hub effect. These are worth studying as **"what are they doing right?"** case studies.
- **Negative-residual stores (e.g. STR-1012, STR-1023, STR-1017)** are **under-performers**: they sold £98k–£133k *less* than their inputs predicted. These are **diagnostic priorities** — possible causes include execution problems, stock-outs not reflected in the monthly average, local disruption, or new nearby competition. They warrant a closer operational review.

The size of these gaps (~£100k against a sales mean of ~£681k, i.e. ~15%) is material, so even a good model leaves meaningful store-level variation unexplained.

---

## Is the model biased toward certain store types?

Yes — there is a clear, systematic pattern. Average residual by store type:

| store_type | Mean residual | Interpretation |
|---|---:|---|
| Airport | **+23,362** | Model **under-predicts** — Airport stores consistently beat the model |
| Mall | **+13,435** | Model **under-predicts** |
| High Street | +273 | Essentially unbiased |
| Residential | **−17,069** | Model **over-predicts** — Residential stores consistently fall short |

**Reading the pattern:** the final model includes *region* dummies but **not** *store_type* dummies. As a result it bakes the store-type differences into the error term: it **systematically under-predicts Airport and Mall stores** (which sell more than their measured inputs suggest) and **over-predicts Residential stores** (which sell less). Four of the five biggest negative residuals are Residential, and both Airport rows plus two Malls appear among the biggest positive residuals — exactly what this bias predicts.

**Implication / recommended fix:** add `store_type` dummies to the model. Doing so would absorb this structural difference, reduce these biased residuals, and likely lift R² further. Until then, leadership should mentally **adjust M1's predictions upward for Airport/Mall stores and downward for Residential stores**, and treat store-type as a real, unmodelled driver rather than noise.

---

## Caveat

Residual analysis is diagnostic, not causal: a large residual flags a store worth investigating, but it does not by itself tell you *why* the store over- or under-performed. The next step is qualitative follow-up on the flagged stores, plus re-fitting the model with `store_type` included.
