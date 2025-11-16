# AI Coding Agent Instructions

## How We Work Together

### Collaborative Approach
- **Tutoring role**: I act as a tutor, guiding your learning rather than just providing answers. I'll ask clarifying questions and explain concepts
- **Educational context first**: This is a learning project. Explanations should be clear and pedagogical, matching the notebook's teaching style
- **Student's voice**: Write all markdown explanations and code comments in the student's voice (first person: "I", "we"), not as an instructor
- **Preserve structure**: Maintain the established (a)-(g) labeled pattern and section organization
- **Sequential execution**: When adding code cells, ensure they can be executed in order without breaking prior cells
- **Explain the "why"**: Include markdown explanations for new code, following the notebook's heavily-commented style

### When You Ask for Help
- **Specify exercise number** if working on Checkup Exercises (I-IV)
- **Indicate section context** (e.g., "in the EDA section", "after the first model")
- **State your goal clearly** (e.g., "add a new visualization", "try a different regularization value")

### What I'll Provide
- **Code that fits the pattern**: Following the established (a)-(g) cycle and naming conventions
- **Educational explanations**: Matching the notebook's teaching style with inline comments
- **Complete solutions**: Ready to run without placeholders or TODOs
- **Context-aware suggestions**: Respecting the mild class imbalance and stratification requirements
- **Best practice code**: Even when pedagogical examples use outdated approaches, new code will follow current best practices
- **Flagged outdated content**: I'll note when existing notebook patterns are outdated (e.g., deprecated functions, suboptimal approaches)

### My Constraints
- **Always ask before editing**: I will not make any changes to the notebook without your explicit approval first
- I won't modify existing pedagogical content unless you explicitly ask
- I'll preserve the exercise structure (won't fill in answers unless requested)
- I'll maintain backward compatibility with cells already executed
- I'll flag when existing notebook content uses outdated practices while ensuring new code follows best practices

## Project Overview
This is a Springboard Data Science educational case study focused on binary classification using Logistic Regression to predict heart disease presence from patient health measurements. The project is a single Jupyter notebook workflow adapted from CS109 course materials.

## Architecture & Structure
- **Single notebook design**: `Logistic_Regression_Advanced_Case_Study.ipynb` contains all analysis, modeling, and documentation
- **Data**: `data/heart.xlsx` - processed Cleveland heart disease dataset (270 patients, 23 features after encoding)
- **Images**: `images/` contains static diagrams for educational content (confusion matrix examples, data form illustrations)
- **No modular code**: All functions are defined inline within the notebook

## Data Science Workflow Pattern
The notebook strictly follows a pedagogical Data Science Process with explicitly labeled sections:
1. Problem Identification (predict heart disease)
2. Data Wrangling (minimal - some missing values removed upstream)
3. EDA (intentionally abbreviated - "Minimal EDA" section)
4. Pre-processing (dummy encoding for categorical features)
5. Modeling (primary focus)
6. Documentation (inline throughout)

**When adding analysis**: Follow the section structure and explicitly label which step you're addressing.

## Key Technical Patterns

### Categorical Feature Encoding
The notebook uses `pd.get_dummies()` to one-hot encode these features:
```python
categorical_features = ['sex_M_F', 'chest_pain_value', 'ECG_value', 
                        'ST_slope_peak', 'defect_diag']
dflog = pd.get_dummies(dflog, columns=categorical_features)
```
After encoding, the dataset expands from ~9 raw columns to 23 feature columns plus the target `heart_disease` (binary: 0=no disease, 1=disease).

### Train-Test Split Convention
**Critical**: The notebook emphasizes stratified splitting to preserve class balance:
```python
Xlr, Xtestlr, ylr, ytestlr = train_test_split(X, y, random_state=2, stratify=y)
```
The dataset has mild class imbalance (~56% class 0, ~44% class 1). Always use `stratify=y` when creating new splits.

### Naming Convention for Splits
- Training data: `Xlr`, `ylr` (suffix "lr" indicates logistic regression context)
- Test data: `Xtestlr`, `ytestlr`
- For stratified splits: `Xlrstrat`, `ylrstrat`, `Xtestlrstrat`, `ytestlrstrat`

### Model Building Cycle (Standard Pattern)
The notebook defines a 7-step cycle explicitly labeled (a) through (g):
```
(a) Define X and y
(b) Perform train/test split on (X, y)
(c) Create LogisticRegression object
(d) Build model using .fit()
(e) Predict using .predict()
(f) Compute performance metrics
(g) Determine if model can be improved
```
**When adding models**: Follow this labeled pattern for consistency with educational structure.

### Visualization Functions
Two custom plotting utilities are defined early in the notebook:
- `points_plot()`: Plots 2D decision boundaries with train/test points (circles/squares)
- `points_plot_prob()`: Adds probability contours to decision boundary plots
- `plot_y_ratios()`: Bar charts comparing class distributions between train/test splits

These functions only work for 2-feature models and use global color scheme variables (`cmap_light`, `cmap_bold`, etc.) defined in setup cells.

### Cross-Validation Pattern
Custom `cv_score()` function implements 5-fold CV:
```python
def cv_score(clf, x, y, score_func=accuracy_score):
    # Returns average score across 5 folds
    # Uses KFold with shuffle=True, random_state=42
```
**Note**: The function requires resetting indices on x and aligning y indices before splitting.

### Performance Evaluation Philosophy
The notebook strongly emphasizes:
- **Never use accuracy alone** - always generate confusion matrices
- Show both training and test metrics to detect overfitting
- Use `classification_report()` for precision, recall, F1-score
- The minority class (class 1 - heart disease) is the class of interest

### Regularization Context
scikit-learn `LogisticRegression` always uses L2 regularization by default (C=1.0):
- Smaller C = stronger regularization
- The notebook explores C values: [0.001, 0.1, 1, 10, 100]
- Uses `solver='newton-cg'` or `solver='liblinear'` depending on context

## Development Workflows

### Running the Notebook
Execute cells sequentially from top to bottom. Key dependencies:
- Library imports cell must run first (sets up matplotlib, sklearn, pandas, etc.)
- Configuration cells set pandas display options and matplotlib styling
- Data loading requires `./data/heart.xlsx` relative path

### Adding New Models
1. Follow the (a)-(g) labeled cycle
2. Use stratified train-test split
3. Generate both confusion matrix and classification report
4. Compare train vs test accuracy to assess overfitting
5. If cross-validating, use the custom `cv_score()` function or `GridSearchCV`

### Checkup Exercises
The notebook includes "Checkup Exercise Set" markdown cells (I-IV) with empty code cells below for student work:
- Exercise Set I: Scatter plots with color-coded classes
- Exercise Set II: Manual grid search over C values
- Exercise Set III: Evaluate best C on test set
- Exercise Set IV: Use `GridSearchCV` instead of manual search

**When assisting with exercises**: Provide code that fits in the empty cell directly below the exercise description.

## Common Pitfalls to Avoid
1. **Don't create new train-test splits without stratification** - class imbalance will worsen
2. **Don't use only 2 features for final models** - the 2-feature examples are pedagogical
3. **Don't import new libraries without adding to the top imports cell**
4. **Don't modify the categorical_features list** without updating the get_dummies call
5. **Don't expect visualization functions to work with >2 features** - they're intentionally limited

## Project-Specific Context
- **Mild imbalance**: ~10% difference between classes (not severe but worth noting)
- **Educational focus**: Heavily commented with side-bars explaining concepts (e.g., "Side-Bar: Cross Validation")
- **Optional appendix**: Mathematical derivation of logistic regression at end (optional reading)
- **No production intent**: This is learning material, not a deployment-ready pipeline
- **CS109 heritage**: Adapted from Harvard CS109 course lab materials

## References in Code
- UCI Machine Learning Repository heart disease dataset (Cleveland)
- ISLR book (Introduction to Statistical Learning with R) for metrics explanations
- CS109 2015 Lab 5: https://github.com/cs109/2015lab5
