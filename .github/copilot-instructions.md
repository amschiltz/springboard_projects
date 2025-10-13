````instructions
# Springboard Data Science Project Collection - AI Assistant Guide

## Project Overview
This is a **Springboard Data Science Bootcamp** portfolio containing educational case studies, capstone projects, and mini-projects. The codebase follows structured learning methodologies with emphasis on statistical rigor, business applications, and progressive skill development from basic data manipulation to advanced machine learning.

## Repository Architecture

### Project Categories & Structure
- **Statistical Inference**: `Frequentist_Inference_Case_Study_Part_A/`, `Frequentist Inference Case Study Part B/` - Educational Q&A format with theoretical foundations
- **SQL Case Studies**: `Case_Study-Country_Club/SQLFiles_Tier_[1-2]/` - Multi-tier difficulty progression with local SQLite database
- **API Integration**: `API_Mini_Project/` - Real-world data fetching using Open-Meteo API with pandas wrangling
- **Capstone Projects**: `Capstone_2/` - Full ML pipeline from data wrangling to stakeholder presentation
- **Challenges**: `Unit_4_Challenge-Tier_3.ipynb` - Independent exploration exercises

### Data Science Workflow Pattern
```
notebooks/01_data_wrangling.ipynb → 02_eda.ipynb → 03_modeling.ipynb
├── data/raw/ (immutable source files)
├── data/interim/ (temporary outputs, flags)
├── data/processed/ (analysis-ready datasets)
├── reports/figures/ (plots for documentation)
└── deliverables/ (final polished submissions)
```

## Critical Development Conventions

### Educational Q&A Structure
- **Always preserve**: `__Q1:__`, `__Q2:__` question formatting and `__A:__` answer sections
- **Maintain empty code cells**: Educational completion format requires placeholder cells
- **Dual implementation pattern**: Manual statistical calculations first, then SciPy/library validation
- **Business context integration**: Connect statistical results to real-world decision making

### Statistical Analysis Standards
```python
# Required imports pattern for statistical projects
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm, t
from numpy.random import seed

# Reproducibility: ALWAYS use seed(47) before random operations
seed(47)

# Population vs Sample distinction (critical concept)
pop_std = np.std(data)          # n denominator
sample_std = np.std(data, ddof=1)  # n-1 denominator (Bessel's correction)
```

### Data Path Patterns & Common Issues
- **Capstone projects**: Use relative paths from notebook directory: `'../data/raw/filename.csv'`
- **Case studies**: Files often in notebook root: `'insurance2.csv'` not `'data/insurance2.csv'`
- **SQL projects**: Database files replicated across tiers: `sqlite_db_pythonsqlite.db`
- **API projects**: Save fetched data locally: `'los_angeles_weather.csv'`, `'new_york_weather.csv'`

### Visualization Conventions
```python
# Standard plot formatting for educational context
_ = plt.hist(data, bins=30)
_ = plt.xlabel('Variable Name (units)')  # Always label axes
_ = plt.ylabel('Frequency/Count')
_ = plt.title('Descriptive Business-Relevant Title')
plt.axvline(mean_value, color='red', linestyle='--', label='Mean')  # Reference lines
```

## Project-Specific Workflows

### SQL Case Studies
- **Local execution**: Use `LocalSQLConnection.py` wrapper for Python-SQLite integration
- **Online platform**: https://sql.springboard.com/ (Username: student, Password: learn_sql@springboard)  
- **Progressive difficulty**: Tier 1 (basic queries) → Tier 2 (complex joins, subqueries)
- **Database schema**: Country club with facilities, bookings, members tables

### Statistical Inference Projects
- **Mathematical rigor**: Include LaTeX formulas in markdown cells before implementation
- **Central Limit Theorem focus**: Demonstrate with varying sample sizes (n=10, n=50)
- **Confidence intervals**: Always specify one-sided vs two-sided based on business question
- **Hypothesis testing**: Manual p-value calculation using area under curve, then validate with `scipy.stats`

### Capstone Structure
- **Model performance targets**: Recall ≥ 0.70, ROC-AUC > 0.75 for churn prediction
- **Reproducible experiments**: Use `notebooks/experiments/` for scratch work
- **Stakeholder deliverables**: Presentation slides in `reports/slides/`, final polished versions in `deliverables/`
- **Environment management**: Virtual environment with `requirements.txt` (note: some files incorrectly named `.rtf`)

### API Integration Projects
- **Error handling**: Implement proper HTTP response validation and retry logic
- **Data persistence**: Save API responses locally to avoid repeated calls during development
- **Parameter patterns**: Use dictionaries for API parameters: `{'latitude': 40.7128, 'longitude': -74.0060}`

## Development Environment

### Python Environment Setup
```bash
# Standard virtual environment pattern
python3 -m venv .venv
source .venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
```

### Common Dependencies
- **Core stack**: pandas, numpy, matplotlib, seaborn
- **Statistical**: scipy.stats, statsmodels
- **ML**: scikit-learn, ydata-profiling (for automated EDA)
- **Visualization**: plotly (for interactive charts in API projects)
- **Database**: sqlite3 (built-in), sqlalchemy (for advanced queries)

## Troubleshooting Common Issues

### File Path Problems
- Update CSV import paths when notebooks reference incorrect relative paths
- Check for `.rtf` file extensions on what should be `.txt` files (e.g., requirements.txt.rtf)
- Verify database file locations across different project tiers

### Statistical Analysis
- **Always validate manual calculations** against library functions for educational value
- **Use appropriate statistical tests**: t-tests for unknown population parameters, not z-tests
- **Check sample size assumptions** for Central Limit Theorem applications
- **Interpret confidence intervals correctly**: parameter estimation, not probability statements

### Jupyter Notebook Best Practices
- **Preserve educational structure**: Don't consolidate Q&A sections or remove markdown explanations
- **Maintain reproducibility**: Ensure all cells run from top to bottom without dependencies on previous runs
- **Variable naming**: Use descriptive names reflecting business context (`charges_insured` vs `group1`)

When assisting with this codebase, prioritize educational clarity, statistical rigor, and maintaining the structured learning progression that characterizes Springboard's curriculum methodology.
````