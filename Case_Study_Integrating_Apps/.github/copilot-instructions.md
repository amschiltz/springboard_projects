# AI agent quickstart for this repo

Use these repo-specific rules to be productive immediately.

## Project scope and structure
- This folder is a single, self-contained case study: `Case_Study_Integrating_Apps`.
- Artifacts:
  - Data CSVs: `AppleStore.csv`, `appleStore_description.csv`, `googleplaystore.csv`, `googleplaystore_user_reviews.csv` (kept at repo root of this folder).
  - Three guided notebooks: `Springboard Apps project - Tier 1/2/3 - Complete.ipynb` that lead from basic loading/cleaning to analysis and a permutation test.
- There is no package/module layout; work happens inside notebooks with pandas/numpy/matplotlib.

## Core workflow (what to run and how)
- Open and run notebooks in order: Tier 1 → Tier 2 → Tier 3.
- Expected libraries: pandas (pd), numpy (np), matplotlib.pyplot (plt). scipy/random may appear in text; Tier 3 contains concrete `import pandas as pd`, `import numpy as np`, `import matplotlib.pyplot as plt`.
- Data is loaded directly from local CSV filenames (relative paths). Example from Tier 3:
  - `google = 'googleplaystore.csv'`
  - `Google = pd.read_csv(google)`
- Typical analysis steps reflected across tiers:
  - Subsetting columns to comparable schema:
    - Google: `['Category', 'Rating', 'Reviews', 'Price']`
    - Apple: `['prime_genre', 'user_rating', 'rating_count_tot', 'price']`
  - Type fixes: convert `Price` and `Reviews` to numeric, e.g. remove `$` then `pd.to_numeric`.
  - Add a `platform` column with constant values 'google' or 'apple'.
  - Combine/summarize by `platform` using `groupby()['Rating'].describe()` and visualize distributions.
  - Tier 3 runs a permutation/shuffle test by creating a `Permutation1` column via shuffling `Rating` and comparing grouped summaries by `platform`.

## Conventions and patterns you should follow
- Use consistent column naming as in notebooks; don’t silently rename beyond the specified subsets unless a step explicitly does so.
- When fixing types:
  - Google `Price` may include non-numeric artifacts (e.g., stray labels like 'Everyone'); filter out, strip `$`, then cast with `pd.to_numeric(errors='coerce')` and validate with `df.dtypes`.
  - Convert `Reviews` to numeric before summaries; drop rows with 0 reviews prior to comparisons when instructed.
- Always add and use a `platform` column before grouped summaries or permutation logic.
- Visuals: simple histograms/boxplots with matplotlib are acceptable; keep grouping by `platform` consistent with analysis text.

## Data dependencies and I/O
- All reading is local via `pd.read_csv(<filename>)` from the same directory; avoid absolute paths.
- Do not write back to the source CSVs. If exporting, prefer a new filename alongside the originals.

## Extending the notebooks (how to contribute safely)
- New analysis should be added as new cells at the end of the relevant Tier notebook; avoid altering earlier instructional prompts.
- If you need reusable code, create small helper cells (not separate modules) to keep the repo flat and reproducible.
- Keep randomness controlled in permutation sections by seeding numpy when adding new randomized analyses: `np.random.seed(42)`.

## Gotchas and edge cases observed here
- Google `Price` contains non-numeric tokens; filter/fix before numeric ops.
- A large number of apps may have `Reviews == 0`; remove these for fair comparisons.
- CSVs rely on default encoding; if a read error occurs, try `encoding='utf-8'`.

## Examples from this repo
- Load and inspect (Tier 3):
  - `google = 'googleplaystore.csv'` → `Google = pd.read_csv(google)` → `Google.dtypes` → `Google['Price'].unique()`
- Create platform and summarize:
  - `Google['platform'] = 'google'; Apple['platform'] = 'apple'`
  - `df = pd.concat([Google_subset, Apple_subset], ignore_index=True)`
  - `df.groupby(by='platform')['Rating'].describe()`
- Permutation idea (Tier 3):
  - `df['Permutation1'] = np.random.permutation(df['Rating'].values)`
  - `df.groupby(by='platform')['Permutation1'].describe()`

## What not to do
- Don’t introduce external ML frameworks or heavy plotting libs; keep to pandas/numpy/matplotlib used here.
- Don’t restructure into packages or scripts; this case study is intentionally notebook-driven.
- Don’t move or rename the CSVs/notebooks; relative-path reads assume current layout.
