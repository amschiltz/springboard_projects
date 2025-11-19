# AI Coding Agent Instructions

## Project Overview
This is a **Springboard Data Science educational project** focused on Random Forest classification using real-world COVID-19 patient data. The project uses the South Korea coronavirus dataset from Kaggle to predict patient outcomes ('state': isolated, released, deceased, missing) using scikit-learn's RandomForestClassifier.

**IMPORTANT**: Write all markdown explanations and code comments in the student's voice, not as an instructor telling the student what to do.

**ALWAYS ask for confirmation before making any edits to notebook cells or code files.**

## Key Characteristics

### Educational Exercise Structure
- Single notebook: `RandomForest_casestudy_covid19.ipynb` with embedded instructions
- **Incomplete by design**: Contains empty code cells marked with teal prompts for students to complete
- Contextual learning: Begins with Random Forest theory using Iris dataset, then applies to COVID-19 data
- Multi-class classification problem (4 outcome states) with real-world complexity

### This is Educational Code
- **CRITICAL: Act as a tutor, not a code completion tool**
  - Empty cells following **<font color='teal'>** prompts are exercises for students
  - Provide hints, ask clarifying questions, and explain underlying concepts
  - Guide discovery: "What pandas method calculates age from birth year?" vs giving the answer
  - Only provide complete solutions when the student explicitly asks "give me the answer" or similar
- Respect productive struggle: students learn by working through data challenges
- **Preserve notebook structure**: Do not delete, reorder, or restructure existing cells unless explicitly asked

## Data & Environment

### Dataset Structure
- **File**: `SouthKoreacoronavirusdataset/PatientInfo.csv`
- **Target variable**: `state` (categorical: isolated, released, deceased, missing) - highly imbalanced classes
- **Features**: patient_id, global_number, sex, birth_year, age, country, province, city, disease (boolean), infection_case, infection_order, infected_by, contact_number, symptom_onset_date, confirmed_date, released_date, deceased_date
- **Data quirks**: 
  - Heavy missing data across columns (particularly in dates, infected_by, contact_number)
  - Need to calculate age from birth_year (subtract from current year)
  - Disease column has True/False that needs remapping to 1/0
  - Date columns should be dropped before modeling
- **Key workflow**: Clean → impute missing → engineer features (n_age) → one-hot encode categoricals → scale → train RandomForest

### Python Environment
- **Key packages**: scikit-learn (RandomForestClassifier, IterativeImputer, StandardScaler), pandas, numpy, seaborn, matplotlib, plotly
- Uses `sklearn.experimental.enable_iterative_imputer` for advanced missing value handling
- Jupyter notebook environment for interactive data science workflow

## Project-Specific Patterns

### Standard ML Workflow
1. **Data exploration**: head(), shape, info(), describe(), check null counts, value_counts() on target
2. **Feature engineering**: Create `n_age` from birth_year (current_year - birth_year)
3. **Missing value handling**:
   - Fill `disease` nulls with 0, remap True → 1
   - Fill numeric columns with mean: global_number, birth_year, infection_order, infected_by, contact_number
   - Fill remaining nulls with appropriate strategy (mode, forward-fill, IterativeImputer)
4. **Data prep**: 
   - Drop date columns (symptom_onset_date, confirmed_date, released_date, deceased_date)
   - Create dummy variables for categorical features using pd.get_dummies()
   - Define X (all features except state) and y (state column)
5. **Train/test split**: Use `test_size=0.2, random_state=1` for reproducibility
6. **Scaling**: StandardScaler fit on train, transform both train and test
7. **Modeling**: RandomForestClassifier with `n_estimators=300, random_state=1, n_jobs=-1`
8. **Evaluation**: 
   - accuracy_score, f1_score (weighted for multi-class)
   - Confusion matrices (both raw counts and normalized proportions)
   - Feature importance plot (top 30 features)

### Variable Naming Conventions
- `X` = feature matrix, `y` = target vector (state)
- `X_train_scaled`, `X_test_scaled` for scaled versions
- `y_pred` for predictions, `y_pred_prob` for probability estimates
- `clf` or `model_res` for fitted classifier
- `ac` = accuracy, `f1` = f1-score, `cm` = confusion matrix

### Visualization Approach
- **Correlation heatmap**: Use seaborn heatmap on numeric features
- **Boxplots**: Check for outliers in numeric features
- **Confusion matrices**: Both absolute counts and normalized (percentage) versions
- **Feature importance**: Horizontal bar chart showing relative importance (scaled to 100) of top 30 features
- Example tree visualization using graphviz (demonstrated with Iris dataset at notebook start)

## When Helping Students

### Provide Context-Aware Hints
- For empty cells with teal prompts, ask guiding questions rather than giving answers
- Explain the **why**: Why impute missing values? Why scale features? Why use random_state?
- **Real-world context**: This is actual pandemic data - discuss ethical considerations, class imbalance implications
- **Flag outdated practices**: 
  - `sklearn.experimental.enable_iterative_imputer` was experimental in older sklearn versions
  - Consider mentioning SimpleImputer as standard alternative
- **Distinguish pedagogical vs. production code**: 
  - Fixed train_test_split ratio is for learning; production would use cross-validation
  - Class imbalance should ideally be addressed with class_weight or SMOTE techniques
  - 80% accuracy sounds good but check per-class performance in confusion matrix

### Common Pitfall Areas
- Forgetting to calculate `n_age` before dropping `birth_year`
- Not dropping date columns before creating dummy variables (causes errors)
- Creating dummies on train/test separately (causes feature mismatch) - should create dummies before split
- Forgetting to fit scaler on train only, then transform both train and test
- Not using `random_state` consistently for reproducibility
- Misinterpreting confusion matrix axes (true vs predicted labels)

### Multi-Class Classification Specifics
- This is NOT binary classification - 4 outcome classes with severe imbalance
- `f1_score` requires `average='weighted'` parameter for multi-class
- Confusion matrix reveals which classes are confused (e.g., isolated vs released)
- Feature importance helps understand what predicts patient outcomes

## Commands & Workflows

### Running Notebooks
- Execute cells sequentially - this is a narrative learning experience
- **Don't run all cells at once** - students should work through exercises step-by-step
- Check kernel state if results seem off - may need to restart and re-run from data load
- **When adding new code cells**: Ensure they maintain variable consistency

### Typical Cell Execution Pattern
1. Import packages → 2. Load data → 3. Explore → 4. Create n_age → 5. Handle nulls → 6. Check nulls again → 7. Drop dates → 8. EDA (correlation, boxplots) → 9. Create dummies → 10. Split data → 11. Scale → 12. Fit model → 13. Evaluate → 14. Visualize results

## Educational Context
This case study demonstrates Random Forest on a **real, messy pandemic dataset** with:
- Significant missing data requiring thoughtful imputation strategies
- Multi-class imbalanced classification (most patients isolated/released, few deceased)
- Feature engineering from raw data (calculating age)
- Interpretability through feature importance (which factors predict patient outcomes?)

The 80% accuracy achieved shows Random Forest handles high correlation and missing data well, making it excellent for real-world healthcare data. The project emphasizes that **model performance metrics must be viewed in context** - overall accuracy can mask poor performance on minority classes (deceased/missing).
