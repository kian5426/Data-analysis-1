# Chocolate Data Analysis

Exploratory data analysis and price estimation on a dataset of 1,795 chocolate bar reviews (2006–2017), built with pandas and seaborn.

The project is a three-stage pipeline: clean the raw data, estimate a price per 100 g from quality and cocoa content, and identify the highest-rated companies in the non-dark segment.

## Dataset

`data/raw/chocolate.csv` contains one row per reviewed bar:

| Column | Description |
|---|---|
| `Company` | Manufacturer |
| `Specific Bean Origin` | Region or farm of the beans |
| `Review Date` | Year of the review |
| `Cocoa Percent` | Cocoa content (`"63%"` in raw data, `float` after cleaning) |
| `Company Location` | Manufacturer's country |
| `Rating` | Expert rating (rescaled to 0–100 in step 2) |
| `Bean Type` | Cocoa bean variety (e.g. Trinitario, Criollo, Blend) |
| `Broad Bean Origin` | Country of bean origin |

## Pipeline

| Step | Notebook | What it does | Output |
|---|---|---|---|
| 1 | [`01_preprocessing`](notebooks/01_preprocessing.ipynb) | Fixes column names containing `\n`, casts text columns to `string`, converts `Cocoa Percent` to `float`, plots its distribution | `chocolate_preprocessed.csv` |
| 2 | [`02_pricing`](notebooks/02_pricing.ipynb) | Rescales `Rating` so the maximum is 100, computes `price(100g)`, selects dark chocolates (cocoa > 70%), applies a +10% premium to Trinitario bars | `chocolate_price.csv`, `dark_chocolates.csv` |
| 3 | [`03_best_companies`](notebooks/03_best_companies.ipynb) | For non-dark bars: builds a company × year mean-rating table, averages ratings from 2012 onward, selects the top 10 companies and their bars | `normal_chocolates.csv`, `companies.csv`, `mean_ratings.csv`, `best_ratings.csv`, `chocolates_to_sell.csv` |

### Pricing model

```
price(100g) = Cocoa Percent × Rating × 25
```

With `Rating` normalized to 0–100, a 100% cocoa, top-rated bar is priced at 250,000 (Toman) per 100 g. Dark Trinitario bars get a further +10%.

### Ranking rule

Companies are ranked by mean rating over all review years ≥ 2012. Ties are broken alphabetically by company name.

## Results

| Metric | Value |
|---|---|
| Bars in dataset | 1,795 |
| Dark bars (cocoa > 70%) | 795 |
| Total price of dark bars | 96,733,662.5 |
| Non-dark bars (cocoa ≤ 70%) | 1,000 |
| Bars from the top-10 companies | 59 |
| Total price of those bars | 7,282,125.0 |

Top 10 companies by mean rating (2012 onward): Patric, Tobago Estate (Pralus), Willie's Cacao, L.A. Burdick (Felchlin), Matale, Pacari, Valrhona, Acalli, Amedei, Askinosie.

## Repository structure

```
.
├── data/
│   ├── raw/                # original dataset
│   └── processed/          # outputs of each notebook
├── notebooks/              # run in order: 01 → 02 → 03
├── requirements.txt
└── README.md
```

## Getting started

```bash
git clone https://github.com/kian5426/chocolate-analysis.git
cd chocolate-analysis
pip install -r requirements.txt
jupyter lab
```

Open the notebooks in `notebooks/` and run them in order. Each notebook reads the previous step's output from `data/processed/`, and all paths are relative to the `notebooks/` directory.

## Tech stack

Python · pandas · NumPy · seaborn · Jupyter
