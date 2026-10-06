# Credit Card Fraud Detection with XGBoost and Decision Trees

Python · XGBoost · scikit-learn · imbalanced-learn · pandas · Seaborn

I compared XGBoost and decision-tree classifiers on **284,807 credit-card transactions**, including **492 fraudulent transactions**. For each algorithm, I evaluated the original class distribution, random oversampling, and random undersampling. My research examines how the preferred model changes when evaluation prioritizes balanced precision and recall versus catching as many fraudulent transactions as possible.

**Author:** Aarav Singhvi. The accompanying paper recognizes publication process with *Journal of Student Research, High School Edition, Volume 13, Issue 4 (2024)* and acknowledges mentorship from Matan Gans.

[Read the paper](paper/fraud-detection-paper.pdf) · [Explore the notebook](fraud_detection.ipynb) · [Data source](DATA.md) · [Methods and reproducibility](METHODS-NOTES.md)

## Research findings

The paper reports two distinct outcomes:

- **XGBoost without resampling** provided the strongest F1 score among the six configurations: **88.44% F1**, **94.20% precision**, and **83.33% recall**. Its confusion matrix records 65 detected frauds, 13 missed frauds, and 4 false alarms.
- **XGBoost with random undersampling** increased recall to **96.15%**, detecting 75 of 78 fraudulent test transactions. Precision fell to **2.72%**, with **2,681 false alarms**. The paper favors this model under its stated recall-first evaluation preference.

The tradeoff is concrete: undersampling caught **10 additional fraudulent transactions** while adding **2,677 false alarms** compared with the original-distribution XGBoost model. The study did not measure operational review costs, customer friction, or financial savings, so this preference should not be interpreted as a demonstrated production optimum.

**Accuracy alone hides the problem.** Since only about 0.173% of the full dataset is fraudulent, predicting every transaction as legitimate would achieve about 99.83% accuracy while detecting no fraud.

## Paper results and saved notebook results

The supplied files record **different experiments**. The paper's matrices contain 78 fraudulent test cases; the notebook's saved run contains 97. The notebook uses an unseeded random train/test split and unseeded samplers, which can produce different results across executions.

The repository keeps both sources explicit:

- [Paper metrics](results/paper_metrics.json): Tables 1–2 and Figures 13–18, page 10.
- [Notebook metrics](results/notebook_metrics.json): saved outputs in the supplied notebook.
  
## What I investigated

1. **Data exploration:** schema, missing values, transaction amounts and timing, and severe class imbalance.
2. **Baseline comparison:** default decision-tree and XGBoost classifiers trained on the same 80/20 split.
3. **Random oversampling:** duplicate minority-class training examples until the training classes are balanced.
4. **Random undersampling:** reduce majority-class training examples to match the minority class.
5. **Evaluation:** inspect precision, recall, F1, accuracy, and confusion matrices to expose the different error patterns.

The original notebook applies resampling to training data after splitting. The test set is left at its natural class distribution. No hyperparameter tuning is claimed for this experiment.

## Run locally

Download `creditcard.csv` from the [Kaggle Credit Card Fraud Detection dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud/data) and put it in `data/`. The CSV is not included in this repository.

From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab fraud_detection.ipynb
```

On Windows PowerShell, activate with `.venv\Scripts\Activate.ps1`. Run the notebook cells in order using the environment's Python kernel.

The notebook's Colab-specific Google Drive mount has been replaced with a relative file path. Optional tree diagrams are disabled by default with `RUN_TREE_PLOTS = False`; XGBoost tree rendering requires the optional Graphviz Python package and system executable. The paper already includes the original diagrams.

The original package versions were not recorded. `requirements.txt` lists dependencies without asserting a tested, locked environment. New executions will not necessarily reproduce either saved result set, especially because the original split and sampler seeds are unspecified. Full model execution was not performed during packaging: the raw CSV was not supplied and XGBoost was unavailable in the inspected local environment.

## Limitations and next experiments

- The random split is not stratified or seeded. A time-based evaluation would answer a different and more realistic question about later transactions.
- The original ROC and precision–recall plots use predicted classes. Their AUC values do not measure ranking quality across model probability scores. Future runs should use `predict_proba` and report average precision alongside ROC AUC.
- The same holdout supports the six-way comparison; no additional untouched test set or multi-seed uncertainty estimate is provided.
- Resampling is invoked on already balanced arrays inside the historical pipelines. This is redundant at the configured balance ratio and should be simplified in a revised experiment.
- The dataset spans two days. These results do not establish performance under changing fraud patterns, across institutions, or on unseen cardholders.
- Oversampling changes class prevalence; model scores should not be assumed to be calibrated fraud probabilities.

## Other Attributes

The PDF is included unchanged. `fraud_detection.ipynb` is a presentation copy of `Data_Preprocessing.ipynb`: it adds explanations, makes loading portable, and disables oversized optional tree diagrams. Original statistical training/evaluation cells and their saved metric outputs are retained. The source files in Downloads are unchanged.
