# attrition-governance

Study notes for **"Governance-Aware Evaluation of HR Attrition AI: A Nine-Stage Diagnostic Framework"** (under review, Discover AI).

The nine stages assess discrimination, calibration, feature attributions, subgroup errors, and clustering on the synthetic IBM HR benchmark. The reported silhouette score of 0.236 falls below the study's 0.30 threshold for using the clusters.

## Repository contents

[The analysis script](hardened_pipeline.py) runs the five-fold evaluation and writes [the results](hardened_results.json). The output covers calibration, subgroup errors, the proxy screen, and the clustering check in the table below.

## Reproduce the results

Download the dataset using the [data instructions](data/README.md). From the repository root, run the following commands.

```bash
python -m pip install -r requirements.txt
python hardened_pipeline.py
```

This reruns the five-fold evaluation. Separate holdout and figure outputs fall outside this check.

## Results summary

| Metric | Value |
|---|---|
| AUC (5-fold CV) | 0.801 ± 0.025 |
| ECE (pooled) | 0.053 |
| DPD (Gender) | 0.015 |
| EOD (Age) | 0.266 |
| Proxy MI (StockOptionLevel) | 0.426 |
| Clustering silhouette | 0.236 (below the 0.30 threshold) |

## Evaluation stages

1. Stratified 5-fold cross-validation
2. Calibration audit with ECE, reliability diagrams, post-hoc correction
3. Selective prediction with coverage-accuracy under confidence thresholds
4. SHAP attribution with feature importance with bootstrap stability
5. Subgroup fairness with Demographic Parity Difference, Equalized Odds Difference
6. Proxy variable audit with mutual information screen for protected-characteristic proxies
7. Clustering diagnostics with PCA-projected KMeans with silhouette gating
8. Governance dashboard that collects the metrics
9. Model card with data provenance and deployment conditions

## Dataset

The [dataset notes](data/README.md) give the download link and file path for the synthetic IBM HR Analytics Employee Attrition & Performance dataset. It has 1,470 rows and 35 columns.
