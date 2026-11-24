# India AQI Explorer (2022–2025)

An interactive data-analysis project exploring hourly air-quality and weather patterns across 29 Indian cities. The project uses the supplied CSV dataset and a standalone HTML dashboard to compare pollution levels by city, month, season, hour of day, pollutant, and selected period flags.

> **Important metric note:** The dashboard uses the dataset's `US_AQI` column as its primary AQI measure. The file also contains `EU_AQI` and `PM25_Category_India`; these are separate fields and should not be treated as interchangeable. `US_AQI` is not the same thing as India's official CPCB AQI.

## Project files

```text
india-aqi-explorer/
├── README.md
├── india_aqi_dashboard.html
└── INDIA_AQI_COMPLETE_20251126(1).csv
```

- **`india_aqi_dashboard.html`** — standalone interactive dashboard. It contains the Plotly library and the aggregated data needed by the dashboard.
- **`INDIA_AQI_COMPLETE_20251126(1).csv`** — source file used for the analysis.

The dashboard can be opened without running a Python server. Keep the CSV alongside the README if you want to retain the source dataset with the project; the HTML dashboard itself does not need to load the CSV at runtime.

## Dataset overview

| Item | Details |
|---|---:|
| Records | 842,160 hourly rows |
| Columns | 59 |
| Cities | 29 |
| States | 29 |
| Date range | 5 August 2022 – 26 November 2025 |
| Records per city | 29,040 |
| Average `US_AQI` | 96.80 |
| Average `PM2_5_ugm3` | 34.74 µg/m³ |
| Average `PM10_ugm3` | 56.26 µg/m³ |

The first and last calendar years are partial periods: 2022 starts on 5 August, and 2025 ends on 26 November. Compare full-year results with this limitation in mind.

### Main field groups

- **Location and time:** `City`, `State`, `Latitude`, `Longitude`, `Year`, `Month`, `Day`, `Hour`, `Day_Name`, `Quarter`, `Season`, `Time_of_Day`, and calendar flags.
- **Weather:** temperature, humidity, dew point, wind speed/direction/gusts, precipitation, rain, pressure, solar radiation, and cloud-cover fields.
- **Pollutants:** `PM2_5_ugm3`, `PM10_ugm3`, `CO_ugm3`, `NO2_ugm3`, `SO2_ugm3`, `O3_ugm3`, `Dust_ugm3`, and `NH3_ugm3`.
- **Air-quality indicators:** `US_AQI`, pollutant-specific US AQI fields, `EU_AQI`, pollutant-specific EU AQI fields, `AQI_Category`, and `PM25_Category_India`.
- **Period flags:** `Festival_Period` and `Crop_Burning_Season`.

## Dashboard features

The dashboard provides **City** and **Year** filters. Changing either filter updates the KPI cards and the relevant charts.

1. **City leaderboard** — compares each city's raw mean `US_AQI` with its mean after excluding records above 500.
2. **Monthly pollution pattern** — shows monthly average `US_AQI` and PM2.5.
3. **Seasonal comparison** — compares average AQI and PM2.5 across seasons.
4. **Hourly profile** — shows how AQI and PM2.5 vary by hour of day.
5. **AQI category mix** — displays the distribution of the dataset's `AQI_Category` labels.
6. **Flagged-period comparison** — compares averages for festival, crop-burning, wind-stagnation, and rain flags.
7. **Pollutant and weather relationships** — displays Pearson correlations with `US_AQI`.
8. **Largest hourly AQI records** — lists high-AQI observations for review.
9. **Data quality and coverage** — summarizes missing values, duplicate rows, and extreme readings.

## Findings from this dataset

These are descriptive results calculated from the supplied file, not claims about current air quality.

- **City comparison:** Gurugram has the highest raw mean `US_AQI` (222.52), followed by Delhi (159.84). Aizawl has the lowest raw mean (59.56).
- **Extreme-value sensitivity:** Gurugram's mean falls to 160.59 when records above 500 are excluded; its median is 159. The dataset contains 2,371 records above 500, including 2,081 from Gurugram and 290 from Delhi. These records are flagged for investigation, not automatically declared invalid.
- **Seasonality:** Winter has the highest average `US_AQI` (118.27), followed by summer (109.25), post-monsoon (102.14), and monsoon (71.90).
- **Pollutant relationships:** Pearson correlation with `US_AQI` is strongest for PM10 (r = 0.830), followed by PM2.5 (r = 0.668) and dust (r = 0.663).
- **Rain comparison:** Records flagged as rainy have a lower average `US_AQI` (66.73) than records not flagged as rainy (104.54). This is an association in this dataset; it does not prove that rain alone caused the difference.
- **Flagged periods:** Festival-period records average 104.69 versus 95.87 outside the festival flag. Crop-burning-season records average 102.14 versus 95.49 outside that flag. These comparisons may also reflect season, location, and other factors.

## Data quality notes

| Check | Result |
|---|---:|
| Missing `NH3_ugm3` values | 842,160 (entire column) |
| Missing `US_AQI` values | 145 |
| Missing `AQI_Category` values | 2,516 |
| Exact duplicate rows | 0 |
| `US_AQI` values above 500 | 2,371 |
| `PM10_ugm3` values above 1,000 | 1,102 |
| Maximum `US_AQI` | 2,742 |
| Maximum `PM10_ugm3` | 3,263.4 µg/m³ |

The extreme values materially affect city averages, especially Gurugram. The dashboard therefore shows both the raw mean and a comparison mean excluding `US_AQI` values above 500. This is a sensitivity check only; inspect the underlying records and source methodology before deciding whether any values should be removed or corrected.

## How to open the dashboard

1. Download or clone the project files.
2. Keep `india_aqi_dashboard.html` in a local folder.
3. Double-click the HTML file, or open it directly in Chrome, Edge, or Firefox.
4. Use the **City** and **Year** dropdowns to explore the dashboard.

No Python environment, database, or local web server is required to view the supplied HTML dashboard.

## Suggested analysis workflow

For further work with the CSV, a useful workflow is:

1. Inspect data types, date coverage, missing values, and duplicates.
2. Validate extreme readings, especially the high Gurugram values.
3. Compare cities using mean, median, percentiles, and valid-record counts.
4. Examine monthly, seasonal, and hourly trends.
5. Explore pollutant and weather correlations, while avoiding causal claims from correlation alone.
6. Document any filtering or cleaning decisions before building predictive models.

## Limitations

- The uploaded file does not identify its upstream data provider, collection method, or validation process. Confirm provenance before presenting the figures as official measurements.
- This is a historical dataset through 26 November 2025, not a live AQI feed.
- The primary dashboard measure is `US_AQI`; do not label it as official Indian/CPCB AQI.
- Correlation and flagged-period comparisons describe associations; they do not establish causation.
- Missing `NH3_ugm3` values mean that this field cannot support analysis in the supplied file.
- The 2022 and 2025 periods are incomplete calendar years.

## Tools and technologies

- **CSV** for the source data
- **HTML, CSS, and JavaScript** for the dashboard interface
- **Plotly.js 3.3.1** for interactive charts (bundled in the HTML file)

---

*Prepared from the supplied `INDIA_AQI_COMPLETE_20251126(1).csv` file and the accompanying `india_aqi_dashboard.html` dashboard.*
