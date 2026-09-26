# COVID-19 Patient State Classification

Completed Springboard Random Forest case study using the supplied historical South Korea patient dataset. Includes all notebook exercises, logistic regression, a majority baseline, training-only cross-validation, per-class evaluation and a disease-field sensitivity check.

## Run

Use Python 3.12. Open a terminal in this folder:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab
```

Open `RandomForest_COVID19_Completed.ipynb` and choose Restart Kernel and Run All Cells. On Windows activate with `.venv\Scripts\activate`. Preserve the `SouthKoreacoronavirusdataset/PatientInfo.csv` relative path. Graphviz is not needed. All 31 nonempty code cells executed sequentially using a Python harness, with saved plots and outputs; local Jupyter launch was not tested.

## Data and scope

2,218 rows; 88 missing targets excluded, leaving 2,130 labeled observations. States: 1,791 isolated, 307 released and 32 deceased. Stratified 80/20 split gives 1,704 training and 426 test rows. Confirmation dates in the actual file span January 20–March 18, 2020. Approximate age uses 2020 minus birth year.

This predicts recorded snapshot status, not eventual mortality or clinical prognosis. Isolated outcomes are unresolved. Unknown targets are not imputed and are not a fourth class.

## Results

| Model | Test accuracy | Balanced accuracy | Macro F1 |
|---|---:|---:|---:|
| Majority baseline | 84.04% | 33.33% | 0.3044 |
| Random forest | 82.63% | 70.44% | 0.6851 |
| Logistic regression | 84.51% | 68.52% | 0.6817 |

The forest is selected by training CV macro F1 (0.6891 versus 0.6687), but logistic regression has higher test accuracy and lower log loss. Released-state recall is poor: 17.74% for forest and 8.06% for logistic regression. Six deceased test cases are correctly classified by both; the sample is too small and documentation bias too concerning to support clinical claims.

All 19 recorded disease=True values are deceased. Removing this field reduces CV macro F1 to 0.6264 for the forest and 0.5800 for logistic regression. Its collection timing is unverified. This sensitivity check does not prove leakage, but shows the need to investigate it.

## Implementation notes

- Requested numeric mean filling is demonstrated using training means. `global_number` in the prompt is `global_num` in this CSV.
- Missing disease maps to 0 per the assignment; this means undocumented, not confirmed absence.
- Modeling uses a pre-imputation copy and fits preprocessing within each training CV fold.
- Identifiers and date fields, especially release/death dates, are excluded. Redundant age representations are excluded.
- One-hot encoding handles unseen categories; numeric scaling supports logistic regression.
- Confusion matrices use actual three-class order; feature importances sort all encoded features before selecting the top 20.
- Includes `Model_Comparison.csv` and `Cross_Validation_Results.csv`. Raw data is unchanged.

## GitHub submission

Suggested repository: `covid19-patient-state-classification`.

Description: Comparing random forest and logistic regression for historical COVID-19 patient-state classification, with data cleaning, cross-validation, class imbalance analysis, and model evaluation.

Extract this ZIP, review the answers, upload the entire folder to a public GitHub repository, and submit the notebook page URL. Verify it in a signed-out/private window. Do not upload .venv. No repository was created or uploaded by this deliverable.

Original assignment and dataset: supplied by Springboard, with original notebook attribution to Kaggle DS4C. Solution prepared with ChatGPT assistance; review and understand the work before submitting.
