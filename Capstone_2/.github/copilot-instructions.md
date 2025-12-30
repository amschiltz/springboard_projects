
# Telco Customer Churn – AI Agent Instructions

This project is a Springboard Data Science Capstone focused on telco customer churn prediction. The codebase is organized for reproducible, interpretable analysis and academic deliverables.

## 🏗️ Architecture & Data Flow

**Data pipeline:**
```
data/raw/ → notebooks/01_* (wrangling) → data/processed/ → notebooks/02_* (EDA) → reports/eda_summary_template.md → notebooks/03_* (modeling) → reports/figures/ → deliverables/
```
- **Raw data is immutable.** All transformations are in notebooks, outputs saved to `data/processed/`.
- **Absolute paths** are required for all data loading (see notebook examples).
- **Key dataset:** `data/raw/Telco-Customer-Churn-Blastchar.csv`

## 🛠️ Developer & Notebook Workflows

- **Wrangling:**
  - `TotalCharges`: Convert to numeric, drop NaN (new customers, tenure=0, none churned)
  - `SeniorCitizen`: Map 1/0 → 'Yes'/'No'
  - All categoricals: `.astype('category')`
  - Feature engineering: Use `*_eng` suffix (e.g., `tenure_group_eng`)
- **EDA:**
  - Follow template: Target audit → Univariate → Bivariate → Multivariate
  - Use `references/telco_column_role_manifest_template.csv` for column roles
  - Save plots in `reports/figures/` with descriptive names
- **Modeling:**
  - Start with baseline (majority class), then logistic regression, then advanced models
  - Address class imbalance (class weights or sampling)
  - Prioritize recall and ROC-AUC
- **Profiling:**
  - Use `experiments/` for YData profiling and scratch work
  - See `experiments/ydata_test.ipynb` for `show_profile_in_browser()` pattern

## 📚 Project Conventions & Patterns

- **Column roles:** Always reference `references/telco_column_role_manifest_template.csv`
- **Data dictionaries:** Generate after wrangling (see `data/interim/data_dictionary_raw.csv`)
- **Changelog cells:** Major DataFrame changes must be documented in markdown
- **Templates:** Fill out `reports/*_template.md` as you progress
- **Reproducibility:** Set `SEED = 42` in all notebooks
- **Deliverables:** Final outputs in `deliverables/` (notebooks, PDF, slides)

## ⚠️ Project-Specific Gotchas

- `TotalCharges` is a string with whitespace/nulls in raw data
- Service columns mix 'Yes'/'No' and 'No internet service'
- Contract type is a key churn predictor
- Always use absolute paths for data

## 🧑‍💻 AI Agent Behavior (Study Assistant Mode)

- **Coach, don’t complete:** Never generate full assignment solutions or final analyses
- **Explain before code:** Clarify concepts, ask before showing code
- **No auto-edits:** Always show suggestions for review first
- **Emphasize interpretation:** Focus on reasoning, not just output
- **Stay in scope:** EDA, data cleaning, diagnostics, but not assignment completion

## 🔑 Key Files & Examples

- `notebooks/01_telco_churn_data_wrangling.ipynb`: Data cleaning, feature engineering
- `notebooks/02_telco_churn_eda_ams.ipynb`: EDA structure and plotting
- `notebooks/03_telco_churn_modeling_outline.ipynb`: Modeling workflow
- `references/telco_column_role_manifest_template.csv`: Column roles
- `experiments/ydata_test.ipynb`: Profiling pattern

---
For unclear or missing conventions, consult the above files or ask for clarification.
