# Real-Time Monitoring Dashboard for Red-Light Camera Violations in Ottawa

**Project Owner:** Olamilekan Elegbede  
**Institution:** School of Advanced Technology, Algonquin College  
**Tools:** Microsoft Excel · Power Query · Tableau

---

## Overview

Red-light camera violations are a significant traffic safety concern in Ottawa. While the City of Ottawa publishes violation data publicly through Open Ottawa, the data is spread across multiple disconnected datasets, making it difficult for city officials to identify high-risk intersections or detect meaningful trends.

This project consolidates 13 annual datasets (2015–2026) covering 86 red-light cameras into a single, unified data source and delivers a four-page interactive Tableau dashboard that turns raw open data into actionable traffic safety insights.

---

## Data Sources

- **Red-Light Camera Violations** (2015–2026) — monthly violation counts per camera, 21 columns, up to 86 rows per year
- **Red-Light Camera Locations** — intersection names, coordinates, installation year, and camera facing direction
- **Traffic Camera Locations** — live image feed URLs mapped to intersections for real-time visual context

---

## Data Preparation

All cleaning and transformation was performed in Microsoft Excel and Power Query before loading into Tableau:

- Consolidated 12 annual violation files into a single workbook
- Built a composite key (installation year + rounded coordinates + camera direction) to uniquely identify cameras across files using `XLOOKUP`
- Unpivoted wide monthly columns into a long-format table (831 rows → 9,972 rows) with one record per camera per month
- Performed an inner join in Tableau between the violation union and the camera reference table
- Mapped live traffic camera feed URLs to intersections where available

---

## Dashboard Pages

### 1. Overview Dashboard
![Overview Dashboard](https://github.com/user-attachments/assets/overview-dashboard)

The entry point of the dashboard, providing a consolidated summary for program-level decision-making. Features four KPI cards (total violations, active cameras, total fines issued, top intersection), a monthly bar chart with a reference line, a radar chart of violations by camera direction, a geographic bubble map, and a ranked bar chart of intersections by violation share.

> As of January 2026: **430,800+ total violations** recorded across the network, **$140M CAD** in fines issued, with **King Edward Avenue & St. Patrick Street** holding the highest cumulative violations (~$11.6M CAD).

---

### 2. Seasonal Trends Dashboard
![Seasonal Trends Dashboard](https://github.com/user-attachments/assets/seasonal-trends-dashboard)

Focuses on how violations fluctuate across months, seasons, and years. Includes a year-over-year area chart, monthly violation trends, a yearly violations per season chart, and a stacked bar chart of camera direction violations by season. Violations decrease in winter and peak in summer, suggesting weather plays a measurable role in driver behaviour. **2023 recorded the highest single-year violations (~56,000).**

---

### 3. Camera Insights Dashboard
![Camera Insights Dashboard](https://github.com/user-attachments/assets/camera-insights-dashboard)

An interactive, location-level deep-dive. Clicking any camera on the map updates all elements: KPI cards (intersection, install year, direction, total violations, fines), a monthly bar chart, a scatter plot of violations by camera age, a risk classification indicator (1–Critical to 5–Minimal), a box plot distribution, and a live camera feed where available. Risk tiers are derived from interquartile thresholds applied consistently across all 86 cameras.

---

### 4. Violations Dashboard
![Violations Dashboard](https://github.com/user-attachments/assets/violations-dashboard)

A fully filterable row-level record of all infractions from 2015–2026. A color-coded violations table (red = Critical Risk → green = Low Risk) surfaces problem locations at a glance. Two KPI cards identify the all-time peak enforcement period and the highest-cumulative intersection. A box plot shows the statistical distribution of average monthly violations across the entire camera fleet.

> **August 2021** recorded the highest number of violations across all periods in the dataset.

---

## Key Findings

| Metric | Value |
|---|---|
| Total violations (2015–Jan 2026) | ~430,800 |
| Total fines issued | ~$140M CAD |
| Active cameras (Jan 2026) | 86 |
| Highest-violation intersection | King Edward Ave & St. Patrick St |
| Direction with most violations | Northbound (~126,000) |
| Peak violation month | August 2021 |
| Peak violation year | 2023 (~56,000) |

---

## Future Work

- **Live data integration** — connect directly to a streaming API for near real-time monitoring
- **Predictive modelling** — incorporate time-series forecasting or regression to anticipate high-risk periods
- **Additional datasets** — integrate the Traffic Collision dataset from Open Ottawa to explore causal relationships between collisions and violation hotspots
