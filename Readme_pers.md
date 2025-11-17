### Air Quality Prediction System for a Swedish City — ID2223 Lab 1 (Grade A)

This repository contains Machine Learning system for predicting PM2.5 levels for all air quality sensors in Catalunya, using the Hopsworks Feature Store, GitHub Actions, and a fully automated serverless ML pipeline.

1. Overview

This project implements a serverless ML system that:

- Ingests historical and live air quality & weather data

- Creates daily updated Feature Groups in Hopsworks

- Trains an XGBoost model to forecast PM2.5

- Runs daily batch inference for the next days

- Generates a dashboard with predictions for ALL sensors located in my chosen city

- Produces a hindcast graph to evaluate prediction accuracy


