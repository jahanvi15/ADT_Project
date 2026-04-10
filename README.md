# Consumer Spending & Economic Stress Tracker

**DSCI-D532 | Final Project | Indiana University | Spring 2026**  
Team: Duhita Pradhan · Jahanvi Joshi · Sudhiksha Mhatre

---

## Live Dashboard

Open `index.html` in any modern browser — no server, no dependencies, no install required.

---

## Project Overview

An end-to-end data management and visualization project built on U.S. Bureau of Labor Statistics Consumer Price Index (CPI) data spanning 1935–2017, combined with FRED economic indicators (1967–2017).

### What it shows

- **CPI trends** across 5 spending categories (Food & Beverage, Housing, Medical Care, Transportation, Recreation)
- **Recession impact** — how prices behave during vs. normal economic periods
- **Economic indicators** — unemployment rate & GDP growth correlated with CPI
- **Year-over-year inflation** — per-category annual % change with top inflation years
- **Great Recession zoom** — monthly CPI 2006–2011 with recession band overlay

---

## Repository Structure

```
consumer-spending-tracker/
├── index.html              # Main dashboard (single-file, no build step)
├── data.js                 # All chart data, auto-generated from SQLite
├── consumer_spending.db    # SQLite database (star schema)
├── README.md               # This file
└── pipeline/
    └── build_db.py         # Python/Colab script to rebuild the database
```

---

## Database Schema (Star Schema)

```
dim_date ──────┐
dim_category ──┼──► fact_cpi
dim_area ──────┘       │
dim_series ────────────┘

dim_date ──► fact_economic_indicators
```

| Table | Type | Rows |
|-------|------|------|
| dim_date | Dimension | 893 |
| dim_category | Dimension | 5 |
| dim_area | Dimension | 45 |
| dim_series | Dimension | 5,373 |
| fact_cpi | Fact | 3,289 |
| fact_economic_indicators | Fact | 51 |

---

## Data Sources

| Source | Description | License |
|--------|-------------|---------|
| [BLS CPI via Kaggle](https://www.kaggle.com/datasets/bls/consumer-price-index) | Consumer Price Index CSV files | CC0 Public Domain |
| [FRED Economic Data](https://fred.stlouisfed.org) | Unemployment rate & GDP growth 1967–2017 | Public |
| [US Recession Dataset](https://www.kaggle.com/datasets/shubhaanshkumar/us-recession-dataset) | NBER recession periods | Public |

---

## Tech Stack

**Backend / Pipeline**
- Python 3 + pandas
- SQLite (via `sqlite3` module)
- Google Colab (zero-setup reproducibility)

**Frontend Dashboard**
- HTML5 / CSS3 / Vanilla JavaScript
- [Chart.js 4.4](https://www.chartjs.org/) (via CDN)
- Google Fonts: Syne + DM Mono
- No build tools, no npm, no frameworks

---

## Rebuilding the Database

If you want to regenerate `consumer_spending.db` from raw BLS CSV files:

1. Open [Google Colab](https://colab.research.google.com)
2. Paste the full pipeline script from `pipeline/build_db.py`
3. Upload the 8 BLS CSV files when prompted:
   - `cu_data_11_USFoodBeverage.csv`
   - `cu_data_12_USHousing.csv`
   - `cu_data_14_USTransportation.csv`
   - `cu_data_15_USMedical.csv`
   - `cu_data_16_USRecreation.csv`
   - `cu_series.csv`, `cu_item.csv`, `cu_area.csv`
4. Run all cells → download `consumer_spending.db`
5. Regenerate `data.js` by running:
   ```bash
   python3 pipeline/build_db.py --export-js
   ```

---

## Deploying to GitHub Pages

1. Fork or push this repo to GitHub
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/ (root)`
4. Your dashboard will be live at `https://<username>.github.io/consumer-spending-tracker/`

---

## Key Findings

- **Medical Care** shows the highest CPI during recession periods (336.6 avg), more than double the normal period average (135.9) — driven by long-run healthcare cost inflation rather than recession effects specifically
- **Transportation** shows the largest gap between recession/normal CPI, reflecting oil-price volatility during recession years
- **Recreation** (the only Discretionary category) shows minimal difference between recession and normal periods (110.8 vs 107.0)
- Peak unemployment (9.7% in 1982) correlated with peak food price inflation in the late 1970s–early 1980s period

---

## AI Assistance Disclosure

Portions of the pipeline code and query structuring were developed with assistance from Claude (Anthropic, claude-sonnet-4-6).
