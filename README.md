# CIE 2 – Activity 1: Multiple Linear Regression on California Housing

## 1. What this activity is about
Take the role of a data analyst at a real-estate advisory firm and **explain and forecast the median house value of a neighbourhood** from several census features at once. 

## 2. Dataset
| | |
|---|---|
| **Name** | California Housing (1990 US Census, block-group level) |
| **Original source** | Pace, R. K. & Barry, R. (1997), *Sparse Spatial Autoregressions*, Statistics & Probability Letters 33(3) (StatLib repository) |
| **Copy used** | <https://github.com/ageron/handson-ml/tree/master/datasets/housing> → `data/housing.csv` |
| **Size** | 20,640 rows × 10 columns (19,475 rows after cleaning) |
| **One row** | One census block group (aggregate neighbourhood data – no personal data) |
| **Target (y)** | `median_house_value` (USD, 1990) |
| **Inputs (X)** | `median_income`, `housing_median_age`, `total_rooms`, `total_bedrooms`, `population`, `households`, `latitude`, `longitude` (+ 3 engineered ratios in model M2) |

## 3. Work completed
1. Problem framing (user, decision, X, y, success metric).
2. Data inspection and cleaning: 207 rows with missing `total_bedrooms` dropped; 958 rows with the target capped at \$500,001 dropped (censored values); duplicates checked (none).
3. EDA with written observations: distributions, scatter plots, correlation heatmap, feature–feature correlations, geographic map; note that correlation ≠ causation.
4. **Manual worksheet** on 6 observations and 2 features: design matrix, XᵀX, Xᵀy, determinant, inverse, solved coefficients, worked prediction and residual, verified against scikit-learn (`manual_calculation_worksheet.md` is also generated).
5. 80:20 split (15,580 train / 3,895 test, `random_state=42`); scaler fitted on training data only.
6. Models: mean baseline → single-feature (income) → **M1** (8 raw features) → **M2** (7 features, redundant counts combined into per-household ratios).
7. Coefficient interpretation (raw and standardised), intercept discussion, test-set predictions, MAE / MSE / RMSE / R² / adjusted R².
8. Residual analysis (residual plot, histogram, predicted-vs-actual with 45° line, error by price band, residual map), Breusch–Pagan test, VIF, assumption check.
9. New prediction inside the training range, conclusion, decision log, limitations, ethics, expansion ideas.

## 4. Key results (test set, 3,895 unseen neighbourhoods)
| Model | Features | Test MAE (USD) | Test RMSE (USD) | Test R² | Test adj. R² |
|---|---|---|---|---|---|
| Baseline: predict training mean | 0 | 79,594 | 98,424 | −0.000 | −0.000 |
| Single feature (`median_income`) | 1 | 56,826 | 74,280 | 0.430 | 0.430 |
| **M1** – 8 raw features | 8 | 45,916 | 61,823 | 0.605 | 0.605 |
| **M2** – 7 refined features (recommended) | 7 | 46,859 | 62,950 | 0.591 | 0.590 |

* Income is the strongest single clue; adding more features improves RMSE by about \$12k over the single-feature model, and the training / cross-validation / test R² are close (no overfitting).
* **Multicollinearity:** in M1 the VIF of `total_bedrooms` is about 37 and of `households` about 35, with sign flips in coefficients. Combining the counts into ratios (M2) brings the largest VIF to about 9.4 at a cost of about \$1.1k in RMSE – *multicollinearity hurt interpretation, not prediction*.
* **Where the model fails:** it over-predicts cheap and under-predicts expensive neighbourhoods; errors are clustered geographically; a few predictions are negative.
* **Limits:** 1990 data (do not use for today's prices), no neighbourhoods above \$500k, neighbourhood medians (not single houses), associations not causes.

## 5. Repository structure
```
├── Activity1_Multiple_Linear_Regression_California_Housing.ipynb   # executed notebook with outputs
├── manual_calculation_worksheet.md                                 # typed manual worksheet
├── data/housing.csv
├── images/                                                         # figures saved by the notebook
├── results/model_comparison.csv
├── requirements.txt
└── README.md
```

## 6. How to run
```bash
pip install -r requirements.txt
jupyter notebook Activity1_Multiple_Linear_Regression_California_Housing.ipynb   # Kernel → Restart & Run All
```
Tested with Python 3.12, scikit-learn 1.9, pandas 3.0, numpy 2.5, statsmodels 0.15, matplotlib 3.11, seaborn 0.13. Random seed 42 everywhere.

## 7. Ideas to extend this project
Ridge/Lasso, log-transformed target, `ocean_proximity` and distance-to-coast features, tree-based models, handling the censored target properly, spatial cross-validation, a Streamlit valuation app.
