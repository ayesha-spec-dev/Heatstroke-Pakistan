# HeatShield Pakistan 🌡️
### AI-Powered Heatwave Risk Intelligence and Decision Support System

A complete, leakage-aware data science pipeline that analyzes heatwave vulnerability across three Pakistani cities — **Karachi, Multan, and Murree** (2021–2025) — and turns climate, health, infrastructure, and economic data into a risk-monitoring and policy-recommendation tool.

Built as the final project for **DS-110: Introduction to Data Science**, Air University, Islamabad.

---

## 📌 Overview

Pakistan's summer heatwaves strain hospitals, overwhelm the power grid, and cause real economic and human loss — but there's no single dataset that connects weather, health, and infrastructure data to help authorities plan ahead. This project builds that connection.

The pipeline pulls together **14 different data sources** (NASA POWER, NOAA, Open-Meteo, NDMA, PBS, ILO, NEPRA, K-Electric, and others) into one consolidated dataset, then uses machine learning to predict heatstroke admissions, rank city vulnerability, and generate city-specific emergency and policy recommendations.

**Research questions answered:**
1. How have heatwave conditions changed across the three cities (2021–2025)?
2. Which climate, infrastructure, and socioeconomic factors drive heatstroke admissions?
3. Can machine learning accurately predict admissions?
4. Which city is most vulnerable overall?
5. Can observations be grouped into meaningful risk categories?
6. What actions should health departments, power authorities, and city planners take?

---

## 📊 Key Results

Five models were trained after removing target-leakage variables (see [Data Notes](#-important-data-notes) below):

| Model | R² | MAE | RMSE |
|---|---|---|---|
| **XGBoost Regressor** | **0.986** | 69.69 | **131.02** |
| Random Forest Regressor | 0.979 | 80.46 | 158.45 |
| LightGBM Regressor | 0.960 | 109.61 | 219.27 |
| Linear Regression | 0.930 | 215.78 | 291.80 |
| LSTM (sequence forecast) | −0.571 | 856.81 | 1382.86 |

**XGBoost performed best**, followed closely by Random Forest, which also provided the clearest feature-importance insights. The LSTM sequence model underperformed the tabular models and is kept in the project as a forecasting prototype rather than a production model — this is disclosed honestly rather than hidden.

K-Means clustering grouped observations into **Low / Medium / High** heat-risk categories (silhouette score 0.46) for use in dashboards and emergency planning.

---

## 🧠 What Makes This Project's Modeling Trustworthy

An earlier version of this project produced a suspiciously perfect Linear Regression (R² = 1.000), which is a classic sign of **target leakage** — the model was accidentally trained on variables derived from the outcome itself (like healthcare burden and economic loss, which happen *because of* heatstroke admissions, not before them).

That version was corrected: post-outcome variables were excluded from the prediction features and retained only for impact analysis and dashboards. The results above are from the **corrected, leakage-free** models. This project treats that mistake and its fix as part of the story, not something to hide.

---

## 🗂️ Repository Structure

```
heatshield-pakistan/
├── phase_3_The_human_cost_of_heat.ipynb   # Main analysis notebook (data → models → exports)
├── clean_dataset.csv                       # Cleaned, model-ready dataset (2,310 rows × 60 columns)
├── Report_IDS_Final_Corrected.docx         # Full written report (IEEE-style)
├── phase_3_visualization.pbix              # Power BI dashboard
├── figures/                                 # Generated charts (created when notebook runs)
├── outputs/                                  # Model exports, predictions, leaderboard (created when notebook runs)
└── README.md
```

---

## ⚙️ How to Run

HeatShield Pakistan is organized as a two-stage notebook pipeline. The notebooks should be executed in the following order.

### 🔄 Complete Workflow

**Master Merged Dataset (.xlsx)**
→ **EDA & Data Cleaning**
→ **Clean Dataset (.xlsx)**
→ **Machine Learning & Risk Analysis**
→ **Predictions & Power BI Outputs**
→ **Interactive Power BI Dashboard**

---

### Stage 1 — Exploratory Data Analysis & Data Cleaning

Open:

`Copy_of_EDA_Pakistan_Heatwave.ipynb`

This notebook is the starting point of the project.

#### 1. Provide the Master Merged Dataset

Upload or place the **Master Merged Dataset Excel file** where the notebook's data-loading section expects it.

This is the raw consolidated dataset used as the starting point for the HeatShield Pakistan analysis.

#### 2. Run the EDA notebook

Run the cells in:

`Copy_of_EDA_Pakistan_Heatwave.ipynb`

from top to bottom.

The notebook performs the exploratory data analysis and data-cleaning workflow, including data inspection, validation, cleaning, transformation, and visualization.

#### 3. Generate the Clean Dataset

At the end of the EDA workflow, the notebook produces the cleaned dataset as an **Excel file**.

This clean dataset becomes the input for the machine-learning stage.

---

### Stage 2 — Machine Learning & Risk Intelligence

After the clean dataset has been generated, open:

`phase_3_The_human_cost_of_heat_.ipynb`

#### 4. Provide the Clean Dataset

Upload or place the **Clean Dataset Excel file generated by Stage 1** where the notebook's data-loading section expects it.

Do not skip Stage 1 or manually substitute another dataset, as the machine-learning notebook is designed to use the cleaned output produced by the EDA notebook.

#### 5. Run the Machine Learning Notebook

Run the cells in:

`phase_3_The_human_cost_of_heat_.ipynb`

from top to bottom.

This notebook performs the machine-learning and decision-support workflow, including:

- Loading and validating the clean dataset
- Feature preprocessing
- Target-leakage auditing
- Time-aware train/test splitting
- Feature engineering
- Training multiple machine-learning models
- Model evaluation and comparison
- Feature-importance analysis
- K-Means heat-risk segmentation
- Prediction and forecasting
- Generation of Power BI-ready datasets
- Saving trained models and analysis outputs

### 6. Install Dependencies

Install the required Python libraries before running the notebooks:

```bash
pip install pandas numpy scikit-learn xgboost lightgbm tensorflow matplotlib seaborn pillow openpyxl

---

## 🧰 Tech Stack

- **Language:** Python (pandas, numpy, scikit-learn)
- **Models:** Linear Regression, Random Forest, XGBoost, LightGBM, LSTM (TensorFlow/Keras), K-Means
- **Visualization:** Matplotlib, Seaborn, Power BI
- **Methodology:** Time-aware train/test split, per-city lag & rolling features, target-leakage auditing

---

## ⚠️ Important Data Notes

- **Admissions data is monthly, not daily.** `monthly_heatstroke_admissions` is a monthly figure that is repeated across every day in that month. The dataset has daily rows for weather and infrastructure variables, but the health outcome itself only updates once a month — so the model is predicting *monthly* admission levels, not true day-to-day hospital counts.
- **Heatstroke figures are drawn partly from news reporting** (Dawn, Express Tribune) to supplement official NDMA numbers, which are known to undercount heat-related deaths. Neither source is a perfect ground truth.
- This is an **academic prototype**, not a validated clinical or operational forecasting tool. Before any real-world use, it would need to be validated against independently collected daily hospital records.

---

pip install pandas numpy scikit-learn xgboost lightgbm tensorflow matplotlib seaborn pillow openpyxl

---

## 🧰 Tech Stack

- **Language:** Python (pandas, numpy, scikit-learn)
- **Models:** Linear Regression, Random Forest, XGBoost, LightGBM, LSTM (TensorFlow/Keras), K-Means
- **Visualization:** Matplotlib, Seaborn, Power BI
- **Methodology:** Time-aware train/test split, per-city lag & rolling features, target-leakage auditing

---

## ⚠️ Important Data Notes

- **Admissions data is monthly, not daily.** `monthly_heatstroke_admissions` is a monthly figure that is repeated across every day in that month. The dataset has daily rows for weather and infrastructure variables, but the health outcome itself only updates once a month — so the model is predicting *monthly* admission levels, not true day-to-day hospital counts.
- **Heatstroke figures are drawn partly from news reporting** (Dawn, Express Tribune) to supplement official NDMA numbers, which are known to undercount heat-related deaths. Neither source is a perfect ground truth.
- This is an **academic prototype**, not a validated clinical or operational forecasting tool. Before any real-world use, it would need to be validated against independently collected daily hospital records.

---

## 🚀 The HeatShield Pakistan Data Product

Beyond prediction, this project is framed as a decision-support platform with six modules:

| Module | What It Does |
|---|---|
| Risk Monitor | Daily city-level risk level and alerts |
| AI Forecasting Engine | Predicts future admissions, burden, and economic impact |
| City Vulnerability Intelligence | Ranks cities by combined health, climate, and infrastructure risk |
| What-If Simulation Lab | Lets planners test scenarios (e.g. "+2 outage hours during a heatwave") |
| Emergency Planning Assistant | Converts forecasts into staffing, cooling-center, and backup-power actions |
| Policy Recommendation Engine | City-specific recommendations for Karachi, Multan, and Murree |

---

## 🔭 Future Work

- Live weather feeds and verified daily hospital admission data
- Satellite urban heat island indicators and GIS neighborhood mapping
- Real-time outage telemetry from power utilities
- SMS/public alert integration and automated response logging

---

## 👥 Authors

**Ayesha Ghani** · **Muhammad Dawood Abbasi** · **Anish Fatima**
Dept. of Creative Technologies, Air University, Islamabad
Course: DS-110 Introduction to Data Science · Instructor: Dr. Qurat-ul-Ain

---

## 🙏 Acknowledgments

Data accessed from publicly available repositories maintained by NASA, NOAA, Open-Meteo, NDMA, PBS, PMD, ILO, NEPRA, and K-Electric.
