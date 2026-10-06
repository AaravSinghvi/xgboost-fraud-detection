# Sources, preserved behavior, and checks

## Result provenance

The paper's Tables 1–2 report rounded results. Figures 13–18 provide confusion counts on a 56,962-row holdout containing 78 fraud cases. The saved notebook also has 56,962 test rows, but 97 fraud cases. Its training-set count of 395 fraud cases plus its 97 test cases agrees with the full recorded count of 492.

There is also an internal discrepancy for the paper's undersampled decision tree: Figure 15 shows TN 51,064, FP 5,820, FN 2, TP 76. These counts imply approximately 89.779% accuracy, 1.289% precision, and 2.544% F1, while Table 1 lists 89.844%, 1.297%, and 2.560%. Recall agrees. `paper_metrics.json` preserves the table values, figure counts, and independently recomputed metrics separately. The XGBoost table values agree with their corresponding matrices to the printed precision.

The paper and notebook must be treated as distinct result snapshots. Unseeded splitting and sampling make variability plausible; the original split indices, exact execution history, and package versions are unavailable. The paper describes repeated 80/20 training circumstances, while the supplied notebook creates one split and shares it across all six configurations.

The paper favors undersampled XGBoost under a recall-first preference. Its no-resampling XGBoost has the highest reported F1. These are different selection criteria, and neither result establishes the best deployment threshold or an operational cost optimum.

## Preserved historical code

The training/evaluation cells are unchanged. The notebook still uses default estimator parameters, an unseeded/unstratified split, and hard class predictions for its ROC and precision–recall plots. The latter values are stored as `hard_label_auc` in the notebook metrics JSON rather than being represented as probability-ranking performance.

Presentation changes are limited to the relative CSV loader, added Markdown sections, removal of Colab-specific display metadata, and a disabled-by-default switch around optional tree rendering. Optional plots retain their plotting code and import their own plotting helpers when enabled. Their old oversized display outputs are omitted. The paper preserves those historical figures.

The historical repeated call to random resampling uses already balanced training arrays, so it is redundant under the default equal-class sampling target. It does not mean the test set was resampled.

## Checks completed

- Python syntax parsing for every code cell.
- Exact source comparison for all original statistical training/evaluation cells.
- Metric extraction for all six notebook configurations.
- Paper result counts and rounded metrics cross-checked against its page 10 figure/table layout.
- Basic notebook JSON structure and local Markdown/image link checks.
- Scoped credential/private-path scan of the source and documentation.
- Paper copy checked for byte-for-byte identity with the supplied file.

Full model training was not rerun: the CSV was not supplied and XGBoost was unavailable in the inspected environment. The dependency installation and optional Graphviz diagrams have not been tested. Packaging checks do not replace an end-to-end reproduction or a full security audit.
