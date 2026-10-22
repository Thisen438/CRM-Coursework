# CRM Coursework: Market Analysis and Stock Segmentation Using RFM

Code and data for the individual coursework of **6BUIS017W CRM & CM with Business Intelligence** at the University of Westminster (2026/27).

The project segments S&P 500 shares in two ways and compares the results:

- **Risk and return**: mean weekly return and weekly beta against the S&P 500 benchmark (`^GSPC`).
- **Trading activity**: an adapted Recency, Frequency, Monetary (RFM) model built on unusually high volume trading days.

Both feature sets are clustered with agglomerative (Ward) clustering and K means, giving four baseline solutions, followed by an RFM sensitivity analysis.

> RFM is normally a customer analytics tool. Here it is adapted to describe share trading activity only. It does not measure company value, investment profitability or future returns.

## Study setup

| Item | Value |
| --- | --- |
| Universe | S&P 500 constituents, taken from the [Wikipedia list](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies) |
| Benchmark | S&P 500 index, `^GSPC` |
| Period | 1 January 2021 to 1 January 2025 (end date exclusive) |
| Price data | Daily prices, adjusted prices and volume (yfinance or YahooFinancials) |
| Environment | Python in Google Colab |

## Method overview

### 1. Data preparation and feature engineering
- Download the ticker list (with retrieval date), share prices and benchmark data, and keep a saved snapshot of the data.
- Validate the data: duplicate dates, missing values, nonpositive prices, volume errors, unequal histories and corporate actions. Exclusions are logged, not silently filled.
- Resample to Friday-ending weeks and align share and benchmark returns on identical week labels.
- Features:
  - **Mean weekly return**: arithmetic mean of weekly adjusted close returns.
  - **Weekly beta**: covariance of share and benchmark weekly returns divided by benchmark variance.
  - **Recency (R)**: calendar days from the latest high volume day to the observation date.
  - **Frequency (F)**: count of high volume days in the latest 60 trading sessions.
  - **Monetary (M)**: sum of Close × Volume over the latest 60 trading sessions (approximate traded value in USD).
- A high volume day is one where volume exceeds twice the mean volume of the preceding 20 sessions.

### 2. Stock segmentation and sensitivity analysis
- Two feature matrices: risk/return and RFM, with `log1p` applied to M and standardised inputs.
- Agglomerative clustering (Ward linkage, Euclidean distance) with dendrograms, and K means with a fixed seed and multiple initialisations.
- Cluster counts from 2 to 8 compared using silhouette scores, inertia, cluster sizes and profiles.
- Cluster profiles in original units and labelled 2D visualisations.
- Correlation analysis of R, F and log1p(M), and a repeat of the Ward RFM solution without Monetary.

### 3. Business intelligence and critical review
- A data mart proposal for storing the features and cluster results.
- A review of what the clusters can and cannot tell an investment analyst or a fintech BI stakeholder.

## Repository structure

```
.
├── notebooks/   Colab notebook(s) with the full analysis
├── data/        Ticker list snapshot, cached price data, feature tables
├── outputs/     Cluster assignments, figures and dendrograms
└── README.md
```

Folders are added as the work progresses.

## How to run

1. Open the notebook in `notebooks/` in Google Colab.
2. Run all cells in order. The notebook installs any packages it needs and records package versions.
3. If live data access fails, the notebook can use the cached dataset in `data/`.

## Author

Individual coursework submitted to the University of Westminster. Module leader: Dr Panagiotis Chountas.
