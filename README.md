# MetroScan: NYC Subway Ridership Analysis

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Nishanth-G-Palaniswami/metroscan-nyc-subway-ridership/blob/main/MetroScan_NYC_Subway_Ridership_and_Delay_Detection.ipynb)

A PySpark notebook over hourly NYC MTA (Metropolitan Transportation Authority) subway ridership. It flags ridership anomalies with a rolling average, clusters stations by their 24-hour ridership profile, and trains regression models to predict hourly ridership.

## What the notebook does

1. **Rolling-average anomaly flagging.** A 24-period rolling mean per station; hours with significantly low ridership relative to the rolling mean are flagged. The notebook treats these dips as a proxy for service disruptions. That proxy is not validated against MTA delay or incident records, so read "delay-like anomaly" as "unexplained ridership dip."
2. **Station clustering.** Each station is pivoted to 24 hourly features, reduced with PCA, and clustered with K-Means into station types (commuter-heavy, tourist-heavy, and so on). Average hourly ridership is plotted per cluster.
3. **Ridership regression.** Linear Regression, Random Forest, and Gradient Boosted Trees (GBT) trained on `log_ridership`, evaluated on raw ridership with RMSE (Root Mean Square Error) on a held-out test split.

## Recorded results

| Model | Test RMSE (ridership count) |
|---|---|
| Linear Regression | 117.45 |
| Random Forest (50 trees) | 114.48 |
| Gradient Boosted Trees (50 iterations) | **104.12** |

Charts: weekday vs weekend hourly ridership, per-station ridership with rolling average and flagged anomalies, cluster profiles.

## Data

Source: hourly subway ridership published by the MTA (available through [data.ny.gov](https://data.ny.gov)).

The notebook does **not** download the raw data. It mounts Google Drive and reads a precomputed feature table, `mta_hourly_features.parquet`, built in an earlier PySpark ETL step that reduced the raw feed (about 110M rows, 2020 to 2024) to a 16M-row hourly feature table. That ETL step is not included in this repository. To run the notebook you need that Parquet file in your own Drive at the path referenced in cell 4, or you need to rebuild it from the public dataset.

## Run

1. Open the notebook in Colab with the badge above.
2. Place `mta_hourly_features.parquet` in your Google Drive and update the path in cell 4 if it differs from `/content/drive/MyDrive/BD_Project/`.
3. Run all cells.

Local alternative:

```bash
git clone https://github.com/Nishanth-G-Palaniswami/metroscan-nyc-subway-ridership.git
cd metroscan-nyc-subway-ridership
pip install -r requirements.txt
jupyter notebook MetroScan_NYC_Subway_Ridership_and_Delay_Detection.ipynb
```

You will still need the feature Parquet; replace the Drive mount cell with a local path.

## Requirements

Python 3.8+, PySpark, pandas, NumPy, scikit-learn, Matplotlib, Plotly, PyArrow. See `requirements.txt`.

## Project structure

```
.gitignore
LICENSE                                                   MIT
README.md
requirements.txt
MetroScan_NYC_Subway_Ridership_and_Delay_Detection.ipynb  the notebook
```

## License

MIT, see [LICENSE](LICENSE).

## Author

Nishanth Ganapathy Palaniswami · [LinkedIn](https://www.linkedin.com/in/nishanth-g-palaniswami)
