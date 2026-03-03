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
- The dataset physically ends after 2016. Because of strict aging laws (e.g., Barolo), some regions lack ratings for 2014-2016 entirely and cannot be used for recent baselines.
- The warming simulation assumes all other variables (like rainfall and water stress) remain constant, which is a conservative estimate given warming typically exacerbates drought. 

---

## 3. Evaluation Summary & Caveats

**Model:** Random Forest Regressor trained on region-year aggregate climate features.

- **Performance:** The Random Forest Regressor fits the historical data with in-sample accuracy. However, rigorous out-of-sample validation (Leave-One-Year-Out Cross-Validation) yields lower R² scores.
- **The "Wildcard" Factor:** This performance gap proves that climate is an indicator of quality, but it leaves significant variance unexplained. Human intervention, changing consumer tastes, and winemaking style can heavily mask the raw climate data, especially in "wildcard" regions with less data.
- **Adaptation Caveat:** The model's future projections assume winemakers make no changes. In reality, vulnerable regions will likely adapt to adjust to growing concerns regarding climate change and global warming.

