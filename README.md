# ReDi_Pavithra_Sathya_Kumar
Airport Capacitiy prediction using ML
Project Overview

This project analyses daily flight traffic across European airports and builds a machine-learning model to predict airport capacity — the total number of flights an airport can handle per day.

The dataset contains 6.8 lakh+ rows, covering over 330 airports in Europe from 2016 to 2022.
Using this data, the project performs:

Exploratory Data Analysis (EDA)
Understanding IFR/VFR behavior
Identifying the busiest airports
Traffic seasonality analysis
Comparing multiple ML models
Building a forecasting model using airport traffic history
The final goal is to forecast daily traffic at major airports, supporting operational planning such as staffing, runway scheduling, and resource management.

Motive

Airport capacity forecasting is important because air traffic is:
Seasonal (summer peaks, winter lows)
Highly influenced by weekday/weekend patterns
Sensitive to weather and IFR/VFR conditions
Strongly affected by major events such as the pandemic

Airports, airlines, and air-traffic managers need to understand these variations to plan operations efficiently.
As a student, I chose this dataset to practice:

Data cleaning
Feature engineering
Model comparison
Forecasting
Real-world aviation data analysis

Dataset Used

Source: flights.csv (provided EU airport traffic dataset)
Time period: 2016–2022
Total rows: 688,099
Columns: 14
Geographical coverage: 332 airports across Europe

Key Columns Used

  FLT_DATE → daily timestamp
  APT_ICAO → airport ICAO code
  APT_NAME → airport name
  STATE_NAME → country
  FLT_DEP_1 / FLT_ARR_1 / FLT_TOT_1 → daily departures, arrivals, and total flights
  FLT_DEP_IFR_2 / FLT_ARR_IFR_2 / FLT_TOT_IFR_2 → IFR (Instrument Flight Rules) flights
  MONTH_NUM, YEAR, DAYOFWEEK → time-based indicators
  IFR_ratio → created feature: IFR flights / total flights

Cleaning Performed

  Filled missing IFR values with 0
  Converted date to datetime
  Created IFR ratio
  Selected subsets (Top 50 EU airports, and later Germany’s busiest airports)
  Created lag features for forecasting

Project Workflow
1. Data Loading & Cleaning

Loaded flights.csv using pandas
Checked missing values and fixed IFR data
Extracted useful columns
Created new fields: year, month, weekday, weekend flag

2. Exploratory Data Analysis (EDA)

Performed multiple analyses to understand air-traffic patterns:
Daily flight trends across Europe
Top countries by traffic
Top airports by total flights
Seasonality: monthly averages
IFR vs VFR dependence
Identified busiest airports for modelling
These patterns helped decide which features to include in the prediction model.

3. Model Comparison (EU Level)

Trained three machine-learning models using the Top 50 busiest EU airports:
Linear Regression
Random Forest Regressor
XGBoost Regressor

Before fixing data leakage, results were artificially high because same-day IFR data leaked into the features.

After fixing leakage, the valid results were:

Model	MAE	RMSE	R²
Linear Regression	185.30	236.89	0.04
XGBoost	213.92	279.78	-0.34
Random Forest	223.65	295.70	-0.49

These results are expected and valid because EU-wide traffic prediction without lag features is highly complex.


4. Forecasting Model (Germany’s Busiest Airports)

For realistic prediction, I built a forecasting model using the Top 15 busiest airports in Germany, because they:
Have continuous data from 2016–2022
Show clearer seasonality
Contain fewer missing values
Are stable enough for lag-based forecasting

Lag features added:
lag_1 = yesterday’s traffic
lag_7 = traffic last week
lag_30 = traffic last month

These features allow the model to learn temporal traffic patterns, making predictions real.

Model Used:

XGBoost Regressor (best performance overall)

Train/Test Split:
Train: 2016–2020
Test: 2021–2022

5. Results of the Forecasting Model

EDDF (Frankfurt):
Predicted traffic closely follows actual values
Captures seasonal peaks, winter lows, and COVID recovery

EDDM, EDDB:
Accurate predictions with visible weekly cycles
Smaller airports (e.g., EDDR):

Noisy but trend alignment remains correct


6. Future Prediction (Next Day Forecast)

Using the latest available data, the model predicts the next day’s flight capacity for Frankfurt (or any German airport) using:

Next day’s date
lag_1, lag_7, lag_30
Airport’s IFR ratio
Month & weekday


