# Data

The raw dataset is **not** stored in this repository. Download it and place the CSV here:

```
data/BankChurners.csv
```

**Source:** [Credit Card Customers on Kaggle](https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers) (`sakshigoyal7/credit-card-customers`) - 10,127 rows x 23 columns.

## Option 1 - manual download
Download the zip from Kaggle, unzip it, and copy `BankChurners.csv` into this folder.

## Option 2 - from inside the notebook
If the file is not found, the notebook tries `opendatasets` and will ask for your Kaggle username and API key
(Kaggle -> Settings -> API -> *Create New Token*). Never commit `kaggle.json`.

## On Kaggle
Add the dataset to a Kaggle notebook; the loader finds it at `/kaggle/input/credit-card-customers/BankChurners.csv`.

> The last two columns of the raw file (`Naive_Bayes_Classifier_...`) are leaked model output and are removed in Step 2 of the notebook.
