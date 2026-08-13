# HeatShield Pakistan 🌡️

## AI-Powered Heatwave Risk Intelligence and Decision Support System

HeatShield Pakistan is a leakage-aware data science project that analyzes heatwave vulnerability across **Karachi, Multan, and Murree** from **2021–2025**. It combines climate, health, electricity-grid, demographic, labor, and economic indicators to study heat-related health burden and translate the results into city-level risk intelligence and preparedness recommendations.

The project is organized as a **two-stage notebook pipeline**:

**Master Merged Dataset → EDA & Cleaning → Clean Dataset → Machine Learning → Risk Segmentation & Forecasting → Power BI Decision Support**

📄 **[Read the full written report](./Report_IDS_Final_Corrected.pdf)** — methodology, all result tables, figures, and references.

---

## 📌 Project Overview

Extreme heat in Pakistan affects more than temperature alone. Heat exposure can interact with hospital pressure, electricity outages, population density, poverty, water access, and labor conditions.

HeatShield Pakistan brings these dimensions into a single analytical workflow.

The project uses data assembled from multiple public and secondary sources, including **NASA POWER, NOAA, Open-Meteo, NDMA, PBS, ILO, NEPRA, K-Electric**, and other referenced sources.

### Study Scope

| Item | Details |
|---|---|
| **Cities** | Karachi, Multan, Murree |
| **Study period** | 2021–2025 |
| **Observations** | 2,310 daily city-date records |
| **Prediction target** | `monthly_heatstroke_admissions` |
| **Health outcome granularity** | Monthly |
| **Main predictive models** | Linear Regression, Random Forest, XGBoost, LightGBM, LSTM |
| **Risk segmentation** | K-Means clustering |
| **Dashboard** | Power BI |

### Research Questions

1. How have heatwave conditions changed across Karachi, Multan, and Murree from 2021–2025?
2. Which climate, infrastructure, and socioeconomic factors are associated with heatstroke admissions?
3. How accurately can machine-learning models predict heatstroke admissions?
4. Which city shows the greatest overall vulnerability?
5. Can observations be grouped into meaningful heat-risk categories?
6. What preparedness actions can be derived from the analysis for health, power, and city-planning stakeholders?

---

## 🔬 Project Architecture

HeatShield Pakistan is deliberately separated into two analytical stages.

### Stage 1 — EDA & Data Preparation

`Copy_of_EDA_Pakistan_Heatwave.ipynb`

The EDA notebook starts from the **Master Merged Dataset**, performs data inspection and exploratory analysis, standardizes column names, converts dates, and prepares the cleaned analytical dataset.

### Stage 2 — Machine Learning & Decision Support

`phase_3_The_human_cost_of_heat_.ipynb`

The Phase 3 notebook takes the **clean dataset produced by Stage 1** and performs leakage auditing, feature engineering, time-aware splitting, model training, evaluation, feature-importance analysis, K-Means risk segmentation, forecasting, and Power BI export generation.

### End-to-End Flow

```text
Master_Merged_Dataset.csv
          │
          ▼
Copy_of_EDA_Pakistan_Heatwave.ipynb
          │
          ├── Data inspection
          ├── Cleaning & standardization
          ├── EDA
          ├── Statistical analysis
          ├── Feature engineering
          ├── PCA / clustering / feature importance
          │
          ▼
clean_dataset.csv
          │
          ▼
phase_3_The_human_cost_of_heat_.ipynb
          │
          ├── Leakage audit
          ├── Time-aware feature engineering
          ├── Model training
          ├── Model evaluation
          ├── Feature importance
          ├── K-Means risk segmentation
          ├── Forecasting
          └── Power BI exports
          │
          ▼
outputs/ + figures/ + powerbi/
          │
          ▼
HEATSTROKE DASHBOARD.pbix
```

---

## 📊 Key Model Results

The final admissions-prediction workflow excludes target-derived and post-outcome variables from the prediction matrix.

| Model | R² | MAE | RMSE |
|---|---:|---:|---:|
| **XGBoost Regressor** | **0.986** | **69.69** | **131.02** |
| Random Forest Regressor | 0.979 | 80.46 | 158.45 |
| LightGBM Regressor | 0.960 | 109.61 | 219.27 |
| Linear Regression | 0.930 | 215.78 | 291.80 |
| LSTM | −0.571 | 856.81 | 1382.86 |

**XGBoost is the strongest reported model**, followed by Random Forest.

The LSTM performed substantially worse than the tabular models and is therefore treated as a **forecasting prototype**, not the project's primary predictive model.

K-Means clustering produces **Low, Medium, and High** heat-risk categories, with a reported silhouette score of approximately **0.46**.

> **Interpretation note:** These metrics should be interpreted in the context of the dataset structure and its monthly health outcome. They should not be treated as evidence that the model is ready for clinical or operational deployment.

---

## 🧠 Target Leakage: What Was Found and Fixed

An earlier version of the project produced an unrealistically perfect Linear Regression result (**R² = 1.000**).

The investigation identified target leakage: variables such as healthcare burden, productivity loss, economic loss, and admissions-derived fields contain information that is directly related to the outcome or occurs after the health impact.

The corrected modeling workflow excludes these variables from the admissions prediction matrix.

The leakage-free feature set uses pre-event or allowable predictors such as:

- `nasa_avg_temp_c`
- `nasa_precipitation_mm`
- `heat_index_c`
- `labor_avg_humidity_pct`
- `top_soil_moisture`
- `root_zone_moisture`
- `avg_monthly_shortfall_mw`
- `monthly_outage_hours`
- `population_density`
- `poverty_rate_pct`
- `clean_water_access_pct`
- Calendar features
- Prior temperature, heat-index, and outage lag/rolling features
- City as a categorical predictor

The notebook explicitly audits the feature matrix and records excluded leakage columns.

This leakage correction is intentionally documented because it is an important part of the project's modeling methodology.

---

## 🗂️ Repository Structure

```text
heatshield-pakistan/
├── Copy_of_EDA_Pakistan_Heatwave.ipynb
│   └── EDA, cleaning, statistical analysis & exploratory modeling
│
├── phase_3_The_human_cost_of_heat_.ipynb
│   └── Machine learning, evaluation, risk segmentation & forecasting
│
├── Master_Merged_Dataset.csv
│   └── Master input dataset for Stage 1
│
├── clean_dataset.csv
│   └── Cleaned dataset generated by Stage 1
│
├── HEATSTROKE DASHBOARD.pbix
│   └── Interactive Power BI dashboard
│
├── Report_IDS_Final_Corrected.pdf
│   └── Full written report (IEEE-style, methodology, tables & references)
│
├── figures/
│   └── Generated visualizations
│
├── outputs/
│   └── Model predictions, leaderboard, clusters, feature importance,
│       forecasts and Power BI-ready CSV files
│
└── README.md
```

> **Filename note:** The EDA notebook currently expects the input file to be named `Master_Merged_Dataset.csv`, and the Phase 3 notebook expects `clean_dataset.csv`. If the master file is downloaded with a name such as `Master_Merged_Dataset(1).csv`, rename it to `Master_Merged_Dataset.csv` before running Stage 1.

---

# ⚙️ How to Run

## Important: Run the notebooks in order

The project **cannot be reproduced by starting with the Phase 3 notebook**.

You must complete **Stage 1 first**, because Stage 2 uses the cleaned dataset generated by Stage 1.

---

## 1️⃣ Stage 1 — EDA & Data Cleaning

Open:

```text
Copy_of_EDA_Pakistan_Heatwave.ipynb
```

### Step 1 — Provide the Master Merged Dataset

Upload the Master Merged Dataset file into your notebook environment.

The current EDA notebook expects:

```text
Master_Merged_Dataset.csv
```

If using Google Colab, upload the file to the Colab session before running the notebook.

### Step 2 — Run the EDA Notebook

Run the notebook **from top to bottom**.

The EDA workflow includes:

- Dataset loading and structure checks
- Date and city standardization
- Column-name standardization
- Missing-value analysis
- Duplicate checks
- Univariate analysis
- Bivariate analysis
- City-wise comparison
- Time-series and trend analysis
- Seasonal analysis
- Outlier detection
- Feature engineering
- PCA
- K-Means exploration
- Statistical hypothesis testing
- Feature-importance analysis
- Summary of findings

### Step 3 — Generate the Clean Dataset

During the cleaning stage, the notebook creates:

```text
clean_dataset.csv
```

The current notebook writes this file directly with:

```python
df.to_csv("clean_dataset.csv", index=False)
```

This is the **clean dataset that must be passed to Stage 2**.

---

# 2️⃣ Stage 2 — Machine Learning & Risk Intelligence

After Stage 1 has finished, open:

```text
phase_3_The_human_cost_of_heat_.ipynb
```

### Step 4 — Provide the Clean Dataset

Upload the `clean_dataset.csv` file produced by Stage 1 into the Phase 3 notebook environment.

The Phase 3 notebook expects:

```text
clean_dataset.csv
```

Do **not** replace it with the original Master Merged Dataset.

### Step 5 — Install Dependencies

Install the required Python packages:

```bash
pip install pandas numpy scikit-learn scipy statsmodels xgboost lightgbm tensorflow matplotlib seaborn pillow openpyxl
```

### Step 6 — Run the Phase 3 Notebook

Run all cells from top to bottom.

The notebook performs:

1. Dataset validation
2. Target and feature identification
3. Target-leakage auditing
4. Time-aware train/test splitting
5. Lag and rolling feature engineering
6. City encoding
7. Missing-value handling for the model matrix
8. Feature scaling
9. Linear Regression
10. Random Forest
11. XGBoost
12. LightGBM
13. LSTM sequence forecasting
14. Model comparison
15. Feature-importance analysis
16. K-Means risk segmentation
17. Future forecast generation
18. Power BI dataset generation
19. Best-model and preprocessor export

---

## 🤖 Model Availability and Fallbacks

The Phase 3 notebook checks whether the major ML libraries are available.

If the native libraries are installed:

- XGBoost uses the native XGBoost implementation.
- LightGBM uses the native LightGBM implementation.
- TensorFlow/Keras is used for the LSTM.

If one of these libraries is unavailable, the notebook uses a transparent fallback and labels the result accordingly.

**For reproduction of the reported model metrics, install the native libraries listed above.**

In particular, the reported XGBoost, LightGBM, and LSTM results should be reproduced with the corresponding native packages installed.

---

## 📁 Generated Outputs

The Phase 3 notebook creates these directories automatically:

```text
outputs/
figures/
powerbi/
```

### `outputs/`

Contains model and analysis outputs such as:

```text
bi_clustering_metrics.csv
bi_clusters.csv
bi_feature_importance.csv
bi_future_forecast.csv
bi_model_leaderboard.csv
bi_predictions_lr.csv
bi_predictions_rf.csv
bi_predictions_xgb.csv
bi_predictions_lgb.csv
bi_predictions_lstm.csv
clean_dataset.csv
best_heatwave_model.pkl
preprocessor.pkl
```

The exact output set may vary slightly depending on the execution environment and model availability.

### `figures/`

Contains generated visualizations, including model-comparison, prediction, feature-importance, clustering, and forecasting figures.

### `powerbi/`

Contains copies of the Power BI-ready CSV files generated from the Phase 3 outputs.

---

# 📊 Power BI Dashboard

After Stage 2 has been executed, open:

```text
HEATSTROKE DASHBOARD.pbix
```

using **Power BI Desktop**.

The dashboard is organized around the project's analytical questions and decision-support workflow.

### Dashboard pages

| Page | Purpose |
|---|---|
| **Command Center** | Executive overview, KPIs, city risk and admissions trends |
| **Climate Trend Intelligence** | Climate patterns and city-level heat trends |
| **Heatstroke Driver Analysis** | Factors associated with heatstroke admissions |
| **Human & Economic Impact** | Admissions, healthcare, outage, labor and economic impact |
| **Predictive Model Intelligence** | Model leaderboard, predictions and error analysis |
| **Risk Segmentation** | K-Means risk categories and city vulnerability |
| **Future Outlook & Policy Recommendations** | Forecasts and city-specific preparedness actions |

The dashboard uses Power BI-ready files generated by the Phase 3 notebook.

---

## 🧰 Tech Stack

### Programming & Data

- Python
- pandas
- NumPy
- scikit-learn

### Machine Learning

- Linear Regression
- Random Forest Regressor
- XGBoost Regressor
- LightGBM Regressor
- LSTM / TensorFlow / Keras
- K-Means clustering

### Analysis

- Exploratory Data Analysis
- Statistical hypothesis testing
- PCA
- Feature engineering
- Time-aware train/test splitting
- Lag and rolling features
- Target-leakage auditing
- Feature importance

### Visualization

- Matplotlib
- Seaborn
- Power BI

---

## ⚠️ Important Data & Methodology Notes

### 1. The health outcome is monthly

`monthly_heatstroke_admissions` represents a **monthly heatstroke admission figure**.

The analytical dataset contains **daily city-date rows**, but the monthly admission value is repeated across the daily records belonging to that month.

Therefore, the predictive target should be described as:

> **monthly heatstroke admission level**

and **not** as true daily hospital admissions.

This distinction is important when interpreting both the model and the dashboard.

### 2. The dataset is multi-source

The project combines data from multiple sources with different temporal coverage and levels of measurement.

Some variables have substantially more missing values than others because their source data are available only for particular cities, periods, or collection windows.

The EDA notebook explicitly analyzes missingness before the modeling stage.

### 3. Heatstroke admissions are not a perfect ground truth

Some heatstroke-related figures are supplemented by secondary reporting sources such as news reports, while official sources may undercount heat-related impacts.

These data should therefore be treated as a research dataset rather than a gold-standard hospital registry.

### 4. Daily rows do not mean daily health labels

Weather and infrastructure variables can vary daily, but the target remains monthly.

This means the dataset should not be interpreted as containing 2,310 independent daily hospital-admission observations.

### 5. High model scores require careful interpretation

The reported XGBoost R² of approximately 0.986 is strong, but it does not by itself establish real-world forecasting performance.

The dataset has a short 2021–2025 observation window, repeated monthly outcome values, heterogeneous source coverage, and engineered temporal features.

The project therefore presents the models as a **research and decision-support prototype**.

---

## 🎯 Key Findings

The project identifies several important patterns:

- **Karachi** ranks highest in overall vulnerability in the project's composite assessment.
- **Multan** has the highest average temperature profile among the three cities.
- **Murree** generally has lower heat exposure and admissions but is important for identifying unusual heat events.
- Grid-related indicators such as **electricity shortfall and outage hours** emerge as important leakage-free predictors.
- Socioeconomic indicators including **poverty, population density, and clean-water access** contribute to the vulnerability picture.
- XGBoost and Random Forest outperform the Linear Regression baseline in the corrected modeling workflow.
- K-Means produces Low, Medium, and High operational risk profiles.

---

## 🚨 Decision-Support Concept

HeatShield Pakistan is designed to connect analysis with preparedness decisions.

### Risk Monitor

Identify city-level heat-risk conditions and support alerting concepts.

### AI Forecasting Engine

Estimate future heatstroke admission levels using the selected predictive models.

### City Vulnerability Intelligence

Compare cities using combined health, climate, infrastructure, and socioeconomic indicators.

### What-If Simulation

Explore how changes such as increased outage hours could affect risk-related indicators in a future version.

### Emergency Planning Assistant

Translate elevated risk into preparedness concepts such as hospital surge readiness, cooling centers, water access, and backup power.

### Policy Recommendation Engine

Generate city-specific preparedness recommendations:

- **Karachi:** hospital surge capacity, cooling centers, ambulance readiness and backup power
- **Multan:** extreme-heat protection, worker safety, hydration and infrastructure preparedness
- **Murree:** unusual-heat monitoring, tourism alerts and local preparedness

> Some of these capabilities are represented through analytical outputs and dashboard concepts rather than a fully deployed real-time operational system.

---

## 🔭 Future Work

- Independently verified daily or hourly hospital admission data
- Live weather forecasting feeds
- Satellite-based urban heat-island indicators
- GIS neighborhood-level vulnerability mapping
- Real-time power-outage telemetry
- Longer historical observation periods
- Improved sequence forecasting
- SMS and public-alert integration
- Automated intervention and response logging

---

## 👥 Authors

**Ayesha Ghani · Muhammad Dawood Abbasi · Anish Fatima**

---

## 🙏 Data Sources & Acknowledgments

The project uses data and information from multiple publicly available and referenced sources, including:

- NASA POWER
- NOAA GHCN-Daily
- Open-Meteo
- NDMA
- PBS
- PMD
- ILO
- NEPRA
- K-Electric
- Referenced secondary reporting sources

See the notebooks and supporting documentation for source-specific details and methodology.

---

## ⚖️ Project Status

HeatShield Pakistan is a **data science and decision-support prototype**.

It is not a validated clinical forecasting system, an official emergency-warning service, or a production government platform.

The project's purpose is to demonstrate how multi-source climate, health, infrastructure, and socioeconomic data can be integrated into a reproducible analytical workflow and translated into decision-support insights.
