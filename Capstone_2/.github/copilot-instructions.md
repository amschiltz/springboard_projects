


# Telco Customer Churn – AI Agent Instructions

This project predicts telco customer churn using a reproducible, notebook-driven pipeline. The codebase is structured for clarity, interpretability, and academic rigor.

## 🏗️ Architecture & Data Flow

**Pipeline:**
```
data/raw/ → notebooks/01_* (wrangling) → data/processed/ → notebooks/02_* (EDA) → reports/eda_summary_template.md → notebooks/03_* (preprocessing/modeling) → reports/figures/ → deliverables/
```
- All data wrangling and feature engineering: `notebooks/01_*`, outputs to `data/processed/`
- EDA: `notebooks/02_*`, summary templates in `reports/`
- Preprocessing/modeling: `notebooks/03_*`, `notebooks/04_*`
- Figures/outputs: `reports/figures/`, `deliverables/`

## 🛠️ Developer & Notebook Workflows

- Always convert `TotalCharges` to numeric; drop NaN (new customers, tenure=0, none churned)
- Map `SeniorCitizen` 1/0 → 'Yes'/'No'
- All categorical columns: `.astype('category')`
- Engineered features: use `*_eng` suffix (e.g., `tenure_group_eng`)
- Use `references/telco_column_role_manifest.csv` to define column roles/types
- Save all plots to `reports/figures/` with descriptive filenames
- Modeling: baseline (majority class) → logistic regression → advanced models
- Address class imbalance (prefer class weights or sampling)
- Prioritize recall and ROC-AUC in evaluation
- Use `experiments/` for YData profiling and scratch work (see `experiments/ydata_test.ipynb`)

## 📚 Project Conventions & Patterns

- EDA template: Target audit → Univariate → Bivariate → Multivariate
- Engineered features: `*_eng` suffix; keep raw/cleaned/encoded versions in `data/processed/`
- All modeling/preprocessing in `notebooks/03_*` and `notebooks/04_*`; keep code modular and reproducible
- Only use columns defined in the manifest for modeling

## ⚠️ Project-Specific Gotchas

- Do not use `TotalCharges` for new customers (tenure=0) – always NaN, none churned
- Always check for class imbalance before modeling
- Save all outputs/figures in correct subfolders for reproducibility

## 🔑 Key Files & Examples

- `notebooks/01_telco_churn_data_wrangling.ipynb`: Data cleaning/feature engineering
- `notebooks/02_telco_churn_eda_ams.ipynb`: EDA workflow/summary
- `notebooks/03_telco_churn_pre-processing_and_Training_Data_Development.ipynb`: Preprocessing/train-test split
- `notebooks/04_telco_churn_modeling.ipynb`: Modeling/evaluation
- `references/telco_column_role_manifest.csv`: Column roles/types
- `reports/eda_summary_template.md`: EDA reporting template
- `experiments/ydata_test.ipynb`: YData profiling example

## 🧭 Additional Guidance for AI Agents

- **Notebook-driven workflow:** All major steps are in Jupyter notebooks, not scripts. Keep code modular and reproducible within notebooks.
- **Data roles manifest:** Always reference `references/telco_column_role_manifest.csv` for feature selection and type enforcement.
- **Feature engineering:** Use the `*_eng` suffix for engineered features. Maintain both raw and processed versions in `data/processed/`.
- **Plotting:** All plots must be saved to `reports/figures/` with descriptive, context-specific filenames (not just "plot.png").
- **Modeling:** Start with a baseline (majority class), then logistic regression, then advanced models. Address class imbalance using class weights or sampling. Evaluate with recall and ROC-AUC.
- **YData profiling:** Use `experiments/ydata_test.ipynb` as a template for profiling workflows.
- **Reproducibility:** Outputs, figures, and processed data must be saved in the correct subfolders for traceability.
- **Column usage:** Only use columns defined in the manifest for modeling and analysis.

For unclear or missing conventions, consult the above files or ask for clarification.
