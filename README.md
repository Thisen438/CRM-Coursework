# CRM-Coursework

6BUIS017W CRM & CM with BI, CWK1: Market Analysis and Stock Segmentation Using RFM.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Thisen438/CRM-Coursework/blob/main/CWK1_Stock_Segmentation_RFM.ipynb)

## Contents

| Path | Description |
|---|---|
| `CWK1_Stock_Segmentation_RFM.ipynb` | Google Colab notebook with all code and outputs (Tasks 1 to 3) |
| `data/` | Saved data snapshot (raw daily prices, benchmark, constituent list, acquisition log with SHA-256 hashes and package versions, feature tables, cluster assignments, figures) |

## Reproducing the results

1. Open the notebook in Colab with the badge above.
2. Select **Runtime → Run all**.

The setup cell copies the saved `data/` snapshot from this repository, so no files need to be uploaded. The 1.1 download cells only run when `REFRESH_DATA = True`. With the default `False`, every result is rebuilt from the snapshot retrieved on 22 November 2026, and the SHA-256 hashes printed at the end match those in `data/acquisition_log.json`. Setting `REFRESH_DATA = True` downloads a current constituent list and fresh Yahoo Finance prices, which may give slightly different results.

## Task 1: Data preparation and feature engineering

- S&P 500 constituents from Wikipedia; daily Close, Adj Close, Volume, dividends and splits from Yahoo Finance (`yfinance`), 1 Jan 2021 to 1 Jan 2025 (end exclusive), benchmark ^GSPC.
- Validation: 486 shares included, 17 excluded (no data, unequal history, zero volume), each with a logged reason. No prices are filled.
- Weekly (W-FRI) Adj Close returns: mean weekly return and weekly beta against ^GSPC over 207 weeks.
- Adapted volume RFM at 31 Dec 2024: high-volume day = volume > 2 × mean of the previous 20 sessions; Recency in calendar days; Frequency and Monetary (Close × Volume) over the latest 60 sessions.

## Task 2: Stock segmentation and sensitivity analysis

- Two separate standardised matrices: risk-return [mean weekly return, weekly beta] and trading activity [log1p Recency, Frequency, log1p Monetary].
- Ward (Euclidean) and K-means (seed 42, n_init 20) for k = 2 to 8, compared with silhouette, inertia, cluster sizes and original-unit profiles. Selected: Ward risk-return k = 5, RFM k = 3; K-means risk-return k = 5, RFM k = 4.
- Profiles in original units, PCA view of the RFM clusters, adjusted Rand index comparisons and K-means seed-stability checks.
- RFM sensitivity: correlations among R, F and log1p(M), a Ward rerun with R and F only (ARI 0.349), and a robustness check with raw Recency (ARI 0.882).
- Cluster labels for every share: `data/cluster_assignments.csv`.

## Task 3: Evidence for the data mart and critical review

- Sector composition of each clustering against the sample mix (`data/sector_composition.png`).
- Agreement between the risk-return and RFM clusterings (`data/risk_vs_rfm_crosstab.png`).
- Weekly return correlation within and across clusters, with GICS sector as a benchmark (`data/return_dependence.png`).
- Close vs Adj Close betas and returns, and a survivorship check using each share's S&P 500 "Date added".
