# Abalone Ring Count Regression

Estimate abalone ring count from physical measurements and evaluate prediction errors against a mean-value baseline.

---

## Overview

* **Task:** Regression.
* **Dataset:** 4,177 samples, 8 features.
* **Selected Model:** **Random Forest** (holdout RMSE = 2.2241).
* **Baseline Comparison:** Dummy baseline achieved holdout RMSE = 3.2906.

Automated study completed on 2026-09-10; human review pending.

---

## Key Results

### Training Cross-Validation

| Model | RMSE (lower is better) | Fold SD |
| :--- | :--- | :--- |
| **Random Forest** | **2.1552** | 0.0792 |
| Ridge Regression | 2.2718 | 0.0931 |
| Dummy Baseline | 3.2092 | 0.0615 |

Model selection used 3-fold shuffled cross-validation on 3,341 training samples (seed 42). Preprocessing was fitted within each training fold. Fold SD describes variation across folds; it is not a confidence interval. The best CV score is subject to model-selection optimism.

### Final Holdout Evaluation (836 Samples)

| Model | RMSE | MAE | R² |
| :--- | :--- | :--- | :--- |
| **Random Forest** | **2.2241** | **1.5550** | **0.5430** |
| Dummy Baseline | 3.2906 | 2.3848 | -0.0003 |

The random holdout was evaluated only after model selection, using the frozen winner and dummy baseline. Lower RMSE and MAE are better; higher R² is better.

The largest holdout absolute error was **10.9879 rings**. The 30 largest absolute errors are recorded in `error_analysis.csv`.

The approximate 95% bootstrap interval for holdout RMSE was **2.0368–2.3987**, based on 200 resamples. It is conditional on the fitted model and split and excludes training and model-selection uncertainty.

---

## Data Summary

* **Source:** [UCI Abalone](https://archive.ics.uci.edu/dataset/1/abalone). Distributed under CC BY 4.0. Source attribution is recorded in `SOURCE.md`.
* **Data Quality:** 4,177 usable rows, 8 features, 0 missing cells, 0 duplicates removed.
* **Key Feature:** `shell_weight` showed the highest mean permutation importance drop (**2.1433 in negative RMSE** across 5 repeats). For regression, this is an equivalent increase in RMSE after shuffling. This measures model reliance, not a causal effect; correlated predictors can share importance.
* **Collinearity / Notes:** `length` and `diameter` had the strongest absolute Pearson correlation in the training data (**|r| = 0.987**). The target is ring count, not age directly. Observational measurements cannot establish how changing a feature would change age.

---

## Visualizations

| Feature Distributions | Training Cross-Validation |
| :---: | :---: |
| ![Feature Distributions](figures/distributions.png) | ![Training Cross-Validation](figures/model-comparison.png) |

| Holdout Predictions and Residuals | Permutation Feature Importance |
| :---: | :---: |
| ![Holdout Predictions and Residuals](figures/error-analysis.png) | ![Permutation Feature Importance](figures/feature-importance.png) |

---

## Repository Structure

```text
├── figures/                   # Diagnostic, performance, and distribution plots
├── analysis.ipynb             # Pipeline walkthrough; exported code cells are unexecuted
├── audit.json                 # Missingness, duplicates, exclusions, and usable row counts
├── data_dictionary.csv        # Column types, roles, missingness, and unique-value counts
├── descriptive_statistics.csv # Numeric summary statistics for the training features
├── error_analysis.csv         # The 30 largest holdout errors and residuals
├── feature_importance.csv     # Permutation importance means and standard deviations
├── metrics.json               # CV, holdout, and training scores
├── predictions.csv            # All holdout predictions, residuals, and absolute errors
├── project.json               # Dataset specification, configuration, versions, and hashes
├── splits.json                # Training, holdout, and cross-validation row indices
├── snapshot.csv.gz            # Exact analyzed dataset snapshot
├── SOURCE.md                  # Dataset source, license information, and snapshot checksum
├── LEARNING_NOTES.md          # Human review and experiment notes
├── report.html                # HTML report referencing the figure files
└── README.md
```
