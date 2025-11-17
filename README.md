# Air Quality Prediction System for Catalunya — ID2223 Lab 1 (Grade A)

This repository contains a complete Machine Learning system for predicting PM2.5 levels for **six air quality sensors in Catalunya**, using the Hopsworks Feature Store, GitHub Actions, and a fully automated serverless ML pipeline.

---

## 1. Overview

This project implements an end-to-end serverless ML system that:

- Ingests historical and live **air quality** and **weather** data  
- Creates **daily updated Feature Groups** in Hopsworks  
- Trains an **XGBoost model per sensor** to forecast PM2.5  
- Runs **daily batch inference** for the upcoming days  
- Generates a **dashboard** with predictions for all sensors located in Catalunya  
- Produces a **hindcast graph** comparing predicted and observed values  

---

## 2. System Architecture

### **Backfill Feature Pipeline**  
**Notebook:** `notebooks/air_quality/1_air_quality_feature_backfill-per_city.ipynb`

This pipeline:
- Downloads >1 year of historical weather data from **Open-Meteo API**
- Loads historical PM2.5 measurements for each sensor from **AQICN (CSV export)**
- Registers both datasets in Hopsworks as:
  - `weather` Feature Group  
  - `air_quality` Feature Group  

---

### **Daily Feature Pipeline**  
**Notebook:** `notebooks/air_quality/2_air_quality_feature_pipeline-per_city.ipynb`

Automatically executed via **GitHub Actions**:

- Fetches today's PM2.5 observations for each sensor  
- Fetches 7-day weather forecasts  
- Updates Feature Groups in Hopsworks with the new data  

---

### **Training Pipeline**  
**Notebook:** `notebooks/air_quality/3_air_quality_training_pipeline-per_city.ipynb`

This pipeline:
- Builds a Feature View combining **weather + PM2.5**
- Splits data into train/test
- Trains an **XGBoost regressor** for each sensor
- Adds **lagged PM2.5 features** 
- Registers the trained models in Hopsworks Model Registry  
  *(one model per sensor)*

---

### **Batch Inference Pipeline**  
**Notebook:** `notebooks/air_quality/4_air_quality_batch_inference-per_city.ipynb`

Executed automatically using **GitHub Actions**:

- Loads the registered models  
- Predicts future PM2.5 values  
- Generates prediction plots for the dashboard  
- Produces hindcast graphs comparing predicted vs actual values  

---

## 3. Model Performance

Adding lagged PM2.5 features significantly improved model performance.

Example improvement for one sensor:

- **Before lag features:** MSE = 108.7  
- **After lag features:** MSE = 69.069  

This shows the strong predictive value of adding a new lagging feature representing the mean PM2.5 over the previous 3 days.

---

## 4. Dashboard

**Public Dashboard URL:**  
 https://fzohra-ux.github.io/mlfs-book/

The dashboard displays:
- Forecasts for each sensor in Catalunya  
- Hindcast performance visualizations  



