# Chocolate Data Analysis

Pandas-based analysis of a dataset of ~1,800 chocolate bars (company, bean origin, cocoa percentage, rating, bean type).
The pipeline cleans the data, estimates a price per 100 g, and selects products from the top-rated companies.

## Pipeline

| Notebook | Description |
|---|---|
| `notebooks/01_preprocessing.ipynb` | Clean column names and dtypes, convert `Cocoa Percent` to float, plot its distribution |
| `notebooks/02_pricing.ipynb` | Rescale ratings to 0–100, compute `price(100g)`, select dark chocolates (cocoa > 70%), apply +10% for Trinitario beans |
| `notebooks/03_best_companies.ipynb` | For non-dark bars, compute per-company mean rating (2012 onward), select the top 10 companies and their products |

## Structure

```
data/raw/         input dataset
data/processed/   notebook outputs
notebooks/        run in order: 01 -> 02 -> 03
```

## Usage

```bash
pip install -r requirements.txt
jupyter lab
```

Run the notebooks in order from the `notebooks/` directory; paths are relative to it.
