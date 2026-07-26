# Credit Card Fraud Detection — Business-Oriented Threshold Optimization

## Executive Summary
This project builds a credit card fraud detection system focused on decision quality, not just raw model accuracy. Three models (Logistic Regression, Random Forest, XGBoost) were trained on a highly imbalanced dataset (~0.17% fraud), with decision thresholds tuned on a held-out validation set and final performance reported on a completely unseen test set — reflecting realistic production evaluation practice.

## Business Problem
In fraud detection, a model that simply maximizes accuracy is close to useless, since fraud is extremely rare. The real business challenge is balancing two costs: missing actual fraud (recall) versus generating false alerts that create customer friction and investigation overhead (precision). This project explicitly optimizes that trade-off rather than defaulting to a standard 0.5 classification threshold.

## Business Objectives
- Detect fraudulent transactions in a highly imbalanced dataset.
- Minimize false alerts to preserve customer experience.
- Identify temporal and behavioral fraud patterns to support monitoring strategy.
- Select and justify a production-ready decision threshold based on validation data, then confirm performance on a fully independent test set.

## Dataset
Kaggle's anonymized credit card transactions dataset (ULB), containing 284,807 transactions with 31 features (28 PCA components, Time, Amount, and the Class label). After removing 1,081 exact duplicate rows, the dataset used for analysis and modeling contains **283,726 transactions, of which 473 are confirmed fraud (~0.167%)** — a fraud-to-legitimate ratio of roughly 1:599.

## Project Workflow
1. **Data Cleaning** — Verified no missing values; identified and removed 1,081 duplicate rows.
2. **Exploratory Data Analysis** — Analyzed class imbalance, transaction amount distributions by class, temporal patterns (hourly transaction volume and fraud rate), and feature correlation with the fraud label.
3. **Data Splitting** — Stratified train/validation/test split (60/20/20) to preserve the fraud ratio across all three sets.
4. **Modeling** — Trained and tuned three classifiers via GridSearchCV with stratified cross-validation:
   - Logistic Regression (class-weighted)
   - Random Forest (combined with SMOTE oversampling inside the training pipeline)
   - XGBoost (class-weighted via `scale_pos_weight`)
5. **Threshold Optimization** — For each model, selected the classification threshold that maximizes F1-score on the validation set (not the default 0.5).
6. **Final Evaluation** — Applied each model's tuned threshold to the untouched test set to obtain the final, unbiased performance metrics.
7. **Overfitting Check** — Compared train vs. test performance for the top models to confirm generalization.

## Technologies Used
- **Python**, **Pandas**, **NumPy**
- **Scikit-learn** (Logistic Regression, Random Forest, GridSearchCV, threshold tuning, metrics)
- **XGBoost**
- **imbalanced-learn** (SMOTE, used within the Random Forest pipeline)
- **Matplotlib**, **Seaborn**, **Plotly**

## Results
Final performance on the held-out test set, using each model's validation-tuned threshold:

| Model | ROC-AUC | Precision | Recall | F1-Score | Threshold |
|---|---|---|---|---|---|
| **XGBoost** | 0.971 | **0.972** | 0.726 | **0.831** | 0.960 |
| Random Forest | **0.973** | 0.873 | 0.653 | 0.747 | 0.881 |
| Logistic Regression | 0.965 | 0.625 | **0.789** | 0.698 | 0.990 |

XGBoost was selected as the final model for its best precision–F1 balance: at the 0.96 threshold, roughly **97 out of every 100 flagged transactions are genuine fraud**, and the model catches about **73% of all fraud cases** in the test set — a deliberately precision-oriented configuration that minimizes false alerts while still catching most fraud.

**EDA highlights:**
- Fraud transactions skew toward lower amounts (median around $20) with substantial overlap with legitimate transactions, meaning amount alone is not a reliable fraud signal.
- Fraud maintains a low but persistent presence during early morning hours (roughly 0–5 AM), a period of low overall transaction volume — this pattern is directionally interesting but based on small absolute counts and should be validated on larger data before being used to set time-based rules.
- Several PCA components (notably V14, V17, V12, V10) show the strongest linear correlation with the fraud label.

## Business Insights
- Precision and recall involve a real trade-off in this problem: the three models represent genuinely different operating points (Logistic Regression favors recall, XGBoost favors precision), which is more actionable for business decision-making than comparing ROC-AUC alone.
- ROC-AUC does not reliably indicate operational quality — Random Forest and XGBoost have similar ROC-AUC, but XGBoost's F1-score is meaningfully higher at its chosen threshold.
- Reporting validation-set metrics as final results would have overstated recall by roughly 10 percentage points relative to true test-set performance — reinforcing why threshold selection and final evaluation must use separate data splits.

## Business Recommendations
- Deploy XGBoost with the 0.96 threshold as a high-confidence fraud filter, given its strong precision (minimizing customer friction from false alerts).
- Because roughly 27% of fraud is still missed at this threshold, pair the model with a secondary, higher-recall layer (e.g., a lower-threshold Logistic Regression alert queue) for manual review of borderline cases, rather than relying on a single model/threshold.
- Treat the early-morning fraud pattern as a monitoring hypothesis to validate with more data, not yet as an automated time-based rule, given the small sample size in those hours.


## Future Improvements
- Report final metrics exclusively from the test set in all public-facing summaries (validation metrics should only be used internally for threshold selection).
- Add model-based feature importance (e.g., XGBoost's built-in importances or SHAP values) to directly support the claim that V14/V17/V12 drive predictions, rather than relying only on raw correlation.
- Extend the early-morning fraud-hour analysis with a larger or supplementary dataset to validate statistical significance.
- Package the pipeline into reusable functions/scripts for retraining and monitoring.
- Explore a two-tier alerting system (high-precision auto-block + high-recall manual review queue) as outlined in the recommendations above.

## Author
ABDELATIF OUARDA — Data Analyst | Machine Learning
LinkedIn: [linkedin.com/in/abdelatif-ouarda-790a2b36a](https://www.linkedin.com/in/abdelatif-ouarda-790a2b36a)
