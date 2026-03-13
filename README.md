# The Architecture of Dimensionality: Hedonic Pricing & the FWL Theorem

## Objective
Construct and empirically validate a multivariate hedonic pricing model on synthetic 2026 California real estate data, using the Frisch-Waugh-Lovell (FWL) Theorem to formally demonstrate the mechanics of omitted variable bias and the necessity of full-dimensional covariate control.

---

## Methodology

- **Data Ingestion & Feature Engineering:** Sourced Zillow synthetic dataset containing residential transaction-level records with continuous regressors `Property_Age` and `Distance_to_Tech_Hub`, and target variable `Sale_Price` denominated in 2026 USD.

- **Bivariate OLS Baseline:** Estimated a naive single-regressor model regressing `Sale_Price` on `Property_Age` alone, deliberately omitting proximity to tech employment centers to establish a contaminated coefficient benchmark.

- **Multivariate OLS Specification:** Expanded the model to include `Distance_to_Tech_Hub` as a covariate, yielding a full hedonic specification that accounts for location-driven demand premiums alongside structural depreciation.

- **Manual FWL Residual Extraction:** Operationalized the Frisch-Waugh-Lovell Theorem by hand — partialling out the influence of `Distance_to_Tech_Hub` from both `Sale_Price` and `Property_Age` via auxiliary OLS regressions, isolating the orthogonal residual components that carry no shared variance with the omitted dimension.

- **Coefficient Verification:** Regressed the partialled-out `Sale_Price` residuals on the partialled-out `Property_Age` residuals, confirming exact numerical equivalence with the multivariate OLS coefficient — a direct proof of algorithmic *ceteris paribus*.

- **Visualization:** Produced comparative regression plots across all three model specifications (bivariate, multivariate, FWL-isolated) to render the bias correction mechanism visually interpretable.

---

## Key Findings

The bivariate model exhibited **severe omitted variable bias**: by excluding `Distance_to_Tech_Hub` — a regressor strongly correlated with both property age (newer builds cluster near tech corridors) and sale price (location commands a structural premium) — the naive specification **falsely attributed inflated price sensitivity to physical home age**. The `Property_Age` coefficient absorbed the unmodeled spatial demand signal, producing a distorted and economically misleading estimate.

Upon introducing the full covariate set, the multivariate model corrected this bias, disaggregating the independent marginal contributions of age-driven depreciation and location-driven appreciation. The manual FWL implementation **reproduced the multivariate `Property_Age` coefficient exactly**, validating that the theorem's residual-partialling procedure is not merely an algebraic curiosity — it is a precise computational proof that OLS, operating in full dimensionality, successfully holds all other regressors constant. The match to floating-point precision confirms that shared covariance had been completely stripped from the estimator, achieving true orthogonal isolation.

---

## Tech Stack
| Tool | Role |
|---|---|
| Python 3.10+ | Core runtime |
| pandas | Data wrangling & feature construction |
| statsmodels.formula.api | OLS estimation & residual extraction |
| matplotlib | Regression visualization |

---

## Data Source
Zillow synthetic dataset — 2026 California residential real estate metrics (`Sale_Price`, `Property_Age`, `Distance_to_Tech_Hub`).
