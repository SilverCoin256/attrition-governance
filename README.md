# attrition-governance

Code for **"Governance-Aware Evaluation of HR Attrition AI: A Nine-Stage Diagnostic Framework"** (under review, Discover AI).

Builds a structured evaluation pipeline for attrition prediction covering the full range of properties relevant to organizational AI governance: discriminative performance, calibration, explainability, fairness, proxy variable auditing, and segmentation validity. Applied to the IBM HR attrition benchmark.

## Why this exists

Standard HR attrition pipelines report AUC and maybe a confusion matrix. This project treats the evaluation itself as the research contribution — specifically, what properties need to be surfaced before a model output can responsibly inform an HR decision. Calibration, subgroup fairness, SHAP attribution stability, and clustering diagnostics are not optional extensions; on real organizational data they routinely reveal failures that discriminative accuracy hides.

## Results summary

| Metric | Value |
|---|---|
| AUC (5-fold CV) | 0.801 ± 0.025 |
| ECE (pooled) | 0.053 |
| DPD (Gender) | 0.015 |
| EOD (Age) | 0.266 |
| Proxy MI (StockOptionLevel) | 0.426 |
| Clustering silhouette | 0.236 (threshold 0.30 — not passed) |

Full output in `hardened_results.json`.

## Evaluation stages

1. Stratified 5-fold cross-validation
2. Calibration audit — ECE, reliability diagrams, post-hoc correction
3. Selective prediction — coverage-accuracy under confidence thresholds
4. SHAP attribution — feature importance with bootstrap stability
5. Subgroup fairness — Demographic Parity Difference, Equalized Odds Difference
6. Proxy variable audit — mutual information screen for protected-characteristic proxies
7. Clustering diagnostics — PCA-projected KMeans with silhouette gating
8. Governance dashboard — all metrics in a single summary artifact
9. Model card documentation — provenance and deployment condition flags

## Structure

```
hardened_pipeline.py   # full evaluation (stages 1–7)
generate_figures.py    # all paper figures
hardened_results.json  # verified output snapshot
data/README.md         # how to get the dataset
figures/               # generated plots
requirements.txt
```

## Reproduce

```bash
pip install -r requirements.txt
# download dataset first — see data/README.md
python hardened_pipeline.py     # writes hardened_results.json
python generate_figures.py      # writes figures/
```

Every number in the manuscript traces to `hardened_results.json`.

## Dataset

IBM HR Analytics Employee Attrition & Performance — 1,470 observations, 35 features, synthetic, no real employee records.
https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
