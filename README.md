# ⚡️ Forecasting electricity consumption with focus on extreme weather events
> *Modeling electricity demand under flood impact 🌧️⚡*

This project combines **data science**, **time series engineering**, and **model interpretability** to forecast electricity consumption, with special attention to the **impacts of extreme weather events**, such as floods. Our objective? To build models that are not only accurate, but also **robust, explainable, and prepared for the future climate**.

---

## 🌍 Overview

Given the increasing frequency of extreme weather events, understanding how **intense rainfall and floods** affect electricity demand is important for grid operators, urban planners, and energy resilience policies.

In this notebook, we develop:
- Predictive models based on **historical consumption and climate data**;
- Realistic simulations of **consumption behavior during floods**;
- Interpretability analyses with **SHAP** values to identify *how* and *why* climate influences demand.

🎯 **Outcome**: A **resilient**, **transparent**, and **climate-adapted** forecasting system.

---

## 📊 Dataset

The main dataset (`df_final_electricity_consumption_.xlsx`) contains daily records with:

| Category | Variables |
|----------|----------|
| 🔌 **Consumption** | `Electricity_consumption` |
| 📅 **Time** | `Date`, `Month`, `Day`, `Year` |
| 🏙️ **Location** | `City` |
| ☁️ **Climate** | `Total_daily_precipitation`, `Max_temperature`, `Average_humidity`, `Max_wind_gust`, etc. |
| 🌊 **Flood** | `Flood_event` (binary), `Flood_risk_levels` (categorical) |

---

## 🧪 Methodology

The project was structured in **three stages**:

### 1. 🛠️ Pre-processing & Feature Engineering
- Conversion of the `Date` column to `datetime` type;
- Handling of categorical variables (`City`, `Flood_risk_levels`);
- Creation of **time series features**:
  - *Consumption Lags*: `consumption_lag_1`, `consumption_lag_2`, `consumption_lag_7`
  - *Moving Averages*: `consumption_rolling_mean_7d`, `consumption_rolling_mean_30d`


### 2. 🤖 Modeling
- Training and evaluation of Machine Learning models:
  - **Random Forest**
  - **XGBoost**
  - **MLP (*Multilayer Perceptron*)**
- Performance metrics: **MAE**, **RMSE**, and **R²**

### 3. 🔍 Ablation Studies

We conducted four analyses to evaluate the robustness and interpretability of the models:

#### 3.1. ⏳ Impact of **time features** on performance
- Models re-trained **without lags and moving averages**;
- Comparison of metrics to quantify performance loss.

#### 3.2. 🌊 Simulation in a complex flood scenario
- Replacement of the original target with `Electricity_consumption_simulated`;
- Base days before the flood to start the increase (2 days);
- 5% increase in consumption per ordinal risk level;
- 15% decrease in consumption per ordinal risk level;
- Base days for gradual recovery after the flood (7 days);
- Modeling of realistic consumption pattern:
  ▶️ **Pre-flood increase (5% increase)** → **Abrupt drop (15% decrease)** → **Gradual recovery (linear interpolation over 7 days**)

#### 3.3. 📅 Modeling with **flood features**
- Introduction of temporal variables related to the flood event:
  - `Days_since_flood` (Dias_desde_inundacao)
  - `Days_until_flood` (Dias_ate_inundacao)
  - `Is_pre_flood` (Is_pre_inundacao)
- Evaluation of performance gain in extreme scenarios.

#### 3.4.🔍 Interpretability with **SHAP**
- Analysis of global and local feature importance:
  - Visualizations: **bar plot** (feature importance) and **dot plot** (impact direction);
  - Special focus on **meteorological variables** (e.g., precipitation, humidity, temperature).

---

### 📈 Key Results

✅ Models with **flood features** showed **greater robustness** in extreme scenarios.

✅ **Total daily precipitation** and **dew point** were the most influential climatic variables (confirmed by SHAP).

✅ Removal of **temporal lags** resulted in a significant drop in R², proving their relevance.

✅ Realistic simulation of consumption during floods allowed for **training more resilient models**.

---

## 📂 Directory Structure
```text
/Energy_Forecast_Project/
├── Data/
│   ├── Raw_ANEEL                     <- Monthly files (.csv.gz)
    ├── Raw_Meteorological_data_INMET <- Monthly files (.csv.gz)
│   ├── Processed/                    <- Disaggregated daily data
├── Notebooks/
│   ├── Data_Processing.ipynb
│   ├── Flood_Impact_Energy_Forecast.ipynb
├── README
```
---

## 📁 Data Availability



The raw data supporting the findings of this research have been deposited in the Zenodo repository under restricted access to ensure data privacy and institutional compliance. Access is available upon request.
> [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19022528.svg)](https://doi.org/10.5281/zenodo.19022528) 

> **License**: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

Researchers interested in accessing the dataset for reproducibility or collaboration may contact the corresponding author at `dani.bromberger@ufsm.br`





---


## 🚀 Getting Started

Follow these steps to set up your local environment and reproduce the models.

### 1. Installation & Environment Setup

First, clone the repository to your local machine:

```bash
git clone (https://github.com/danibrombergerufsm/flood-impact-energy-forecasting.git)
cd flood-impact-energy-forecasting
```

It is highly recommended to use a **virtual environment** to avoid dependency conflicts:
```bash
# Create the virtual environment
python -m venv venv

# Activate the environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install all necessary dependencies
pip install -r requirements.txt
```

### 2. Running the Pipeline

The project is structured to be executed in stages:

1. **Data Processing:** Open and run the notebook (e.g., `2_Processing_meteorological_data_campo_bom_2021.ipynb`). This process will analyze the raw weather files and generate daily data on the region's weather conditions.

2. **Training:** Execute the modeling notebook (e.g., `Flood_Impact_Energy_Forecast.ipynb`). This trains the **Random Forest, XGBoost, and MLP models**.

3. **Analysis:** Use the SHAP section within the notebooks to visualize feature importance and understand how climate variables affect the forecasts.

## 📦 Requirements (`requirements.txt`)
To ensure the project runs correctly, create a file named `requirements.txt` in your root directory and include the following:

```bash
# Data Manipulation
pandas>=1.5.0
numpy>=1.23.0

# Machine Learning
scikit-learn>=1.0.0
xgboost>=1.6.0
shap>=0.41.0

# Visualization
matplotlib>=3.5.0
seaborn>=0.12.0 # Notebook Environment
ipykernel
jupyter
```

## ⚖️ License
This project is licensed under the **MIT License**. You are free to use, modify, and distribute this software, provided that the original copyright notice and permission notice are included.
