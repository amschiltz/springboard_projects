# Copilot Instructions for Springboard Linear Regression Case Study

## Project Overview
This is a Springboard data science case study focused on linear regression using the red wine dataset. The project follows a structured educational format with three tiers of increasing complexity (Tier 1: complete code, Tier 2: semi-guided, Tier 3: fill-in-the-blanks).

## Project Structure & Key Files
- **Data source**: `wineQualityReds.csv` - Red wine quality dataset with 12 features (acidity, density, alcohol, etc.)
- **Notebooks**: Three tiered case study notebooks with identical structure but different completion levels
- **Target variable**: `fixed.acidity` (not `quality` - it's discrete/categorical, unsuitable for regression)

## Educational Framework & Methodology
This project follows "The Art of Statistics" (AoS) by Professor Spiegelhalter methodology:

### Standard Data Science Pipeline Structure:
1. **Sourcing and loading** - Library imports, data loading, exploration, variable selection
2. **Cleaning, transforming, and visualizing** - Correlation analysis with heatmaps/pairplots  
3. **Modeling** - Four progressive regression models with train/test splits
4. **Evaluating and concluding** - Model comparison and reflection

### Four-Model Progression Pattern:
1. **Model 1**: Simple linear regression (sklearn) - single predictor (`density`)
2. **Model 2**: OLS regression (statsmodels) - same data, different library for comparison  
3. **Model 3**: Multiple linear regression - all features except target and `quality`
4. **Model 4**: Reduced feature model - removing correlated features for elegance

## Code Patterns & Conventions

### Import Structure (Standard across all tiers):
```python
import numpy as np
import pandas as pd  
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from statsmodels.graphics.api import abline_plot
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.model_selection import train_test_split
from sklearn import linear_model, preprocessing
```

### Data Loading Pattern:
```python
wine = pd.read_csv("wineQualityReds.csv", index_col=0)  # Always exclude first column
```

### Visualization Workflow:
- **Correlation analysis**: `wine.corr()` → `sns.heatmap(wine.corr(), annot=True)`
- **Pair relationships**: `sns.pairplot(wine)` 
- **Scatter plots**: `sns.scatterplot()` → `sns.regplot()` for trend lines

### Model Building Pattern:
```python
# Train/test split (always 75/25, random_state=123)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=123)

# Statsmodels requires adding constants
X = sm.add_constant(X)

# Model evaluation uses R-squared and RMSE metrics
```

## Critical Domain Knowledge

### Variable Selection Logic:
- **Avoid `quality`**: Integer/discrete variable → use classification, not regression
- **Use `fixed.acidity`**: Continuous variable, wine domain importance (affects taste, stability)
- **Feature relationships**: `volatile.acidity` and `citric.acid` correlate with `pH` (redundancy consideration)

### Model Performance Expectations:
- Single variable model: ~45% R-squared
- Multiple regression: ~87% R-squared improvement
- RMSE interpretation: Average prediction error in original units

## Fill-in-the-Blank Patterns (Tier 3 specific)
When working with Tier 3 notebooks, common fill-in patterns:
- `_ _ _` typically represents: `wine`, `numpy`, `pandas`, method names
- Method completions: `.head()`, `.info()`, `.shape`, `.describe()`, `.corr()`
- Variable assignments: `X = wine[["density"]]`, `y = wine[["fixed.acidity"]]`

## Notebook Workflow Best Practices
1. **Always check data first**: `.head()`, `.info()`, `.shape` before analysis
2. **Validate variable types**: Continuous vs discrete suitability for regression
3. **Visualization before modeling**: Understand relationships via plots
4. **Progressive complexity**: Start simple (univariate) → build to multivariate
5. **Model comparison**: Compare R-squared, RMSE, and prediction plots across models

## Common Issues & Solutions
- **Missing imports**: Check standard import block is complete
- **Data path**: Ensure `wineQualityReds.csv` is in working directory
- **Statsmodels syntax**: Remember `sm.add_constant(X)` requirement
- **Plot sizing**: Use `plt.figure(figsize=(40,20))` for readable heatmaps
- **Random state consistency**: Always use `random_state=123` for reproducibility