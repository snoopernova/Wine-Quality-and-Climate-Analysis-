# Evidence of Workflow: Agentic Methodology in Predictive Analytics

This document shows how the project was organised and how it progressed through stages using Antigravity with Calude Sonnet 4.6

---

## 1. Goal Setting & Problem Framing

**Strategy:** I set the core objective to predict average wine ratings purely from growing-season climate variables and, critically, to identify which historic wine regions are most vulnerable to the climate crisis. I selected the two original data sources (Wine Enthusiast Kaggle dataset and Open-Meteo).

**Agent Execution:** The agent was prompted to generate an initial project scaffold and outline the statistical limitations of the chosen datasets. 

**Verification:** I verified that the agent correctly identified the performance ceiling (R² ≈ 0.50-0.60) inherent in omitting human winemaking variables, establishing realistic expectations for the subsequent models.

---

## 2. Exploratory Data Analysis & Quality Control

**Human Strategy:** I directed the agent to perform a visual EDA focused specifically on identifying spatial and temporal data leakage risks before modeling began.

**Agent Execution:** The agent wrote Python code to plot univariate distributions and correlation heatmaps across the 16 curated regions.

**Verification & Course Correction:** During this phase, I noticed a critical data quality issue in the agent's output. The agent was attempting to calculate baseline averages up to 2016 for all regions; however, looking at the EDA, I realized the Italian Barolo region had no data past 2013. I instructed the agent to implement logic that handled this missingness rather than blindly dropping the region or imputing false data.

---

## 3. Data Preparation & Feature Engineering

**Strategy:** Raw daily temperature and rainfall are not biologically meaningful to a grapevine. I instructed the agent to engineer two crucial viticultural indices: **Growing Degree Days (GDD)** and a **Water Stress Index**. 

**Agent Execution:** The agent wrote the Pandas aggregations required to sum daily weather data into these two indices. Crucially, the agent had to write specific functions to offset the biological growing season by 6 months depending on whether a region was in the Northern (Apr-Oct) or Southern (Oct-Apr) hemisphere.

**Verification:** I manually verified the alignment of the `year` label against harvest months in extreme examples (e.g., Mendoza vs. Napa Valley) to ensure the agent's datetime functions had not introduced temporal mismatches into the panel dataset.

---

## 4. Modeling & Iteration

**Strategy:** I required the agent to establish a DummyRegressor baseline (predicting the global mean) before attempting any complex machine learning. 

**Agent Execution:** The agent successfully established the baseline and then immediately proposed and implemented a Gradient Boosting Machine to maximize absolute predictive accuracy on the training data.

**Verification & Course Correction:** I intervened at this stage. Recognizing that our ultimate goal was evaluating climate vulnerability (which requires high model interpretability to extract feature importances and run clear warming simulations), I directed the agent to **abandon the GBM and rewrite the pipeline using a Random Forest Regressor.** While slightly less powerful on the training set, the Random Forest's structure was structurally sounder for the final analysis. The agent updated its codebase accordingly.

---

## 5. Evaluation & Rigor

**Strategy:** To prevent the model from overfitting to concurrent global weather patterns, I mandated a strict evaluation strategy.

**Agent Execution:** The agent wrote the initial cross-validation logic.

**Verification & Course Correction:** During evaluation, I caught a critical mistake: the agent had implicitly calculated the R² *in-sample* on the training data instead of reporting the true out-of-sample score. I immediately directed the agent to correct this data leakage error and implement a strict **Leave-One-Year-Out (LOYO) cross-validation**. The agent corrected the code, and the true out-of-sample R² was reported and accepted.

---

## 6. Synthesis & Final Solution

**Strategy:** With the Random Forest successfully tuned and LOYO-validated, I directed the agent to synthesize the model into a final, actionable answer to the core predictive question.

**Agent Execution:** The agent generated the final historical "Climate Vulnerability Scatter Plot" and coded a loop to mathematically simulate future quality under +1°C, +2°C, and +3°C warming scenarios. 

**Verification:** I finalized the output by reviewing the agent's "Model Card" summary. I ensured the agent explicitly documented that these simulations mathematically assume no human adaptation, verifying that the project respectfully concluded with scientific honesty rather than deterministic panic.
