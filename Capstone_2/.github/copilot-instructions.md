
# Telco Customer Churn – AI Agent Instructions

This project is a Springboard Data Science Capstone focused on telco customer churn prediction. The codebase is organized for reproducible, interpretable analysis and academic deliverables.

## 🏗️ Architecture & Data Flow

**Data pipeline:**
```
data/raw/ → notebooks/01_* (wrangling) → data/processed/ → notebooks/02_* (EDA) → reports/eda_summary_template.md → notebooks/03_* (modeling) → reports/figures/ → deliverables/
```

## 🛠️ Developer & Notebook Workflows

  - `TotalCharges`: Convert to numeric, drop NaN (new customers, tenure=0, none churned)
  - `SeniorCitizen`: Map 1/0 → 'Yes'/'No'
  - All categoricals: `.astype('category')`
  - Feature engineering: Use `*_eng` suffix (e.g., `tenure_group_eng`)
  - Follow template: Target audit → Univariate → Bivariate → Multivariate
  - Use `references/telco_column_role_manifest_template.csv` for column roles
  - Save plots in `reports/figures/` with descriptive names
  - Start with baseline (majority class), then logistic regression, then advanced models
  - Address class imbalance (class weights or sampling)
  - Prioritize recall and ROC-AUC
  - Use `experiments/` for YData profiling and scratch work
  - See `experiments/ydata_test.ipynb` for `show_profile_in_browser()` pattern

## 📚 Project Conventions & Patterns


## ⚠️ Project-Specific Gotchas


## 🧑‍💻 AI Agent Behavior (Study Assistant Mode)


## 🔑 Key Files & Examples


For unclear or missing conventions, consult the above files or ask for clarification.
