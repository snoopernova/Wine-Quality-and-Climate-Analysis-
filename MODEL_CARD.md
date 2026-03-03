# Model Card — Wine Quality & Climate Vulnerability Predictor

---

## 1. What This Model Is For / Not For

### What it's for

- **Predicting macro quality trends:** Estimating the average professional wine rating (Wine Enthusiast critic scores) for a specific region and growing season based entirely on climate conditions.
- **Vulnerability sizing:** Projecting which regions face the greatest quality risk under standardised warming scenarios (+1°C to +3°C).

### What it's NOT for

- **Predicting individual wine scores:** The model only looks at regional climate; it cannot account for a specific winemaker's skill, prestige pricing, or subjective critic preferences for individual bottles.
- **Predicting specific future vintages:** It cannot tell you what the 2028 vintage will score, as that requires precise daily weather forecasts, not long-term climate averages.
- **Regions outside the curated 16:** Extrapolating to unrepresented regions (like Burgundy or South Africa) is not recommended as the model has not learned their specific baseline climates.

---

## 2. Data Provenance & Constraints

**Wine Ratings:** Sourced from ~130k Wine Enthusiast reviews scraped around 2017.
- Publisher: Wine Enthusiast magazine (professional critic panel)
- Available via [Kaggle — Wine Reviews](https://www.kaggle.com/datasets/zynicide/wine-reviews)

**Climate Data:** Historical daily weather metrics (temperatures, precipitation) sourced via the [Open-Meteo Historical API](https://open-meteo.com/) and aggregated specifically to the biological growing season of each region:
- Northern Hemisphere: April–October (vintage year)
- Southern Hemisphere: October (prior year)–April (vintage year)

**Constraints:**

| Constraint | Impact |
|---|---|
| Dataset ends after 2016 | Cannot capture post-2016 extreme heat events (e.g. 2022 European heatwave) |
| Barolo and other age-restricted regions | May lack ratings for 2014–2016 due to strict aging laws |
| Warming simulation holds rainfall constant | Conservative estimate — warming typically exacerbates drought, so real impacts may be worse |
| Southern Hemisphere under-represented | 6 of 16 regions; key producers (Marlborough, Stellenbosch) absent from dataset |

---

## 3. Evaluation Summary & Caveats

**Model:** Random Forest Regressor trained on region-year aggregate climate features.

**Performance:**

| Metric | Value | Notes |
|---|---|---|
| In-sample R² | High | Fits historical data well |
| LOYO CV R² (climate + region) | ~0.50–0.65 | Leave-One-Year-Out; truly out-of-sample vintages |
| LOYO CV R² (climate only) | ~0.25–0.40 | Without region fixed effects |
| CV RMSE | ~1.0–1.3 pts | On an 80–100 point scale |

**The "Wildcard" Factor:** The gap between in-sample and out-of-sample performance proves that climate is an indicator of quality, but it leaves significant variance unexplained. Human intervention, changing consumer tastes, and winemaking style can heavily mask the raw climate signal — especially in regions with less historical data.

**Adaptation Caveat:** The model's warming projections assume winemakers make no adaptive changes. In reality, vulnerable regions are likely to respond through vineyard elevation shifts, variety substitution, harvest timing adjustments, and irrigation investment — all of which would partially offset the projected declines.
