# Dynamic Logistic Regression Model — Retail Store Inventory

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/USERNAME/REPOSITORY/blob/main/dynamic_logistic_kaggle.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A binary logistic regression model that is **dynamically linked** to its data source.
Each run loads the latest version of the dataset, re-estimates the model, and reports
the results in SPSS-style tables, together with a reproducibility record
(data source, version, retrieval time and library versions).

**Data source:** [Retail Store Inventory Forecasting Dataset](https://www.kaggle.com/datasets/anirudhchauhan/retail-store-inventory-forecasting-dataset) (Kaggle)

> **Note:** The dataset is synthetic, as stated in its Kaggle description. The model is
> presented as a methodological framework that can be applied to real-world data.

## Repository contents

| File | Description |
| --- | --- |
| `dynamic_logistic_kaggle_v2.py` | Python script (run locally or on a server) |
| `dynamic_logistic_kaggle.ipynb` | Jupyter notebook version (Kaggle, Google Colab, Jupyter) |
| `README.md` | This file |
| `LICENSE` | MIT License |

## Quick start (no installation)

Click **Open in Colab** above, upload `retail_store_inventory.csv` (downloaded from the
Kaggle link) using the folder icon on the left, then select **Runtime → Run all**.

## How the dynamic link works

The script tries these data sources in order and uses the first one that succeeds:

| Order | Source | When it is used |
| --- | --- | --- |
| 0 | `DATA_URL` (public direct link) | When set — e.g. GitHub raw, Zenodo or Google Drive |
| 1 | Kaggle Notebook input folder | When run as a Kaggle Notebook with the dataset attached |
| 2 | `kagglehub` | Latest dataset version (needs a Kaggle API token) |
| 3 | Official Kaggle API | Fallback (needs a Kaggle API token) |
| 4 | Local `retail_store_inventory.csv` | Offline fallback (prints a warning: not live) |

The source actually used is recorded in the **Run Info** sheet of the output file.

## Output

The script prints all tables and saves them to `logistic_results.xlsx`:

| Sheet | Contents |
| --- | --- |
| Run Info | Data link, source, version, retrieval time, N, library versions |
| Block 0 | Variables in the Equation — constant only |
| Omnibus Test | Omnibus Tests of Model Coefficients |
| Model Summary | -2 Log likelihood, Cox & Snell R², Nagelkerke R² |
| Classification | Classification table (cut value 0.5) |
| Block 1 | Variables in the Equation — with predictors and 95% C.I. for Exp(B) |
| Predictions | Original data with predicted probability and group per case |

## Running locally

**Requirements:** Python 3.9 or later.

```bash
pip install pandas numpy scipy statsmodels openpyxl kagglehub
python dynamic_logistic_kaggle_v2.py
```

In Jupyter Notebook, install with `%pip install ...` inside the notebook so the packages
go into the notebook's own environment, then restart the kernel.

**Kaggle API token** (only needed for sources 2 and 3): go to kaggle.com → Settings → API →
Create New Token, then place `kaggle.json` in `~/.kaggle/` (Windows: `C:\Users\<you>\.kaggle\`).

## Configuration

Edit the `SETTINGS` section at the top of the script or notebook:

| Setting | Default | Description |
| --- | --- | --- |
| `DATA_URL` | `""` | Optional public direct link to the CSV |
| `TARGET` | `Holiday/Promotion` | Binary dependent variable |
| `NUMERIC` | Price, Discount, Competitor Pricing, Units Sold, Inventory Level | Numeric predictors |
| `CATEGORICAL` | Category, Region, Weather Condition, Seasonality | Categorical predictors (dummy-coded, first level as reference, as in SPSS Indicator(1)) |
| `SAMPLE_SIZE` | `None` | Random subsample size to match a study sample (`None` = all rows) |
| `RANDOM_SEED` | `42` | Seed for the subsample |
| `CUT_VALUE` | `0.5` | Classification cut value |

## Reproducibility

Because the default sources always load the latest dataset version, results may change if
the data is updated. The version used in the associated study is archived on Zenodo
(see the DOI badge above). Record the values from the **Run Info** sheet when reporting results:

| Item | Value used in the study |
| --- | --- |
| Data retrieval date | ... |
| Dataset version | ... |
| Cases in model (N) | ... |
| Python / pandas / statsmodels / scipy | ... |

Never commit `kaggle.json` — it contains your personal API key.

## Citation

If you use this code, please cite:

> [Author]. (2026). *Dynamic logistic regression model for retail store inventory*
> (Version 1.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX

And the dataset:

> Chauhan, A. (2024). *Retail store inventory forecasting dataset* [Data set]. Kaggle.
> https://www.kaggle.com/datasets/anirudhchauhan/retail-store-inventory-forecasting-dataset

## License

The code is released under the [MIT License](LICENSE). The dataset is subject to its own
license on Kaggle.
