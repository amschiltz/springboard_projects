
# Copilot Instructions for GridSearchKNN_Case_Study

## Project Overview
This project is a self-contained Jupyter notebook case study for K-Nearest Neighbors (KNN) classification and hyperparameter tuning using grid search. The workflow is designed for reproducible, interactive data science analysis.

## Architecture & Data Flow
- **Single-notebook workflow:** All code, outputs, and documentation are in `GridSearchKNN_Case_Study.ipynb`.
- **Data location:** All datasets (e.g., `diabetes.csv`) are in the `data/` subfolder. Data is loaded with pandas using relative paths.
- **No modularization:** There are no Python modules, scripts, or cross-notebook imports—everything is in the notebook.

## Key Files
- `GridSearchKNN_Case_Study.ipynb`: Main notebook, contains all code and analysis steps.
- `data/diabetes.csv`: Primary dataset for the case study.

## Workflow & Conventions
- **Pipeline:** Standard data science flow—load data, clean, EDA, feature engineering, modeling, evaluation, conclusion.
- **Execution:** Run cells sequentially in JupyterLab or VS Code. No build, test, or automation scripts.
- **Dependencies:** Use Python 3.11+ and standard libraries: pandas, numpy, scikit-learn, matplotlib, seaborn. Install in the active environment as needed.
- **Visualization:** All plots are inline using matplotlib or seaborn.
- **Modeling:** Use scikit-learn for KNN and `GridSearchCV` for hyperparameter tuning. Example:
	- `GridSearchCV(KNeighborsClassifier(), param_grid, cv=5)`
- **Documentation:** Use markdown cells for explanations, results, and conclusions. Follow the style of existing sections.

## Project-Specific Patterns
- **Data loading:** Always use `pd.read_csv('data/diabetes.csv')` or similar, referencing the `data/` directory.
- **No external APIs:** All analysis is local; no web or API integration.
- **No cross-notebook code:** Each notebook is standalone; do not import from other notebooks or scripts.
- **Adding data:** Place new datasets in `data/` and document their source in the notebook.

## Integration & Extensibility
- **Dependencies:** If new packages are needed, install them in the notebook environment (see other case studies for `requirements.txt` patterns if needed).
- **Extending workflow:** Add new sections as markdown headers; keep code modular within the notebook.

## Examples
- Data loading: `pd.read_csv('data/diabetes.csv')`
- Model training: `GridSearchCV(KNeighborsClassifier(), param_grid, cv=5)`
- Visualization: `plt.plot(...)`, `sns.heatmap(...)`

## Additional Notes
- **Consistency:** Follow the structure and documentation style of the existing notebook.
- **Data presence:** Ensure all required data files are in `data/` before running the notebook.

---
If any conventions or workflows are unclear, review the main notebook for examples or ask for clarification.
