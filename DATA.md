# Dataset

The paper cites the Machine Learning Group at ULB's [Credit Card Fraud Detection dataset on Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud/data).

Download `creditcard.csv` from that source and place it at `data/creditcard.csv`. Follow the source's access and usage terms. This repository does not redistribute the dataset or require access to the author's Google Drive.

The supplied notebook's recorded data inspection shows:

- 284,807 transaction rows and 31 columns.
- 492 fraud labels and 284,315 legitimate labels.
- Columns `Time`, `V1` through `V28`, `Amount`, and `Class`.
- No missing values reported in the saved inspection.

The paper describes European cardholder transactions over two days in September 2013, with anonymized principal-component features. The repository does not claim access to raw cardholder identities. Transaction-amount values are retained in the dataset's original units; dollar signs in the historical notebook are not treated as a verified currency designation.

The raw CSV was not supplied with this package, so its hash, current contents, and a fresh training run could not be verified.
