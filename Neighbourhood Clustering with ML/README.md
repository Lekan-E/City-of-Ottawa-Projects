# Neighbourhood Safety in Ottawa — K-Means Cluster Analysis

A machine learning analysis of eight years of Ottawa Police Service open data, grouping all 104 Ottawa neighbourhoods into six distinct safety profiles using K-Means clustering. The project covers data cleaning, model selection, geospatial visualisation, and cluster-level analysis of crime composition, police resource allocation, and assignment confidence.



**Analysis period:** 2018–2025 &nbsp;|&nbsp; **Neighbourhoods:** 104 &nbsp;|&nbsp; **Total incidents:** 348,767

**Application:** Check out the interactive web application to gain more information about Ottawa neighbourhoods: https://neigbourhood-map.onrender.com

![A](https://github.com/Lekan-E/City-of-Ottawa-Projects/blob/bdff1635f2c4a903609238e409c579a8d8ce9d51/Neigbourhood%20Clustering%20with%20ML/Images/map.png)
---

## Key Findings

- **Centretown forms a cluster of one** — 52,597 incidents over the period, more than the bottom 42 neighbourhoods combined.
- **Seven neighbourhoods drive Ottawa's violence** — Byward Market, Lowertown, Overbrook-McArthur, New Barrhaven/Stonebridge, West Centretown, Ledbury-Heron Gate, and Vanier North account for 46 homicides and 137 shootings across just 73,755 residents (~15× the national homicide rate).
- **Lowertown** leads in homicides (10); **Overbrook-McArthur** leads in shootings (28); **New Barrhaven/Stonebridge** leads in auto theft (605 — more than double any other neighbourhood).
- **Property crime dominates every cluster**, but the violent share rises sharply in the two highest-risk clusters.
- **Suburban clusters are slightly under-resourced** relative to their crime rate on the police critical-call trend line; the mixed urban/suburban cluster (Glebe, Westboro, Sandy Hill, Hintonburg) draws more critical calls than its crime volume alone predicts.
- **Overall cluster confidence is moderate** (silhouette score 0.186). The mixed urban/suburban cluster (avg −0.092) is the least coherent and may benefit from splitting in a future iteration.

---

## Repository Structure

```
├── Data Cleaning.ipynb       # Ingest, clean, and merge 8 OPS datasets → CleanedData.csv
├── Model Building.ipynb      # K-Means model selection (elbow + silhouette) → ClusteredData.csv
├── Cluster Analysis.ipynb    # Five analyses across the six clusters
├── Ottawa_Safety_Report.md   # Full written report with findings
├── ottawa_cluster_map.html   # Interactive choropleth map (output)
├── CleanedData.csv           # 104 neighbourhoods × 24 features
├── ClusteredData.csv         # CleanedData + cluster label (0–5)
└── Dataset/
    └── GEO/                  # Raw OPS GeoJSON source files
```

---

## Notebooks

### 1. Data Cleaning
Ingests eight GeoJSON files (auto theft, bike theft, criminal offences, homicide, shootings, hate crimes, calls for service, neighbourhood boundaries) and produces a single analysis-ready CSV.

- Aggregates each dataset to one row per neighbourhood by incident count
- Drops duplicate features (`Theft - Motor Vehicle` overlaps the auto theft dataset)
- Consolidates split neighbourhood records (`Carp` + `Carp Ridge`)
- Collapses 12 low-signal columns (ambiguous hate crime categories, minor offence types) into a single `Other` feature
- Resolves naming mismatches between the crime dataset and the calls-for-service dataset using polygon unions, manual overrides, and fuzzy matching (`rapidfuzz`, threshold 70)
- Excludes 9 Greenbelt sub-areas with ambiguous boundaries and no residential population

**Output:** `CleanedData.csv` — 104 neighbourhoods × 24 columns

### 2. Model Building
Evaluates K from 2–11 using the elbow method (WCSS) and silhouette scores, selects **K = 6** from the elbow plot, then fits the final K-Means model.

- Features standardised with `StandardScaler` before fitting
- `KMeans(n_clusters=6, init='k-means++', random_state=42)`

**Output:** `ClusteredData.csv` — CleanedData + `cluster` column (labels 0–5)

### 3. Cluster Analysis
Five progressive analyses across the six clusters:

| # | Analysis | What it answers |
|---|----------|-----------------|
| 1 | Interactive choropleth map | Where do the clusters sit geographically? |
| 2 | Crime type composition (stacked bar) | Is each cluster dangerous due to violence or property crime? |
| 3 | Police demand vs. crime scatter plot | Are critical-response resources allocated proportionally? |
| 4 | Feature z-score heatmap | Which specific crime types define each cluster? |
| 5 | Silhouette score per neighbourhood | How confident is each cluster assignment? |

---

## Cluster Summary

| Cluster | Avg Crimes / Neighbourhood | Neighbourhoods | Population | Character |
|---------|---------------------------|----------------|------------|-----------|
| 3 | 804 | 42 | 173,332 | Rural / low-density, cemeteries, industrial |
| 5 | 1,950 | 14 | 135,838 | Low-mid suburban (Hunt Club, Barrhaven area) |
| 0 | 2,152 | 22 | 225,217 | Mid-suburban |
| 4 | 5,847 | 18 | 275,440 | Mixed urban/suburban — property crime dominant |
| 1 | 11,785 | 7 | 73,755 | High-crime urban core — violence dominant |
| 2 | 52,597 | 1 | 24,994 | Centretown — extreme across all crime types |

---

## Data Sources

All data from Ottawa Police Service open data and the Ottawa Neighbourhood Study (ONS).

| Dataset | Period | Records |
|---------|--------|---------|
| Criminal Offences | 2018–2025 | 344,290 |
| Calls for Service | 2021–2025 | 1,108,780 |
| Bike Theft | 2018–2025 | 15,676 |
| Auto Theft | 2018–2025 | 11,720 |
| Shootings | 2018–2025 | 505 |
| Homicide | 2018–2025 | 131 |
| Hate Crimes | 2018–2024 | 1,689 |
| Neighbourhood Boundaries (ONS) | — | 111 |

---

## Stack

`Python` &nbsp;·&nbsp; `pandas` &nbsp;·&nbsp; `geopandas` &nbsp;·&nbsp; `scikit-learn` &nbsp;·&nbsp; `folium` &nbsp;·&nbsp; `seaborn` &nbsp;·&nbsp; `matplotlib` &nbsp;·&nbsp; `scipy` &nbsp;·&nbsp; `rapidfuzz`
