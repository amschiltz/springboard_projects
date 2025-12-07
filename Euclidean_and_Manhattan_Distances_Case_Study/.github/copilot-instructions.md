# Copilot Instructions for Euclidean_and_Manhattan_Distances_Case_Study

## Project Overview
This project is a data science case study focused on visualizing and comparing Euclidean and Manhattan distance metrics. It is implemented as a Jupyter Notebook and uses a small CSV dataset for demonstration.

## Key Files & Structure
- `Euclidean_and_Manhattan_Distances_Case_Study.ipynb`: Main notebook containing all code, explanations, and visualizations.
- `data/distance_dataset.csv`: Input dataset for analysis.
- `plots/`: Directory for saving generated figures (optional, not required for notebook execution).

## Data Flow
- Data is loaded from `data/distance_dataset.csv` into a pandas DataFrame.
- Distance calculations (Euclidean, Manhattan) are performed using numpy and scipy.
- Results are visualized using matplotlib within the notebook.

## Developer Workflow
- All work is performed interactively in the notebook. No build or test scripts are present.
- To run or modify code, open the notebook in VS Code or Jupyter and execute cells sequentially.
- Visualizations are generated inline; optionally, figures can be saved to `plots/`.

## Patterns & Conventions
- Use pandas for data loading and manipulation.
- Use numpy for manual distance calculations.
- Use scipy's `scipy.spatial.distance` for optimized distance matrix computation.
- Use matplotlib for all plotting; style is set to `'ggplot'` for consistency.
- Reference points for distance calculations are hardcoded (e.g., (5,5), (3,3), (4,4)).
- Notebook cells are annotated with markdown instructions for user tasks.

## Integration Points
- No external APIs or services are used.
- All dependencies are standard Python data science libraries: numpy, pandas, matplotlib, scipy.

## Example Patterns
- Loading data:
  ```python
  df = pd.read_csv('data/distance_dataset.csv', index_col=0)
  ```
- Calculating Euclidean distance:
  ```python
  distEuclid = np.sqrt((df.Z - 5)**2 + (df.Y - 5)**2)
  ```
- Calculating Manhattan distance:
  ```python
  distManhattan = np.abs(df.X - 5) + np.abs(df.Z - 5)
  ```
- Visualizing with matplotlib:
  ```python
  plt.scatter(df.Y - 5, df.Z - 5, c=distEuclid, s=20)
  plt.colorbar()
  ```

## Recommendations for AI Agents
- Focus on notebook cell logic and markdown guidance.
- Maintain code clarity and cell-level documentation.
- Use the provided dataset and avoid introducing new external dependencies.
- Save plots to `plots/` only if explicitly instructed.

---
If any section is unclear or missing important project-specific details, please provide feedback to improve these instructions.