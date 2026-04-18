# EU27 Climate Target Achievement Analysis
### Fit-for-55 | Country Clustering | 2030 Forecast

This project analyses whether each of the 27 EU member states is on track to meet their binding 2030 climate targets under the **Fit-for-55** package, using 20 years of historical data (2005–2024), country segmentation via clustering, and a cluster-informed predictive model.

---

## The Question

> *Based on current trajectories, which EU member states will meet their 2030 Fit-for-55 targets — and which will not?*

We answer this across three binding dimensions:

| Dimension | What it measures | Direction |
|-----------|-----------------|-----------|
| **GHG Emissions** | Total greenhouse gas output (MtCO₂eq) | Must decrease ↓ |
| **Final Energy Consumption (FEC)** | Total energy used across all sectors (Mtoe) | Must decrease ↓ |
| **Renewable Energy Share (RES)** | Share of renewables in total energy mix (%) | Must increase ↑ |

---

## Methodology

The analysis follows a three-stage pipeline:

### 1. Country Segmentation (Clustering)
- Features: 18 sector-level variables across FEC, GHG, and RES — **GDP, population, and composite gap score were deliberately excluded** to cluster on energy behaviour rather than country size or wealth
- Algorithm: K-Means, k=4, validated with elbow method, silhouette score, Davies-Bouldin index, and Ward dendrogram
- Result: 4 country archetypes

| Cluster | Countries | Archetype |
|---------|-----------|-----------|
| C1 | France, Italy, Poland, Spain | Large Emitters |
| C2 | Belgium, Bulgaria, Croatia, Cyprus, Czechia, Estonia, Greece, Hungary, Ireland, Lithuania, Luxembourg, Malta, Netherlands, Romania, Slovakia, Slovenia | Heterogeneous Middle |
| C3 | Austria, Denmark, Finland, Latvia, Portugal, Sweden | Green Transition Leaders |
| C4 | Germany | Singleton |

### 2. Adaptive Window Selection
For each country-dimension pair, OLS trends are fitted on both the **full 2005–2024 history** and the **post-2015 window**. The window with the higher R² is used — allowing countries with genuine post-2015 acceleration to be judged on their recent trajectory.

### 3. Cluster-Informed Shrinkage Forecast
Individual country trends are blended with the cluster pooled trend using R² as the shrinkage weight:

```
blended_slope = R² × individual_slope + (1 − R²) × cluster_slope
```

- High R² → model trusts the country's own trend
- Low R² → model leans on the cluster's pooled trajectory
- Output: 2030 projection with 90% prediction interval per country per dimension

---

## Repository Structure

```
eu27-climate-forecast/
├── data/
│   ├── EU27_ESG_Panel_2005_2024.csv        ← main panel (20 years, 27 countries)
│   ├── EU27_sectors_scaled.csv             ← clustering input (standardised)
│   ├── EU27_sectors.csv                    ← clustering input (raw)
│   ├── EU27_cluster_labels_reduced.csv     ← final cluster assignments (k=4)
│   ├── EU27_cluster_labels.csv             ← cluster assignments (full features)
│   ├── final_forecast.csv                  ← final model results
│   ├── forecast_results.csv                ← baseline forecast results
│   └── forecast_shrinkage.csv              ← shrinkage model results
├── notebooks/
│   ├── EU27_clustering_reduced.ipynb       ← clustering (reduced features)
│   ├── EU27_clustering_analysis.ipynb      ← clustering (full features + k selection)
│   ├── EU27_forecast_model.ipynb           ← baseline forecast
│   ├── EU27_forecast_shrinkage.ipynb       ← shrinkage forecast
│   └── EU27_final_model.ipynb              ← final integrated model ← start here
├── requirements.txt
└── README.md
```

---

## Getting Started

### 1. Clone the repo
```bash
git clone git@github.com:clabalong/eu27-climate-forecast.git
cd eu27-climate-forecast
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the notebooks
Open Jupyter and run in this order:

```
EU27_clustering_reduced.ipynb   →   EU27_final_model.ipynb
```

The final model notebook (`EU27_final_model.ipynb`) is self-contained and is the best place to start if you want to see the full pipeline end to end.

---

## Key Findings

- **7/27** member states are on track for GHG targets
- **6/27** are on track for Final Energy Consumption targets
- **8/27** are on track for Renewable Energy Share targets
- The Large Emitters cluster (FR, IT, PL, ES) is off track on all three dimensions
- Sweden is the only country on track for RES
- Germany has the largest absolute GHG gap but shows meaningful post-2015 acceleration

---

## Contributors

| Name | GitHub |
|------|--------|
| Tuna Erdem | [@clabalong](https://github.com/clabalong) |
| Xu Liu | [@liuxuu-git](https://github.com/liuxuu-git) |
| Sumant Chirde | [@sumantchirde](https://github.com/sumantchirde) |

---

## Data Sources

- National Energy and Climate Plans (NECPs) — country-specific 2030 targets
- Eurostat — historical FEC, GHG, and RES data
- EU Fit-for-55 legislative package — regulatory framework

---

## License

This project is for academic purposes.
