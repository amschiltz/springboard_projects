# Final Report — Telco Customer Churn Prediction

## 1. Introduction

### Problem Statement
Customer churn reduces revenue and increases acquisition costs. This project builds a reproducible, interpretable predictive model to identify customers most at risk of churning so that retention efforts can be prioritized where they have the greatest impact.

### Success Criteria
The primary objectives are to identify at least 70% of actual churners (recall ≥ 0.7) while maintaining precision ≥ 0.5. I also track ROC-AUC and PR-AUC to assess overall discriminative performance. Threshold selection centers on recall: the operating cutoff was selected on pooled out-of-fold validation probabilities to maximize recall while maintaining precision ≥ 0.5 (final threshold ≈ 0.486).

### Stakeholders
The model’s outputs are intended to support decision-making across:
- Executive leadership (strategy and forecasting)
- Customer retention operations (risk-based targeting)
- Marketing (segmentation and outreach design)
- Finance/analytics (ROI evaluation and budgeting)

---

## 2. Data

This analysis uses the **Telco Customer Churn** dataset from Kaggle, which contains **7,043 customer records and 21 original features**. Each record represents a single customer account.

The features can be grouped into four primary domains:

- **Demographics:** gender, `SeniorCitizen`, `Partner`, `Dependents`
- **Account and contract characteristics:** tenure, `Contract`, `PaperlessBilling`, `PaymentMethod`
- **Services:** `PhoneService`, `MultipleLines`, `InternetService`, and related add-on services (`OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`)
- **Billing information:** `MonthlyCharges` and `TotalCharges`

The target variable is **Churn**, a binary indicator (Yes/No) identifying customers who discontinued service during the prior month. Numeric features include tenure, `MonthlyCharges`, and `TotalCharges`; all remaining variables are categorical.

The dataset contains no explicit date fields. Account tenure serves as the sole temporal indicator, limiting the analysis to a cross-sectional view rather than a time-series framework. The target variable is moderately imbalanced, with an overall churn rate of **26.6%**, which informed subsequent evaluation metric selection.

Column roles and data types are governed by a predefined manifest (telco_column_role_manifest.csv). Detailed variable definitions are provided in the raw (telco_raw_data_dictionary.csv) and cleaned (telco_wrangling_cleaned_data_dict.csv) data dictionaries for transparency and reproducibility.
---

## 3. Methods

### 3.1 Data Wrangling 

Initial data audits were conducted to assess structure, completeness, and consistency across all variables. This included review of summary statistics, missingness patterns, categorical value domains, and duplicate records.

No constant columns or duplicate rows were identified.

Data types were standardized by converting `TotalCharges` to numeric (coercing invalid values to NaN) and casting all categorical variables to categorical data types. The `SeniorCitizen` indicator was mapped from binary values (0/1) to human-readable labels (Yes/No).

Rows with missing `TotalCharges` values (n = 11) were removed. These observations corresponded to new customers with tenure equal to zero and did not provide meaningful information for churn modeling.

Outlier and consistency checks were performed using interquartile range (IQR) methods for numeric variables, along with validation against business rules (e.g., customers without internet service should not have internet add-ons). No outliers, impossible values, rare categorical levels, or business-rule contradictions were detected, and no category consolidation was required.

To support downstream analysis and modeling, several engineered features were created:

- `tenure_group_eng`: tenure binned into 0–6, 7–12, 13–24, 25–48, and 49+ months

- `total_services_eng`: count of subscribed services

- `monthly_charges_bin_eng`: MonthlyCharges binned into Low, Medium, and High

- `is_month_to_month_eng`: indicator for month-to-month contracts

- `short_tenure_month_to_month_eng`: flag for customers with short tenure on month-to-month contracts

- `has_streaming_eng`: indicator for any streaming service subscription

Exploratory visualizations (histograms and boxplots) were used to verify distributions and confirm the effects of transformations and feature engineering.

All steps were executed with fixed random seeds (SEED = 42) to ensure reproducibility.

The final analytical dataset contains **7,032 customer records and 27 features**, including all 21 original variables and 6 engineered features. The cleaned dataset and accompanying data dictionary were saved for reproducibility and future use.

The cleaned dataset and accompanying data dictionary were saved to support reproducibility and downstream analysis (`telco_wrangling_cleaned.csv` and `telco_wrangling_cleaned_data_dict.csv`).


### 3.2 Exploratory Data Analysis
#### Target Variable

The churn rate is **26.6%**, indicating moderate class imbalance. This informed subsequent metric selection and threshold design.

---

#### Univariate Patterns

Key distributions include:

- **Tenure:** Right-skewed, with many customers in early tenure stages.
- **MonthlyCharges:** Moderately right-skewed.
- **TotalCharges:** Strongly right-skewed, reflecting cumulative billing.
- **Contract type:** Month-to-month contracts are most common, while long-term contracts concentrate among customers with 49+ months of tenure.

These patterns suggest meaningful behavioral segmentation by contract length and tenure.

---

#### Bivariate Relationships with Churn

Effect size analysis identified:

- **Strongest numeric predictor:** `tenure` (Cohen’s d = -0.857), indicating substantially lower churn among long-tenure customers.
- **Strongest categorical predictor:** `Contract` (Cramér’s V = 0.410), reflecting materially different churn rates across contract types.

Among numeric variables, tenure was the only predictor demonstrating a large effect size. Most categorical variables showed weak to moderate associations.

Several predictors exhibited negligible association with churn (`gender`, `PhoneService`, `MultipleLines`, `has_streaming_eng`) and were evaluated for potential exclusion during feature simplification.

---

#### Multivariate Relationships & Collinearity

Several strong correlations were observed:

- **Tenure–TotalCharges:** r ≈ 0.83  
- **MonthlyCharges–total_services_eng:** r ≈ 0.80  
- **InternetService–MonthlyCharges:** η ≈ 0.906  

These relationships indicate substantial redundancy among billing and service features.

An interaction between short tenure and month-to-month contracts revealed a particularly high-risk segment: customers with short tenure on month-to-month contracts exhibit a churn rate of **51%**, nearly twice the overall churn rate (26.6%).

No evidence of target leakage or proxy variables was identified.

---

#### Feature Simplification Strategy

Based on collinearity analysis and effect size screening, redundant billing variables, negligible predictors, and highly correlated composite indicators were removed in alternative feature specifications.

Created three alternative ('pruned') feature sets from the remaining variables with the following variations to reduce redundancy and collinearity:

- "Monthly Charges": Included binned monthly charges  (`monthly_charges_binned`) and representative add-on service (`Online Security`). Excluded `InternetService` and all other add-ons because `InternetService` was highly correlated with 'monthly_charges_binned' and the internet add-ons are all highly correlated with `InternetService` and each other.
- "Internet Service": Included `InternetService` and excluded monthly charges and all add-ons, to address collinearity issues described above.
- "All Add-Ons": Included monthly charges plus all add-ons and excluded `InternetService`. The goal was to minimize some collinearity while trying to capture any add-ons of particular value.

Cross-validated model comparisons were used to ensure simplification did not degrade performance relative to project targets (recall ≥ 0.70, precision ≥ 0.50, ROC-AUC > 0.75).

---

#### Modeling Implications

Given the predominance of low-cardinality categorical predictors, logistic regression provides a strong, interpretable baseline. Gradient-boosted tree-based models (i.e., LightGBM) were evaluated to capture potential nonlinearities and interaction effects.

Cross-validation was used to assess stability and confirm that feature pruning maintained performance at the defined operating threshold.

When performance differences were negligible under cross-validation, the simpler model was preferred to improve interpretability and reduce the risk of multicollinearity.

---

### 3.3 Modeling Approach

Model development followed a structured progression:

1. **Baseline benchmark** using a stratified baseline classifier to establish a minimum performance reference.
2. **Logistic regression** to provide interpretability and establish a transparent, explainable baseline.
3. **Advanced models** (e.g., gradient boosting) to improve predictive performance.

Class imbalance was addressed using model-based class weighting rather than resampling.

Evaluation was conducted using both threshold-independent metrics (**ROC-AUC**, **Average Precision**) and business-aligned operating criteria (recall maximized subject to a precision constraint). Operating thresholds were selected using cross-validated out-of-fold predictions to reduce optimistic bias. Model selection was based on cross-validation performance, with final validation performed on a held-out test set.

All analysis was conducted in modular, reproducible Jupyter notebooks using only features defined in the project column manifest. Outputs, tables, and figures were saved to support transparency and reproducibility.

---
#### Feature Set Evaluation

To assess the impact of redundancy and multicollinearity on model performance and interpretability, four feature configurations were evaluated.  

1. **All Features:** Full set of engineered and original predictors.
2. **Monthly Charges:** Includes binned monthly charges and retains a representative add-on (`OnlineSecurity`).
3. **Internet-Service Simplified Set:** Retains InternetService while removing correlated monthly charges and add-on features.
4. **Add-On Services:** Keeps monthly charges and all add-on services and drops correlated InternetService.

These configurations were designed to test whether simpler, more interpretable representations could maintain predictive performance relative to the full feature set.

Each feature set was evaluated using Logistic Regression and LightGBM models under identical cross-validation procedures. The stratified dummy baseline was evaluated only on the full feature set.

In total, nine models were trained and compared.


## 4. Results

### Validation / Test Metrics

#### Stratified Baseline Classifier

As shown below, the stratified baseline classifier performed as expected: recall, precision, and PR-AUC (average precision) are close to the overall churn rate (0.266). The ROC-AUC is 0.5074, no better than chance at discriminating churners vs non-churners.

- Recall: 0.2781
- Precision: 0.2766
- ROC-AUC: 0.5074
- PR-AUC (Average Precision): 0.2688



|   Recall (OOF @ thr) |   Precision (OOF @ thr) |   ROC-AUC (OOF) |   PR-AUC/AP (OOF) |   Threshold |
|---------------------:|------------------------:|----------------:|------------------:|------------:|
|               0.8241 |                  0.5087 |          0.8466 |            0.6565 |      0.5000 |
|               0.8174 |                  0.5002 |          0.8337 |            0.6174 |      0.5000 |
|               0.8147 |                  0.4957 |          0.8354 |            0.6202 |      0.5000 |
|               0.8120 |                  0.4992 |          0.8361 |            0.6289 |      0.5000 |
|               0.8033 |                  0.5254 |          0.8463 |            0.6500 |      0.5000 |
|               0.8020 |                  0.5250 |          0.8487 |            0.6627 |      0.5000 |
|               0.8020 |                  0.5159 |          0.8417 |            0.6349 |      0.5000 |
|               0.7973 |                  0.5203 |          0.8432 |            0.6390 |      0.5000 |



- Recall: 0.809 / 0.797
- Precision: 0.529 / 0.492
- ROC-AUC: 0.847 / 0.835
- Classification threshold: ≈ 0.486 (operating; selected on pooled out-of-fold validation to maximize recall subject to precision ≥ 0.5); 0.50 (benchmarking)

### Confusion Matrix (Percent of instances)
- Validation: TN 73.9, FP 26.1, FN 18.26, TP 81.74
- Test: TN 70.18, FP 29.82, FN 20.32, TP 79.68

### Threshold Selection Rationale (LogReg – All Add-Ons)
- 0.45 → Recall 0.84, Precision 0.51, ROC-AUC 0.77 (higher recall, slightly lower precision)
- 0.486 → Recall 0.817, Precision 0.520 (OOF-selected operating threshold: maximizes recall subject to precision ≥ 0.5; applied once to the holdout test set)
- 0.50 → Recall 0.81, Precision 0.53, ROC-AUC 0.77 (benchmark cutoff used for comparability)
- 0.55 → Recall 0.76, Precision 0.55, ROC-AUC 0.77 (precision improves, recall drops)
- 0.60 → Recall 0.71, Precision 0.57, ROC-AUC 0.76 (highest precision among listed, recall below target)

### Model Configuration (Final Logistic Regression)
- C: 0.1, Penalty: l2, Solver: liblinear, Class weight: balanced, Max iterations: 1000

### Business Interpretation of Feature Importances
- Retention factors: longer contract terms (1-year, 2-year) and higher tenure (7+ months) are associated with lower churn; the Online Security add-on shows the strongest retention effect among add-ons, likely by addressing data protection and safety concerns.
- Churn factors: fiber optic internet service, electronic payment method, and multiple phone lines are the strongest predictors of churn, representing distinct segments that warrant targeted retention strategies.

Top coefficients (directional effects):
- Negative (retention): Two-year contract, Tenure 49+, Tenure 25–48, Tenure 13–24, One-year contract, Tenure 7–12, Online Security, Tech Support, Dependents
- Positive (churn risk): Fiber Optic internet, Electronic payment, Multiple lines, Streaming TV/Movies, Paperless billing, Senior citizen

### Visualizations
- ROC curve and feature importance plots

---

## 5. Discussion
The model was optimized for higher recall to capture at-risk customers, which necessarily lowers precision at the chosen operating threshold. Operationally, this means retention teams will engage more customers, increasing intervention costs, but with greater potential lift in prevented churn. A cost–benefit review should benchmark the per-contact cost against expected retention uplift to calibrate threshold selection and targeting rules.

- Insights: What factors are most related to churn?
- Trade-offs between recall, precision, and business use case
- Risks, limitations, and data caveats
- Potential next steps (e.g., additional data, operationalization, A/B testing)

### Limitations

- Optimizing for recall at a relatively low operating threshold increases false positives, reducing precision.
- Class imbalance was handled via class weights rather than sampling; alternative strategies (e.g., SMOTE or cost-sensitive learning) were not explored.
- Results are based on a single public dataset and may not generalize to other industries or customer populations.

---

## 6. Recommendations
- Concrete retention strategies based on model outputs
- Suggested monitoring and evaluation process if deployed
- Communication plan for stakeholders (executives vs technical teams)

The model indicates several actionable strategies to reduce customer churn:

- Incentivize longer contracts and reward customers for extended tenure, as these factors are associated with higher retention.
- For fiber optic internet customers—who often have higher expectations for speed and reliability—investigate and address any service quality issues. Proactively improving reliability and communicating enhancements can help retain this segment.
- Customers with multiple phone lines, such as families or businesses, are typically more sensitive to costs and service quality. Offer cost-effective bundles and regularly benchmark against competitors to ensure your offerings remain attractive.
- For customers who use electronic payment methods, consider targeted retention efforts. This segment may be more likely to churn due to the ease of switching providers. Implement loyalty programs, personalized offers, or exclusive benefits for electronic payment users to increase engagement and reduce churn risk. Additionally, monitor for any friction in the electronic payment process and ensure it is seamless and secure.

By tailoring retention strategies to these key segments, the company can more effectively address the drivers of churn and improve overall customer loyalty.

### Recommendations

I recommend incentives for longer contracts, reliability improvements for fiber customers, tailored bundles for multi-line accounts, and targeted offers for electronic payment users.

---

## 7. Appendices
- Data dictionary (processed dataset)
- Full metrics table (baseline vs advanced models)
- Hyperparameter search notes
- Links to code, notebooks, and figures

---
