# EDA Summary Template — Telco Customer Churn

## Dataset Overview
- Rows: 7032
- Columns: 27

## Target Variable (`Churn`)
- Class distribution: 26.6% churned vs 73.4% retained

## Key Univariate Insights
- Tenure: Right-skewed; many early-tenure customers.
- MonthlyCharges: moderately right-skewed 
- TotalCharges: strongly right-skewed.
- Contract types: Month-to-month most common; long-term contracts concentrate in 49+ months.

## Bivariate Relationships with Churn
- STRONGEST numeric predictor: `tenure` (large effect, Cohen's d = -0.857)
- STRONGEST categorical predictor: `Contract` (relatively large effect, Cramér's V = 0.410)
- Among numeric predictors, `tenure` shows the only 'large' effect size
- Most categorical predictors show weak to moderate effects
- Negligible predictors to consider dropping: `gender`, `PhoneService`, `MultipleLines`

## Multivariate & Correlations
- Numeric correlations: Tenure–TotalCharges high (~0.83); MonthlyCharges–total_services_eng high (~0.80).
- Categorical associations: Contract, InternetService, and bundle/support features show strongest links.
- Interaction effects to explore: Early-tenure × month-to-month is compositionally concentrated; quantify churn lift.

## Potential Drivers of Churn
- Top categories by churn rate (positive churn): tenure_group_eng (0-6 months), PaymentMethod (Electronic check), Contract (Month-to-month), InternetService (Fiber optic), TechSupport (No)
- Bottom categories by churn rate (negative churn): monthly_charges_bin_eng (Low), tenure_group_eng (49+), InternetService (No), TechSupport (No internet service)
- Suspected proxies/leakage: None

## Next Steps for Modeling
- Features to engineer: To be determined after initial model assessment
- Candidate model families: Tree-based models
- Performance criteria reminders: Recall ≥ 0.70, ROC-AUC > 0.75

## Figures
- Churn-rate grid: ../reports/figures/eda_churn_rate_grid.png
- Numeric correlation heatmap: ../reports/figures/eda_numeric_correlation_heatmap.png
- Contract × tenure heatmap: ../reports/figures/eda_contract_by_tenure_rownorm.png
