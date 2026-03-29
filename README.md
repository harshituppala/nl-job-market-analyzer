# NL Job Market Analyzer

**Live Dashboard → [nl-job-market.onrender.com](https://nl-job-market.onrender.com)**

An interactive labour market intelligence dashboard for Newfoundland & Labrador, pulling real employment data from Statistics Canada's public API and surfacing trends, anomalies, and sector comparisons in a clean, professional interface.

---

## What It Does

- **Pulls live data** from Statistics Canada Table 14-10-0090-01 (Employment by industry, monthly, seasonally adjusted)
- **Filters to NL** — all data is specific to Newfoundland & Labrador
- **Interactive sector selector** — drill into any of 15 industries
- **Anomaly detection engine** — flags months where employment shifted more than 2 standard deviations from the rolling mean, surfacing unusual hiring or layoff events
- **KPI summary cards** — current employment, month-over-month change, year-over-year change, anomaly count
- **Sector comparison bar chart** — see all industries ranked by employment in the latest available month
- **Month-over-month change chart** — colour-coded bar chart of monthly deltas
- **Anomaly log** — timestamped table of flagged labour market shifts

---

## Tech Stack

| Layer | Technology |
|---|---|
| Dashboard framework | [Plotly Dash](https://dash.plotly.com/) |
| Data processing | Pandas |
| Visualization | Plotly Graph Objects |
| Data source | [Statistics Canada Open API](https://www.statcan.gc.ca/en/developers) |
| Deployment | Render (via `render.yaml`) |
| Server | Gunicorn |

---

## Architecture

```
Statistics Canada API (Table 14-10-0090-01)
        │
        ▼
  fetch_statscan_data()          ← HTTP GET with graceful fallback
        │
        ▼
  Pandas DataFrame                ← Normalized, date-parsed, NL-filtered
        │
        ├──► compute_anomalies()  ← 2-std-dev rolling anomaly detection
        │
        ▼
  Dash Callbacks                  ← Reactive UI updates on filter change
        │
        ▼
  Plotly Figures                  ← Trend line, MoM bar, sector comparison
```

---

## Running Locally

```bash
git clone https://github.com/harshituppala/nl-job-market-analyzer
cd nl-job-market-analyzer

pip install -r requirements.txt

python app.py
# → Open http://localhost:8050
```

---

## Deploying to Render (Free Tier)

1. Push to GitHub
2. Go to [render.com](https://render.com) → New Web Service
3. Connect your GitHub repo
4. Render auto-detects `render.yaml` — click **Deploy**
5. Live in ~3 minutes at `https://your-app.onrender.com`

---

## Data Source

Statistics Canada, Table 14-10-0090-01:
*Employment by industry, monthly, seasonally adjusted, last 5 months (x 1,000)*

> When the StatsCan API is unavailable, the app falls back to a realistic synthetic dataset derived from published NL regional employment figures (2020–2024), maintaining the same schema and approximate values.

---

## Project Context

Built as part of my CICS internship application portfolio and as supporting infrastructure research for **LocalTask/Aider+**, a hyperlocal task marketplace targeting NL communities. Understanding regional labour market trends — particularly in sectors like construction, healthcare, and professional services — directly informs the worker-side supply strategy for the platform.

---

## Author

**Harshit Kumar Uppala**
B.Sc. Computer Science, Memorial University of Newfoundland
[github.com/harshituppala](https://github.com/harshituppala) · hkuppala@mun.ca
