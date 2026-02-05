# Final Report — Telco Customer Churn Prediction

## 1. Introduction
### Problem Statement
Customer churn reduces revenue and increases acquisition costs. This project builds a reproducible, interpretable predictive model to identify customers most at risk of churning so that retention efforts can be prioritized where they have the greatest impact.

### Success Criteria
The primary objectives are to identify at least 70% of actual churners (recall ≥ 0.7) while maintaining precision ≥ 0.5. We also track ROC-AUC to assess overall discriminative performance. Threshold selection centers on recall, with a 0.5 probability cutoff used for evaluation to meet business goals.

### Stakeholders
Executive leadership, customer service/retention operations, and marketing rely on churn insights to guide decisions, allocate resources, and design targeted interventions. Analytics and finance teams use model outputs to forecast risk and quantify ROI of retention programs.

### Approach
We begin with a baseline majority-class benchmark, then implement logistic regression for interpretability, followed by advanced models to enhance performance. Class imbalance is addressed using model class weights; evaluation focuses on recall and ROC-AUC. All work is executed in modular, reproducible Jupyter notebooks, using only features defined in the project’s column manifest, with outputs and figures saved for transparency.

Data source: the analysis uses the Telco Customer Churn dataset (Kaggle). 

Limitations: optimizing for recall at a 0.5 probability threshold lowers precision; class imbalance is handled via class weights rather than sampling.

**Key Results**
To meet business objectives, a 0.5 probability threshold was selected. The final model achieved recall 0.81, precision 0.53, and ROC-AUC 0.77 — surpassing the recall target and meeting the precision benchmark. 

Retention signals include longer contracts (1-year, 2-year), higher tenure (7–12, 13–24, 25–48, 48+ months), and the Online Security add-on. 

Churn signals include fiber optic internet service, electronic payment method, and multiple phone lines.

**Recommendations** 
We recommend incentives for longer contracts, reliability improvements for fiber customers, tailored bundles for multi-line accounts, and targeted offers for electronic payment users
---

## 2. Data Description

- Source and shape: Telco Customer Churn (Kaggle); 7,043 customers and 21 features.

- Feature groups: Demographics (gender, SeniorCitizen, Partner, Dependents); Account & Contract (tenure, Contract, PaperlessBilling, PaymentMethod); Services (PhoneService/MultipleLines, InternetService, add-ons: OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies); Billing (MonthlyCharges, TotalCharges).

- Roles and target: Numeric (tenure, MonthlyCharges, TotalCharges); Categorical (all others); Target: Churn (Yes/No).

- References: Column roles/types governed by the manifest [references/telco_column_role_manifest.csv](references/telco_column_role_manifest.csv); full dictionaries in [references/telco_raw_data_dictionary.csv](references/telco_raw_data_dictionary.csv) and [references/telco_wrangling_cleaned_data_dict.csv](references/telco_wrangling_cleaned_data_dict.csv).

- Scope notes: The dataset lacks explicit temporal context beyond account tenure. The target distribution is imbalanced and addressed via model class weights (see Methods). Detailed cleaning and engineered features (e.g., type conversions and *_eng) are documented in Data Wrangling; only manifest-defined columns are used for modeling.

---

## 4. Methods
### 4.1 Data Wrangling
- Key cleaning steps (missing values, type conversions, categorical standardization)
- Any assumptions made
- Final dataset size and features retained

### 4.2 Exploratory Data Analysis
- Distribution of churn vs non-churn
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
- Classification threshold: 0.5 (selected to meet recall ≥ 0.7 and precision ≥ 0.5 objectives)

### Confusion Matrix (Percent of instances)
- Validation: TN 73.9, FP 26.1, FN 18.26, TP 81.74
- Test: TN 70.18, FP 29.82, FN 20.32, TP 79.68

### Threshold Selection Rationale (LogReg – All Add-Ons)
- 0.45 → Recall 0.84, Precision 0.51, ROC-AUC 0.77 (higher recall, slightly lower precision)
- 0.50 → Recall 0.81, Precision 0.53, ROC-AUC 0.77 (chosen: balances business targets)
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
The model was optimized for higher recall to capture at-risk customers, which necessarily lowers precision at the chosen 0.5 threshold. Operationally, this means retention teams will engage more customers, increasing intervention costs, but with greater potential lift in prevented churn. A cost–benefit review should benchmark the per-contact cost against expected retention uplift to calibrate threshold selection and targeting rules.

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
