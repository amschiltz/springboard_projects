# Copilot Instructions: Boston Housing Linear Regression Case Study

## Project Overview
This is a **Springboard Data Science** educational case study implementing linear regression analysis on the classic Boston Housing dataset. The project follows academic data science methodology with extensive statistical theory explanation and hands-on implementation.

## Key Architecture & Data Flow

### Data Pipeline
- **Primary Dataset**: `boston.csv` - 506 housing records with 14 features (CRIM, ZN, INDUS, etc.) 
- **Target Variable**: MEDV (median home value) → renamed to PRICE in analysis
- **Workflow**: EDA → Feature Analysis → Model Fitting → Validation → Assumption Testing

### Dual Implementation Strategy
The project implements **two parallel modeling approaches**:
1. **`statsmodels.api`** - Statistical analysis focus with R-like syntax, detailed regression summaries
2. **`sklearn.linear_model`** - Machine learning focus with prediction pipeline

**Formula syntax pattern**: `'PRICE ~ RM + CRIM + PTRATIO'` (statsmodels)
**Array syntax pattern**: `X = bos.drop('PRICE', axis=1); lm.fit(X, y)` (sklearn)

## Development Workflow Patterns

### Notebook Cell Structure
- **Theory cells**: Mathematical derivations with LaTeX formatting (`$$ Y = \beta_0 + \beta_1 X + \epsilon $$`)
- **Implementation cells**: Code following theory explanation  
- **Exercise cells**: Placeholder cells for student implementation (`# your turn`)
- **Visualization cells**: `seaborn`/`matplotlib` plots with consistent styling

### Data Science Conventions
- Use `sns.set_style("whitegrid")` and `sns.set_context("poster")` for plot consistency
- Transform skewed variables with `np.log()` before analysis (see CRIM variable)
- Generate both individual scatter plots and `sns.regplot()` with fitted lines
- Include comprehensive model diagnostics: residual plots, QQ plots, leverage analysis

### Statistical Analysis Workflow
1. **Exploratory Phase**: `describe()` → scatter plots → histograms → correlation analysis
2. **Modeling Phase**: Simple regression → Multiple regression → Model comparison
3. **Validation Phase**: Residual analysis → Assumption testing → Outlier detection

## Critical Implementation Details

### Model Evaluation Pattern
```python
# Always compute multiple metrics together
print(f'R-squared: {lm.score(X, y)}')
print(f'F-statistic: {F_stat}')  
print(f'AIC: {model.aic}')  # statsmodels only
```

### Assumption Testing Requirements  
- **Linearity**: Scatter plots of each X vs Y
- **Homoscedasticity**: Fitted vs residual plots  
- **Normality**: QQ plots of residuals
- **Independence**: Leverage plots for influential points

### Educational Exercise Integration
Each major section includes **"Checkup Exercise Set"** with specific deliverables:
- Interpretation exercises for coefficients and statistical tests
- Plotting exercises with storytelling requirements
- Comparison exercises between modeling approaches

## External Dependencies & Integration

### Core Data Science Stack
- `pandas` - DataFrame operations with `.rename()`, `.describe()` patterns
- `numpy` - Mathematical transformations, particularly `np.log()` for skewed data  
- `matplotlib.pyplot` + `seaborn` - Visualization with academic styling
- `scipy.stats` - Statistical distributions and tests
- `sklearn` - ML pipeline implementation

### Academic Context Integration  
- References **Harvard CS109 course materials** and methodology
- Uses **Springboard workshop** pedagogical structure  
- Includes links to external statistical resources and theory explanations

## File Organization & Assets

### Supporting Materials
- `images/` directory contains theoretical diagrams:
  - `conditionalmean.png` - Visual explanation of conditional expectation
  - `cs109gitflow3.png` - Academic workflow reference
  - `shuttle.png` - Context illustration

### Data Science Notebook Patterns
- Extensive markdown cells with mathematical notation
- Consistent code cell commenting and output interpretation
- Student exercise placeholders following teaching methodology

When working with this codebase, prioritize educational clarity, statistical rigor, and dual implementation approaches that demonstrate both statistical and machine learning perspectives on linear regression.