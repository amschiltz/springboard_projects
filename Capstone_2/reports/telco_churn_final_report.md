# Final Report — Telco Customer Churn Prediction

## 1. Introduction

### Problem Statement
Customer churn reduces revenue and increases acquisition costs. This project builds a reproducible, interpretable predictive model to identify customers most at risk of churning so that retention efforts can be prioritized where they have the greatest impact.

### Success Criteria
The primary objectives are to identify at least 70% of actual churners (recall ≥ 0.7) while maintaining precision ≥ 0.5. I also track ROC-AUC and PR-AUC to assess overall discriminative performance. Threshold selection centers on recall: the operating cutoff was selected on pooled out-of-fold validation probabilities to maximize recall while maintaining precision ≥ 0.5 (final threshold ≈ 0.486). For benchmarking and comparability, results are also reported at a standard 0.50 cutoff.

### Stakeholders
Executive leadership, customer service/retention operations, and marketing rely on churn insights to guide decisions, allocate resources, and design targeted interventions. Analytics and finance teams use model outputs to forecast risk and quantify ROI of retention programs.

### Approach
I begin with a baseline majority-class benchmark, then implement logistic regression for interpretability, followed by advanced models to enhance performance. Class imbalance is addressed using model class weights; evaluation focuses on recall and ROC-AUC. All work is executed in modular, reproducible Jupyter notebooks, using only features defined in the project’s column manifest, with outputs and figures saved for transparency.

Data source: the analysis uses the Telco Customer Churn dataset (Kaggle). 

Limitations: optimizing for recall at a 0.5 probability threshold lowers precision; class imbalance is handled via class weights rather than sampling.
Limitations: optimizing for recall at a relatively low operating threshold can lower precision; class imbalance is handled via class weights rather than sampling.

### Key Results

At a standard 0.50 cutoff (used for benchmarking), the final model achieved recall 0.81, precision 0.53, and ROC-AUC 0.77, surpassing the recall target and meeting the precision benchmark. The operational cutoff was selected separately via pooled out-of-fold validation to maximize recall subject to precision ≥ 0.5 (final threshold ≈ 0.486).

Retention signals include longer contracts (1-year, 2-year), higher tenure (7–12, 13–24, 25–48, 48+ months), and the Online Security add-on. 

Churn signals include fiber optic internet service, electronic payment method, and multiple phone lines.

### Recommendations

I recommend incentives for longer contracts, reliability improvements for fiber customers, tailored bundles for multi-line accounts, and targeted offers for electronic payment users.

---

## 2. Data Description

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

## 4. Methods

### 4.1 Data Wrangling 

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


### 4.2 Exploratory Data Analysis
- Distribution of churn vs non-churn
In the exploratory data analysis I found that 
- Notable trends and relationships
- Key potential drivers of churn

### 4.3 Modeling
- Baseline model (logistic regression)
- Advanced model(s) (e.g., Gradient Boosting)
- Preprocessing pipeline summary (encoding, scaling, class imbalance handling)

---

## 5. Results
### Metrics (Validation / Test)
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

## 6. Discussion
The model was optimized for higher recall to capture at-risk customers, which necessarily lowers precision at the chosen operating threshold. Operationally, this means retention teams will engage more customers, increasing intervention costs, but with greater potential lift in prevented churn. A cost–benefit review should benchmark the per-contact cost against expected retention uplift to calibrate threshold selection and targeting rules.

- Insights: What factors are most related to churn?
- Trade-offs between recall, precision, and business use case
- Risks, limitations, and data caveats
- Potential next steps (e.g., additional data, operationalization, A/B testing)

---

## 7. Recommendations
- Concrete retention strategies based on model outputs
- Suggested monitoring and evaluation process if deployed
- Communication plan for stakeholders (executives vs technical teams)

The model indicates several actionable strategies to reduce customer churn:

- Incentivize longer contracts and reward customers for extended tenure, as these factors are associated with higher retention.
- For fiber optic internet customers—who often have higher expectations for speed and reliability—investigate and address any service quality issues. Proactively improving reliability and communicating enhancements can help retain this segment.
- Customers with multiple phone lines, such as families or businesses, are typically more sensitive to costs and service quality. Offer cost-effective bundles and regularly benchmark against competitors to ensure your offerings remain attractive.
- For customers who use electronic payment methods, consider targeted retention efforts. This segment may be more likely to churn due to the ease of switching providers. Implement loyalty programs, personalized offers, or exclusive benefits for electronic payment users to increase engagement and reduce churn risk. Additionally, monitor for any friction in the electronic payment process and ensure it is seamless and secure.

By tailoring retention strategies to these key segments, the company can more effectively address the drivers of churn and improve overall customer loyalty.

---

## 8. Appendices
- Data dictionary (processed dataset)
- Full metrics table (baseline vs advanced models)
- Hyperparameter search notes
- Links to code, notebooks, and figures

---
