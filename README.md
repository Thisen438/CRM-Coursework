# CRM-Coursework

6BUIS017W CRM & CM with BI, CWK1: Market Analysis and Stock Segmentation Using RFM.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Thisen438/CRM-Coursework/blob/main/CWK1_Stock_Segmentation_RFM.ipynb)

## Contents

| Path | Description |
|---|---|
| `CWK1_Stock_Segmentation_RFM.ipynb` | Google Colab notebook with all code and outputs |
| `data/` | Saved data snapshot (raw daily prices, benchmark, constituent list, acquisition log with SHA-256 hashes, feature tables, cluster assignments, figures) |

## Task 1: Data preparation and feature engineering

- S&P 500 constituents from Wikipedia; daily Close, Adj Close, Volume, dividends and splits from Yahoo Finance (`yfinance`), 1 Jan 2021 to 1 Jan 2025 (end exclusive), benchmark ^GSPC.
- Validation: 486 shares included, 17 excluded (no data, unequal history, zero volume), each with a logged reason. No prices are filled.
- Weekly (W-FRI) Adj Close returns: mean weekly return and weekly beta against ^GSPC over 207 weeks.
- Adapted volume RFM at 31 Dec 2024: high-volume day = volume > 2 × mean of the previous 20 sessions; Recency in calendar days; Frequency and Monetary (Close × Volume) over the latest 60 sessions.

## Task 2: Stock segmentation and sensitivity analysis

- Two separate standardised matrices: risk-return [mean weekly return, weekly beta] and trading activity [log1p Recency, Frequency, log1p Monetary].
- Ward (Euclidean) and K-means (seed 42, n_init 20) for k = 2 to 8, compared with silhouette, inertia, cluster sizes and original-unit profiles. Selected: Ward risk-return k = 5, RFM k = 3; K-means risk-return k = 5, RFM k = 4.
- Profiles in original units, PCA view of the RFM clusters, adjusted Rand index comparisons and K-means seed-stability checks.
- RFM sensitivity: correlations among R, F and log1p(M), and a Ward rerun with R and F only (ARI 0.349).
- Cluster labels for every share: `data/cluster_assignments.csv`.

## Reproducing the results

1. Open the notebook in Colab with the badge above.
2. Upload the files in `data/` to a `data/` folder in the Colab session.
3. Run the setup cells, then the reload cell in the Appendix instead of the three 1.1 download cells, then run from 1.2 onwards.

Downloading again from Yahoo Finance and Wikipedia gives a current constituent list and may give slightly different adjusted prices, so the saved snapshot is the reference dataset.
