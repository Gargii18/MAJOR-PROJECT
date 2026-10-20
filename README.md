# 🌿 Biodiversity Loss Analysis & IUCN Status Prediction (Streamlit prototype)

Web-app prototype that integrates the whole project - the report *"Biodiversity Loss Analysis and Species Extinction
Prediction System"* and its analysis script - into one interactive tool.

| Page | What it does |
|---|---|
| 🏠 Overview | KPIs and trends from the IUCN Red List history |
| 📈 1 · Trends & Forecast | Cubic/linear/quadratic/exponential regression, CAGR, prediction intervals, hold-out back-test, editable data (add new years) |
| 📊 2 · Distribution Fitting | Poisson / Geometric / Negative Binomial MLE fits, AIC/BIC, χ² GOF, top countries |
| 🔗 3 · Markov Chain | Transition counts & probabilities, Sankey flow, extinction-pathway analysis, scenario projection |
| 🌳 4 · Status Predictor | Random Forest predicting next IUCN status, interactive prediction, performance, 5-algorithm benchmark, batch CSV scoring, model download |
| 🗂️ Data & Pipeline / ℹ️ About | Data quality, expected formats, method notes |

## Run locally
```bash
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

## Use your real data
Copy `Distribution_Data.csv` and `Markov_Random_Forest (1).xlsx` into `data/` (or upload them in the sidebar).
Without them the app uses **synthetic demo data** and says so on every affected page.

## Deploy on Streamlit Community Cloud (free)
1. Push this folder to a GitHub repository (include `data/` files only if you are happy for them to be public).
2. Go to <https://share.streamlit.io> → **New app** → pick the repo, branch `main`, main file path `app.py`.
3. Click **Deploy**. Dependencies are installed from `requirements.txt` automatically.

## Project structure
```
app.py                  Streamlit UI (all pages)
core/
  constants.py          statuses, colours, IUCN Red List history 1996-2025
  data_loader.py        robust CSV/XLSX parsers + data-quality report
  demo_data.py          synthetic demo generators (never real findings)
  regression.py         Module 1
  distribution.py       Module 2
  markov.py             Module 3
  classifier.py         Module 4
data/                   put real datasets here
tests/test_smoke.py     renders every page headlessly (pytest -q)
.streamlit/config.toml  theme
```

## Important interpretation notes
* The Random Forest predicts a **Red List category for the next assessment**, not extinction itself, and only uses two
  features (2021 status + taxonomic group). On changed-species data it performs similar to a lookup table.
* The status-change file contains **only species whose category changed**, so Markov probabilities and RF outputs are
  conditional on a change.
* Regression totals reflect assessment effort as well as true biodiversity loss; forecasts are illustrative.
