# Copilot Instructions for Bayesian Optimization Case Study

## Project Overview
This workspace contains a Jupyter notebook for a Bayesian optimization case study focused on flight delay prediction. The main workflow is centered around interactive data analysis and model development in the notebook, using CSV data files.

## Key Files and Structure
- `Bayesian_optimization_case_study.ipynb`: Main notebook for all code, analysis, and documentation. All logic, experiments, and results are contained here.
- `data/flight_delays_train.csv` and `data/flight_delays_test.csv`: Training and test datasets for flight delay prediction. These are loaded and processed within the notebook.

## Developer Workflow
- **Interactive Development**: All code is written and executed in the notebook. There are no standalone scripts or modules.
- **Data Loading**: Use pandas to read CSV files from the `data/` directory. Example:
  ```python
  import pandas as pd
  train_df = pd.read_csv('data/flight_delays_train.csv')
  test_df = pd.read_csv('data/flight_delays_test.csv')
  ```
- **Modeling**: Bayesian optimization techniques are applied, typically using libraries such as scikit-learn, scikit-optimize, or similar. Install missing packages directly in notebook cells using `!pip install` if needed.
- **Experiment Tracking**: Results, plots, and notes are kept in notebook cells. There is no external logging or experiment management.

## Patterns and Conventions
- **Single Notebook Pattern**: All code, markdown, and outputs are in one notebook. Avoid creating extra scripts unless explicitly requested.
- **Relative Paths**: Always use relative paths for data files (e.g., `data/flight_delays_train.csv`).
- **Inline Documentation**: Use markdown cells for explanations, methodology, and results.
- **Reproducibility**: Ensure all code cells can be run top-to-bottom without errors, assuming required packages are installed.

## Integration Points
- **External Libraries**: Commonly used libraries include pandas, numpy, matplotlib, scikit-learn, and Bayesian optimization packages. Install as needed.
- **No External Services**: There are no API calls, web services, or cloud integrations in this project.

## Example Workflow
1. Load data from CSV files in `data/`.
2. Perform exploratory data analysis (EDA) in notebook cells.
3. Build and tune models using Bayesian optimization.
4. Document findings and results in markdown cells.

## Guidance for AI Agents
- Focus all code and documentation in the notebook.
- Reference data files using relative paths.
- Use markdown cells for explanations and results.
- If a new workflow or script is needed, ask for user confirmation before creating files outside the notebook.

---
If any section is unclear or missing, please provide feedback for further refinement.
