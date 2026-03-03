# Predictive Analytics: Wine Quality & Climate Vulnerability

A predictive analytics project exploring how growing-season climate (temperature, rainfall, and derived indices) affects wine quality ratings across **16 premium wine regions spanning both hemispheres** (2000–2016). This project uses a tuned Random Forest Regressor to predict vintage ratings and simulate the structural impact of future climate warming scenarios (+1°C / +2°C / +3°C).

---

## Project Structure

```
Final Submission/
├── Wine_Quality_Climate_FINAL.ipynb          # Main analysis notebook
├── winemag-data-130k-v2.csv                  # Source dataset (Wine Enthusiast reviews)
├── climate_data_cache.csv                    # Cached Open-Meteo API data (pre-fetched)
├── requirements.txt                          # Python package dependencies
├── environment.yml                           # Conda environment spec
├── Evidence_of_Workflow.md                   # Agent tooling log & verification notes
├── Predictive Analytics Indv Coursework.pdf  # Coursework brief
└── README.md                                 # This file
```

---

## Dataset & Provenance

**1. Wine Enthusiast Reviews** – `winemag-data-130k-v2.csv`
- ~130,000 professional wine reviews with critic ratings (80-100 points), regional origin, vintage year, and price.
- Download from [Kaggle](https://www.kaggle.com/datasets/zynicide/wine-reviews) and place it in the project root folder.

**2. Climate Data** – `climate_data_cache.csv`
- Historical daily weather metrics (temperatures, precipitation) fetched automatically via the [Open-Meteo Historical API](https://open-meteo.com/). 
- This data is aggregated by the biological growing season of each region (Apr-Oct for Northern Hemisphere, Oct-Apr for Southern) to create viticulturally relevant indices like Growing Degree Days (GDD) and Water Stress.

---

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
```

### 2. Create and activate a virtual environment
```bash
python3 -m venv my_env
source my_env/bin/activate        # macOS / Linux
# my_env\Scripts\activate         # Windows
```

### 3. Install dependencies
```bash
pip install pandas numpy requests scikit-learn plotly jupyter
```

### 4. Add the dataset
Download `winemag-data-130k-v2.csv` from Kaggle and place it in the same folder as the notebook.

### 5. Launch the notebook
```bash
jupyter notebook Wine_Quality_Climate_V10.ipynb
```
Run all cells **top to bottom**. The notebook will automatically use the `climate_data_cache.csv` if present to speed up execution and ensure reproducibility.

---

## Methodological Highlights

| Feature | Implementation |
|---|---|
| **Temporal Alignment** | Grapes in the Southern Hemisphere (e.g., Mendoza, Margaret River) harvested in 2015 began growing in October 2014. The data pipeline strictly handles this 6-month offset to ensure accurate biological windows. |
| **Feature Engineering** | Raw daily weather is converted into established viticulture metrics: **GDD** (accumulated heat) and **Water Stress** (precipitation vs evapotranspiration). |
| **Cross-Validation** | Standard K-Fold splits risk temporal data leakage. This project strictly relies on **Leave-One-Year-Out (LOYO) Cross-Validation** to force the model to predict entirely out-of-sample vintages. |

---

## Analysis Steps

1. **Obtain and Frame:** Define predictive questions regarding climate's impact on wine quality and regional vulnerability.
2. **Exploratory Data Analysis (EDA):** Analyze distributions, missingness, and survivorship bias.
3. **Data Preparation:** Group 130k+ reviews into a regional panel dataset (16 regions × 17 years) and merge with engineered climate metrics.
4. **Model Selection:** Establish baselines (DummyRegressor, Ridge) and shortlist a Random Forest Regressor for its ability to capture non-linear tipping points (the "Goldilocks" heat effect).
5. **Fine-Tuning & Evaluation:** Evaluate strict LOYO out-of-sample performance and conduct error analysis via Region-specific MAE.
6. **Final Solution:** Map historic climate vulnerability and run a +1/+2/+3°C Global Warming Simulation.

---

## Key Findings

- **Predictability Varies by Terroir:** The Random Forest achieved in-sample accuracy, but out-of-sample LOYO MAE proved that climate alone does not perfectly dictate wine scores. Established regions (like Columbia Valley) are highly predictable by weather (MAE < 0.40), while "wildcard" regions (like Paso Robles) see their climate signal heavily masked by winemaking intervention (MAE > 1.0).
- **Vulnerability Tipping Points:** The warming simulation explicitly demonstrates that historically warm regions (like parts of Australia or California) are sitting at their maximum heat threshold; adding +2°C pushes them off a quality cliff where high-end production is no longer structurally viable without adaptation.
- **The "Low Risk" Winners:** Conversely, historically cooler regions (like the Finger Lakes) see modest gains under warming models as they finally receive enough accumulated heat to ripen grapes consistently.

---

## License
This project is for academic/personal use. Wine review data © Wine Enthusiast. Climate data © Open-Meteo.
