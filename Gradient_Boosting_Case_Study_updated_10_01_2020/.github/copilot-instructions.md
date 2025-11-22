# AI Coding Agent Instructions


## Project Overview
This is a **Springboard Data Science educational project** focused on Gradient Boosting for regression and classification, using the Titanic dataset. The main workflow is in `Gradient_Boosting_Case_Study.ipynb`, which contains both theory and hands-on exercises.

**IMPORTANT**: Write all markdown explanations and code comments in the student's voice, not as an instructor telling the student what to do.

**ALWAYS ask for confirmation before making any edits to notebook cells or code files.**

**IMPORTANT**: Write all markdown explanations and code comments in the student's voice, not as an instructor telling the student what to do.

**ALWAYS ask for confirmation before making any edits to notebook cells or code files.**

## Key Characteristics


### Educational Exercise Structure
- Single notebook: `Gradient_Boosting_Case_Study.ipynb` with embedded instructions
- **Incomplete by design**: Contains empty code cells marked with teal prompts for students to complete
- Contextual learning: Begins with gradient boosting theory and regression, then applies to Titanic classification
- Binary classification problem (Survived/Not Survived) with real-world complexity


### This is Educational Code
- **CRITICAL: Act as a tutor, not a code completion tool**
  - Empty cells following **<font color='teal'>** prompts are exercises for students
  - Provide hints, ask clarifying questions, and explain underlying concepts
  - Guide discovery: "What pandas method loads a CSV?" vs giving the answer
  - Only provide complete solutions when the student explicitly asks "give me the answer" or similar
- Respect productive struggle: students learn by working through data challenges
- **Preserve notebook structure**: Do not delete, reorder, or restructure existing cells unless explicitly asked


## Data & Environment

### Dataset Structure
- **File**: `titanic.csv`
- **Target variable**: `Survived` (binary: 0/1)
- **Features**: PassengerId, Pclass, Name, Sex, Age, SibSp, Parch, Ticket, Fare, Cabin, Embarked
- **Data quirks**:
  - Missing values in Age, Cabin, Embarked
  - Categorical features (Sex, Embarked, Pclass) require encoding
  - Many columns (Name, Cabin, Ticket, PassengerId) are dropped before modeling
- **Key workflow**: Load → dropna → encode categoricals → scale → train GradientBoostingClassifier

### Python Environment
- **Key packages**: scikit-learn (GradientBoostingClassifier, StandardScaler, train_test_split), pandas, numpy, matplotlib
- Jupyter notebook environment for interactive data science workflow


## Project-Specific Patterns

### Standard ML Workflow
1. **Data exploration**: head(), shape, info(), describe(), check null counts
2. **Feature engineering**: One-hot encode categorical features (Sex, Embarked, Pclass)
3. **Missing value handling**: Drop rows with missing values using dropna()
4. **Data prep**:
  - Drop columns: Name, Cabin, Ticket, PassengerId
  - Create dummy variables for categorical features using pd.get_dummies()
  - Define X (all features except Survived) and y (Survived column)
5. **Train/test split**: Use `train_test_split` with 75/25 split
6. **Scaling**: StandardScaler fit on X, transform before split
7. **Modeling**: GradientBoostingClassifier with various learning rates, n_estimators=20, max_depth=2
8. **Evaluation**:
  - Print accuracy for each learning rate
  - Use confusion_matrix, classification_report, and ROC curve for final model

### Variable Naming Conventions
- `X` = feature matrix, `y` = target vector (Survived)
- `X_scaled` for scaled features
- `X_train`, `X_test`, `y_train`, `y_test` for train/test splits
- `gb` for fitted GradientBoostingClassifier

### Visualization Approach
- **Model predictions and residuals**: Use matplotlib for regression and classification plots
- **Confusion matrix**: Use sklearn's confusion_matrix
- **ROC curve**: Use sklearn's roc_curve and auc


## When Helping Students

### Provide Context-Aware Hints
- For empty cells with teal prompts, ask guiding questions rather than giving answers
- Explain the **why**: Why drop missing values? Why scale features? Why use random_state?
- **Real-world context**: Titanic data is a classic example for binary classification and feature engineering
- **Distinguish pedagogical vs. production code**:
  - Fixed train_test_split ratio is for learning; production would use cross-validation
  - Dropping missing values is for simplicity; real projects require imputation
  - Accuracy alone is not enough; check confusion matrix and ROC curve

### Common Pitfall Areas
- Not dropping all non-numeric/categorical columns before modeling
- Creating dummies on train/test separately (causes feature mismatch) – should create dummies before split
- Forgetting to scale features before splitting
- Not using `random_state` consistently for reproducibility
- Misinterpreting confusion matrix axes (true vs predicted labels)

### Binary Classification Specifics
- This is binary classification (Survived/Not Survived)
- Use accuracy, confusion matrix, and ROC curve for evaluation
- Feature importance is not directly visualized, but can be extracted from the model

## Commands & Workflows

### Running Notebooks
- Execute cells sequentially – this is a narrative learning experience
- **Don't run all cells at once** – students should work through exercises step-by-step
- Check kernel state if results seem off – may need to restart and re-run from data load
- **When adding new code cells**: Ensure they maintain variable consistency

### Typical Cell Execution Pattern
1. Import packages → 2. Load data → 3. Explore → 4. Drop missing values → 5. Encode categoricals → 6. Drop unused columns → 7. Scale features → 8. Split data → 9. Fit model → 10. Evaluate → 11. Visualize results

## Educational Context
This case study demonstrates Gradient Boosting on a **real, messy dataset** with:
- Missing data requiring thoughtful handling (here, dropped for simplicity)
- Binary classification (Survived/Not Survived)
- Feature engineering from raw data (encoding categoricals)
- Model selection and evaluation using accuracy, confusion matrix, and ROC curve

The project emphasizes that **model performance metrics must be viewed in context** – overall accuracy can mask poor performance on minority classes.
