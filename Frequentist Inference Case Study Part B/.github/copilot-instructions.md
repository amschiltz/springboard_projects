# AI Coding Assistant Instructions

## Project Overview
This is a **Springboard Data Science** educational case study focused on **frequentist statistical inference** using medical insurance claims data. The project demonstrates applying statistical concepts (z-statistics, t-statistics, Central Limit Theorem, confidence intervals) to answer real-world business questions about hospital revenue and insurance coverage effects.

## Key Architecture & Workflow

### Project Structure
- **Single Jupyter notebook**: `Frequentist Inference Case Study - Part B.ipynb` - complete statistical analysis workflow
- **Dataset**: `insurance2.csv` - medical insurance claims with columns: `age`, `sex`, `bmi`, `children`, `smoker`, `region`, `charges`, `insuranceclaim`
- **Educational format**: Question-Answer structure with markdown explanations and empty code cells for student completion

### Data Science Workflow Pattern
1. **Data loading**: Always use `pd.read_csv('insurance2.csv')` (note: original code references `'data/insurance2.csv'` but file is in root)
2. **Statistical analysis sequence**: EDA → Hypothesis formation → Manual calculations → SciPy validation
3. **Mathematical rigor**: Manual implementation of statistical formulas before using library functions
4. **Business context**: Hospital administrator decision-making scenarios drive the analysis

## Development Conventions

### Statistical Analysis Pattern
```python
# Standard imports for this project
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import t
from numpy.random import seed
```

### Educational Code Structure
- **Dual implementation approach**: Calculate statistics manually first, then validate with SciPy
- **Mathematical formulas**: Include LaTeX equations in markdown cells for statistical concepts
- **Empty code cells**: Preserve Q&A format with empty cells for student work
- **Business framing**: Connect statistical results to hospital operational decisions

### Key Statistical Concepts Implemented
- **One-sided vs two-sided confidence intervals** for business decision making
- **Pooled standard deviation calculations** for two-sample t-tests
- **Manual t-statistic calculation** with formula: `t = (x̄₀ - x̄₁) / (sₚ√(1/n₀ + 1/n₁))`
- **Central Limit Theorem applications** to non-normal data distributions

## Critical Context for AI Assistants

### Data Path Issues
- Code references `'data/insurance2.csv'` but file is located at `'insurance2.csv'` (root level)
- Update import paths when running cells: `medical = pd.read_csv('insurance2.csv')`

### Statistical Methodology
- **Always validate manually calculated statistics** against SciPy functions
- **Use appropriate statistical tests**: t-tests for unknown population parameters, not z-tests
- **Consider business context**: Hospital revenue thresholds, insurance coverage comparisons
- **Maintain educational value**: Show work step-by-step, don't just use library functions

### Jupyter Notebook Specific
- **Preserve markdown structure**: Don't remove Q&A formatting
- **Maintain empty cells**: Keep placeholder code cells for educational completion
- **Variable naming**: Use descriptive names like `charges_insured`, `charges_uninsured` for clarity
- **Mathematical notation**: Use LaTeX in markdown for statistical formulas

When helping with this project, focus on educational clarity, statistical rigor, and connecting analysis to business decisions rather than just producing working code.