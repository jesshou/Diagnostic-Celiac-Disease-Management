# HormoniQ — PCOS Risk Prediction

A machine learning tool that estimates the probability of Polycystic Ovary Syndrome (PCOS) from a set of clinical and lifestyle measurements. Built at **HackPrinceton 2024**.

> **Note on the repo name:** although this repository is named *Diagnostic-Celiac-Disease-Management*, the code and dataset here are about **PCOS**, not celiac disease. The name is a leftover from the fork.

## What it does

`algorithm.py` trains a [Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) classifier on the included `PCOS Dataset.csv` and outputs the estimated probability that a patient has PCOS based on their input features. The pipeline:

1. **Loads** the PCOS dataset.
2. **Cleans** it — coerces string columns to numbers, fills missing values (median for continuous fields like `AMH`, `II beta-HCG`, `Marriage Status`; mode for categorical fields like `Fast food`), strips whitespace from column names, and drops any remaining non-numeric rows.
3. **Selects features** — drops identifiers and a handful of low-signal / leaky columns (`Sl. No`, `Patient File No.`, `Blood Group`, `TSH`, `Waist:Hip Ratio`, etc.), using `PCOS (Y/N)` as the target label.
4. **Trains** a `RandomForestClassifier` on a 70/30 train/test split and prints test accuracy.
5. **Predicts** — feeds a sample feature vector to the model and prints the probability of PCOS vs. non-PCOS.

## Getting started

### Requirements

- Python 3.8+
- pandas, numpy, matplotlib, seaborn, scikit-learn

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Run it

```bash
python algorithm.py
```

## Repository structure

```
.
├── algorithm.py       # Data cleaning, model training, and prediction
├── PCOS Dataset.csv   # Training data
└── README.md
```

## Known limitations / next steps

A few things a future version would want to fix — worth calling out honestly:

- **Hard-coded file path.** The dataset is loaded from an absolute Windows path (`C:\Users\jessi\...`). Change this to a relative path so the script runs anywhere:
  ```python
  data = pd.read_csv("PCOS Dataset.csv")
  ```
- **"User input" is a placeholder.** Despite the `Please enter the following details:` prompt, the input vector is hard-coded rather than actually read from the user. A real version would collect the 37 feature values interactively or via a form.
- **`StandardScaler` is instantiated but unused.** The prediction path notes scaling but applies none — the scaler is never fit or transformed. Either drop it or fit it on the training data and reuse it at inference.
- **No fixed random seed.** `train_test_split` and the classifier run without a `random_state`, so accuracy varies between runs. Set one for reproducibility.

## Disclaimer

This is a hackathon project for educational purposes only. It is **not** a medical device and must not be used for actual diagnosis. Consult a qualified healthcare professional for any medical concerns.

## Acknowledgments

Built at HackPrinceton 2024. Forked from [Riptiva-Roy/HormoniQ](https://github.com/Riptiva-Roy/HormoniQ).
