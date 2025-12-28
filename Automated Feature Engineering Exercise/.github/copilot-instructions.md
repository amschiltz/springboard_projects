# Copilot Instructions for Automated Feature Engineering Exercise

## Project Overview
- This project demonstrates automated feature engineering for customer churn prediction using [Featuretools](https://github.com/Featuretools/featuretools).
- The workflow is based on adapting a public notebook, removing AWS/S3 dependencies, and focusing on local/Colab execution.
- Data is relational: members, transactions, logs, and label times (cutoff_times).

## Key Files & Data
- Main workflow: `Feature Engineering.ipynb`
- Data: `data/` directory (CSV files: `members.csv`, `transactions.csv`, `logs.csv`, `MS-31_labels.csv`)
- Feature definitions: `data/churn/features.txt` (binary, saved/loaded via Featuretools)
- Requirements: `requirements.txt` (notably: featuretools, pandas, dask, matplotlib, seaborn)

## Core Patterns & Conventions
- **EntitySet Construction**: Use Featuretools' `EntitySet` to represent tables and relationships. Each table (members, transactions, logs) is added with correct index and time_index. Relationships are always parent (`members.msno`) to child (`transactions.msno`, `logs.msno`).
- **Feature Engineering**: 
  - Use `ft.dfs` for Deep Feature Synthesis, passing the entityset, cutoff_times, and lists of primitives.
  - Custom primitives are defined as Python classes inheriting from `AggregationPrimitive` or `TransformPrimitive`.
  - Hand-crafted features (e.g., `price_difference`, `percent_100`) are created before adding tables to the entityset.
- **Cutoff Times**: Always filter features using the `cutoff_times` table to avoid data leakage.
- **Interesting Values**: Set `interesting_values` for categorical/boolean columns to enable conditional feature generation.
- **Saving/Loading Features**: Use `ft.save_features` and `ft.load_features` for portability and reproducibility.

## Developer Workflow
- Install dependencies from `requirements.txt` (pip install -r requirements.txt).
- Run all code in `Feature Engineering.ipynb` sequentially for end-to-end feature engineering.
- Adjust `chunk_size` and `n_jobs` in `ft.dfs` for performance tuning.
- Data is expected in `data/` as CSVs; notebook can be adapted to use local or remote data sources.
- Visualizations and EDA are performed inline using matplotlib/seaborn.

## Project-Specific Notes
- No build/test automation: all work is interactive in the notebook.
- No AWS/S3 integration; all data is local.
- Custom primitives are encouraged and exemplified in the notebook.
- The project is educational and intended for experimentation and learning with Featuretools.

## References
- See `Feature Engineering.ipynb` for all code patterns and workflow details.
- For more, see the [original notebook](https://github.com/Featuretools/predict-customer-churn/blob/master/churn/3.%20Feature%20Engineering.ipynb) and [Featuretools documentation](https://docs.featuretools.com/).
