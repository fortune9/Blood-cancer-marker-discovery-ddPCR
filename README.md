# Blood biomarker discovery

This project evaluates a small blood-based panel for cancer detection using synthetic methylation, protein, and fragmentomics data. The dataset contains 350 samples from 300 patients. After retaining each patient's earliest draw and applying quality filters, 219 patients from sites A and B had the protein measurements needed for modeling.

The [Python notebook](./Biomarker-discovery-and-modeling.ipynb) covers missing-data assessment, exploratory PCA, control-only site and plate correction, and feature selection with LASSO. Imputation, batch correction, and scaling are refitted within each training fold. Five repeats of nested cross-validation estimate held-out performance and marker-selection stability. Panels are capped at five methylation markers, three proteins, and, when included, two fragmentomics features.

## Main findings

| Model | Mean held-out AUC | Sensitivity at empirical 95% specificity |
|---|---:|---:|
| Methylation + protein | 0.890 | 0.573 |
| After repeat-overlap and GC-content filtering | 0.761 | 0.271 |
| Methylation + protein + fragmentomics | 0.888 | 0.531 |

Filtering methylation regions for assay design reduced performance in this cohort. Adding the tested fragmentomics features did not improve the original model. Stage I detection remained weaker than detection of later-stage cancers. These estimates come from repeated splits of the same patients; a fixed panel and threshold still require independent validation.

## Submission files

- [Executed notebook](./Biomarker-discovery-and-modeling.ipynb) — complete analysis, tables, and figures.
- [HTML report](./Biomarker-discovery-and-modeling.html) — browsable notebook output with embedded figures.
- [PDF summary](./blood_biomarker_discovery_summary.pdf) — concise results and interpretation.
- [Run instructions](./how-to-run.md) — environment setup, execution, and HTML export. The analysis used Python 3.13.9; [direct requirements](./requirements.txt) and a [Linux/WSL dependency lock](./requirements-lock.txt) are provided.
- [R–Python output comparison](./R_vs_Python_output_comparison.md) — file-by-file comparison with the reference R analysis.
- `candidate_data/` — the five input CSV files. A fresh run writes 40 result CSVs to `report_artifacts/`.

The random seed and CV settings are recorded in the notebook. Follow [how-to-run.md](./how-to-run.md) to regenerate the results from the included data.
