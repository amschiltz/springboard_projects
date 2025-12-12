# Copilot Instructions for CosineSimilarityCaseStudy

## Project Overview
This project is a data science case study focused on cosine similarity. The main analysis is performed in the Jupyter notebook `Cosine_Similarity_Case_Study.ipynb`, using the dataset `distance_dataset (1).csv`.

## Key Files
- `Cosine_Similarity_Case_Study.ipynb`: Main notebook for all code, analysis, and results.
- `distance_dataset (1).csv`: Input data for the case study.

## Development Workflow
- All code and analysis are performed in the notebook. There are no separate scripts or modules.
- To add new analysis, create new cells in the notebook.
- Data is loaded directly from the CSV file in the same directory.
- No custom build, test, or deployment scripts are present.

## Conventions & Patterns
- Use clear markdown cells to explain each analysis step and result.
- Prefer pandas and numpy for data manipulation; sklearn for similarity calculations.
- Visualizations (if any) should use matplotlib or seaborn.
- Keep all code and results reproducible within the notebook.

## Integration & Dependencies
- Standard Python data science stack (pandas, numpy, sklearn, matplotlib, seaborn).
- No external APIs or services are integrated.

## Examples
- To load the dataset:
  ```python
  import pandas as pd
  df = pd.read_csv('distance_dataset (1).csv')
  ```
- To compute cosine similarity:
  ```python
  from sklearn.metrics.pairwise import cosine_similarity
  similarity = cosine_similarity(df)
  ```

## Additional Notes
- If adding new data, place it in the same directory and document its use in the notebook.
- Keep the notebook clean and well-commented for clarity.

---

_If you need to update these instructions, edit `.github/copilot-instructions.md`._
