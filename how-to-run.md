# Reproduce the Python biomarker analysis

The notebook `Biomarker-discovery-and-modeling.ipynb` reproduces the QC, batch-effect, and repeated nested-CV analyses in the R report. It reads the five CSV files in the included `candidate_data/` directory and writes 40 result CSVs to `report_artifacts/`. Run the commands below from `early_cancer_detection_multimodal_python/`.

## Python and environment

The completed run used **CPython 3.13.9 on Linux/WSL**. `requirements.txt` pins the direct packages used by the notebook and its execution/export tools. `requirements-lock.txt` also pins their installed dependencies from that run; use it for the closest match on Linux/WSL. On another operating system, use `requirements.txt` if the lock cannot be installed, and expect small numerical differences.

```bash
python3.13 --version  # 3.13.9 for the recorded environment
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-lock.txt
python -m pip check
python -m ipykernel install --user --name biomarker --display-name "Python (biomarker)"
```

The required inputs are `candidate_data/samples.csv`, `methylation.csv`, `protein.csv`, `fragmentomics.csv`, and `region_annotation.csv`. Keep the original filenames and column names. The notebook's first code cell sets `PROJECT_ROOT`, `INPUT_DIR`, `OUTPUT_DIR`, the random seed, and the CV settings; check these paths if you run it from a different directory.

## Run interactively

```bash
jupyter lab
```

Open `Biomarker-discovery-and-modeling.ipynb`, select **Python (biomarker)**, then choose **Run All** and save the notebook. The full run uses five repeats, five outer folds, and three inner folds, and took about 15 minutes in the tested environment. It creates or replaces the CSV files in `report_artifacts/`.

## Generate HTML

To run the analysis for the first time and export a single HTML file with its tables and figures embedded:

```bash
jupyter nbconvert --execute --to html --embed-images \
  --ExecutePreprocessor.kernel_name=biomarker \
  --ExecutePreprocessor.timeout=-1 \
  Biomarker-discovery-and-modeling.ipynb
```

This writes `Biomarker-discovery-and-modeling.html` beside the notebook and generates the result CSVs. The HTML command does **not** save newly computed outputs back into the `.ipynb` file. To save both an executed notebook and an HTML copy, run these two commands instead:

```bash
jupyter nbconvert --execute --to notebook --inplace \
  --ExecutePreprocessor.kernel_name=biomarker \
  --ExecutePreprocessor.timeout=-1 \
  Biomarker-discovery-and-modeling.ipynb
jupyter nbconvert --to html --embed-images Biomarker-discovery-and-modeling.ipynb
```

If the notebook has already been run and saved, only the second command is needed to refresh the HTML without repeating the analysis.

## Check the results

A complete run produces **40 CSV files** in `report_artifacts/`, including QC and batch-effect tables and outputs for the original, filtered-methylation, and fragmentomics models. For a quick check:

```bash
find report_artifacts -maxdepth 1 -name '*.csv' | wc -l
```
