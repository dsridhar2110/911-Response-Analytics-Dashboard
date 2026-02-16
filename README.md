# Comprehensive Analysis of PSAP Distribution, Distress Call Trends & Forecasting

A **data exploration, visualisation and forecasting** project focused on U.S. Public Safety Answering Points (PSAPs), distress call volumes by medium and geography, incident patterns, and response-time predictions. Built as an interactive **R Shiny** dashboard for recruiters and stakeholders to explore findings at a glance.

---

## Project overview

- **Domain:** Emergency services & 911 / distress call infrastructure.
- **Deliverable:** Interactive Shiny app with maps, time-series charts, distress-type analysis, and forecasting.
- **Outcome:** Clear view of PSAP coverage, call trends (2017–2024), incident/resolution patterns, and predicted response times for the next quarter and by hour of day.

---

## Objectives

- **Map PSAP distribution** — Primary and Secondary PSAP counts by state (hexbin choropleth).
- **Analyse call trends** — Yearly volumes by Wireline, Wireless, VoIP, and Text (2017–2024) with animated line charts.
- **Explore call geography** — Choropleth of call share by type (Wireline / Wireless / VoIP / Text) across U.S. states.
- **Distress call analysis** — Incident frequency and resolution metrics by type, day of week, hour, and date range (bar, heatmap, line, bubble).
- **Forecast response times** — Daily average (SARIMA, 60-day horizon) and hourly estimates (regression by hour) for planning.

---

## Dataset summary

| Source | Description |
|--------|-------------|
| **PSAP data** (`state_responses_columns.xlsx`) | Primary and Secondary PSAP counts per state. |
| **Spatial** (`us_states_hexgrid.geojson`) | U.S. state boundaries (hex grid) for mapping. |
| **Call volumes (yearly)** | In-app: Wireline, Wireless, VoIP, Text totals (2017–2024). |
| **Call types by state** (`call_types_state_wise.csv`) | State-level call counts/percentages by Wireline, Wireless, VoIP, Texts. |
| **Distress incidents** (`combined.csv`) | Incident counts, response times (`Resp_TS`), day, hour, date, distress type (e.g. Assault, Crime_Main). |
| **Daily averages** (`Daily_Averages_combined.csv`) | Date and average response time for time-series modelling. |
| **Hourly averages** (`Hourly_Averages_combined.csv`) | Hour and response time for hourly prediction model. |

---

## Key visualisations

- **Hexbin map** — Primary vs Secondary PSAP density by state (toggle, tooltips, annotations).
- **Line chart** — Call volumes by type (Wireline, Wireless, VoIP, Text) over years with year slider and animation.
- **Choropleth** — Call distribution by selected type across U.S. states.
- **Bar chart** — Incident overview or resolution analysis by distress (main) type.
- **Heatmap** — Incident frequency or resolution efficiency by weekday × distress type.
- **Line chart (hourly)** — Hourly incident or resolution trends by distress type.
- **Bubble chart** — Monthly trends by distress type (size = scaled count).
- **Time-series plot** — Actual vs SARIMA-forecasted average response time (with month slider and animation).
- **Actual vs predicted (hourly)** — Actual mode vs predicted mean response time by hour (animation).

---

## Modelling / forecasting approach

- **Daily average response time**
  - Log transform of average response; tsibble with `fill_gaps()`.
  - **STL** decomposition (seasonal window periodic).
  - **SARIMA** on seasonally adjusted series: `pdq(1,1,1) + PDQ(1,1,0)`.
  - **Forecast:** 60-day ahead point forecast.
- **Hourly response time**
  - One-hot encoding of hour; Z-score scaling of response time.
  - **Linear regression** (response_time_scaled ~ hour dummies).
  - Predictions for hours with missing response time; inverse scaling to original units.
  - Comparison: actual (mode by hour) vs predicted (mean by hour).

---

## Tech stack

**Language & runtime:** R  

**Libraries & tools:**

- **App & UI:** `shiny`, `shinydashboard`, `shinyWidgets`, `htmlwidgets`
- **Visualisation:** `ggplot2`, `plotly`, `highcharter`, `viridis`, `RColorBrewer`, `scales`, `hrbrthemes`
- **Data & time series:** `dplyr`, `tidyverse`, `lubridate`, `tsibble`, `fable`, `feasts`
- **Spatial:** `sf`, `maps`
- **Data I/O:** `readxl`

---

## How to run locally

1. **Prerequisites:** [R](https://www.r-project.org/) (4.x recommended) and [RStudio](https://posit.co/download/rstudio-desktop/) (optional).
2. **Clone the repo** and set the working directory to the project root (where `main.R` and the data files live).
3. **Required files in project root:**
   - `main.R`
   - `state_responses_columns.xlsx`
   - `us_states_hexgrid.geojson`
   - `call_types_state_wise.csv`
   - `combined.csv`
   - `Daily_Averages_combined.csv`
   - `Hourly_Averages_combined.csv`
4. **Install dependencies:** The script installs packages if missing (`install.packages(...)`). Run once:
   ```r
   source("main.R")
   ```
   Or open `main.R` in RStudio and run the script; the Shiny app will launch.
5. **Launch app only:** From R console:
   ```r
   shiny::runApp(".", launch.browser = TRUE)
   ```
   (Ensure your working directory is the project root so relative paths to CSVs, Excel, and GeoJSON resolve correctly.)

---

## Project report (PDF)

The **full project report** (methodology, design choices, and detailed analysis) is available in the **`docs`** folder:

- **[Project Report (PDF)](docs/Project_Report.pdf)** — Detailed structure, data pipeline, visualisation design, and modelling notes.

For a quick overview, use this README; for depth and narrative, refer to the report in **`docs`**.

---

## Repository structure (high level)

```
├── main.R                    # Shiny app + data load + models + UI/server
├── combined.csv               # Distress incidents & response times
├── Daily_Averages_combined.csv
├── Hourly_Averages_combined.csv
├── call_types_state_wise.csv
├── state_responses_columns.xlsx
├── us_states_hexgrid.geojson
├── docs/
│   └── Project_Report.pdf     # Full report (see above)
└── README.md
```

---

*Data Exploration & Visualisation project — structured for clarity and recruiter-friendly navigation.*
