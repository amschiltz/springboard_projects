# Springboard Data Science - Frequentist Inference Case Study

## Project Overview
This is an educational Springboard data science project focused on hands-on frequentist statistical inference using Python. The project follows a structured Q&A learning format with theoretical foundations from "The Art of Statistics" by Professor Spiegelhalter.

## Core Statistical Workflow

### Required Imports Pattern
```python
from scipy.stats import norm, t
import numpy as np
import pandas as pd  
from numpy.random import seed
import matplotlib.pyplot as plt
```

### Reproducible Analysis Convention
- **Always use `seed(47)`** before any random sampling operations
- This ensures consistent results across all exercises and enables comparison with expected outcomes
- Apply seed immediately before each sampling operation, not just once at the beginning

### Statistical Analysis Patterns

#### Population vs Sample Statistics
- Use `n` denominator for population standard deviation: `np.std(data)`
- Use `n-1` denominator (Bessel's correction) for sample standard deviation: `np.std(data, ddof=1)`
- The project emphasizes this distinction heavily - always consider whether you're analyzing a complete population or estimating from a sample

#### Distribution Functions
- Use `norm.rvs(loc, scale, size)` for generating normal random variates
- Use `norm.cdf(x, loc, scale)` for cumulative probabilities
- Use `norm.ppf(p, loc, scale)` for critical values (inverse CDF)
- For t-distribution: replace `norm` with `t` and include degrees of freedom parameter

#### Central Limit Theorem Applications
- Standard error of the mean: `σ/√n` where σ is population standard deviation
- Sample size matters: project demonstrates with n=10, n=50 comparisons
- Expected pattern: larger samples → narrower sampling distributions

## Notebook Structure Conventions

### Q&A Format
- Questions are numbered sequentially: `__Q1:__`, `__Q2:__`, etc.
- Always include `__A:__` section for answers
- Leave empty code cells after each answer section for implementation

### Visualization Standards
```python
_ = plt.hist(data, bins=30)
_ = plt.xlabel('height (cm)')  # Always label axes
_ = plt.ylabel('number of people')
_ = plt.title('Descriptive title')
# Use axvline for reference lines (mean, std dev markers)
```

### Documentation References
- Include specific page references to "The Art of Statistics" (e.g., "p. 236 of *AoS*")
- Link to external statistical resources for deeper explanations
- Provide hints that guide toward scipy.stats functions

## Data Analysis Approach

### Confidence Intervals
- Use both z-distribution (when σ known) and t-distribution (when σ estimated)
- Standard pattern: `critical_value * (std_error)`
- Always compare interval width between z and t approaches
- Check if true population parameter falls within calculated interval

### Hypothesis Testing
- Calculate p-values using area under the curve approach
- Use both manual calculation and `cdf()` function verification
- Focus on "how surprised should we be?" interpretation

### Sampling Simulations
- Create functions like `townsfolk_sampler(n)` for repeated sampling
- Simulate multiple trials (e.g., "year's worth of daily samples")
- Compare empirical results with theoretical CLT predictions

## Common File Patterns
- Main analysis notebooks: `[Topic] Case Study - Part [A/B].ipynb`
- Data files: typically CSV format (e.g., `insurance2.csv`)
- Expect both synthetic data (known distributions) and real-world datasets

## Educational Methodology
- Start with known population parameters, then progress to estimation scenarios
- Build intuition through visualization before formal statistical tests
- Emphasize conceptual understanding over computational complexity
- Progressive difficulty: basic sampling → sampling distributions → inference → confidence intervals

## Key Learning Objectives
When assisting with this codebase, focus on helping students understand:
- When to use population vs sample statistics (n vs n-1 denominators)
- Practical application of Central Limit Theorem
- Difference between z-statistics and t-statistics
- Confidence interval interpretation and calculation
- Connection between theoretical concepts and Python implementation