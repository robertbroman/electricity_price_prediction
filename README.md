# Electricity Price Prediction (SE3) from Weather Data 🇸🇪⚡

This project predicts Swedish electricity prices in price area SE3 using weather data from selected cities. It uses Hopsworks Feature Store for feature storage and runs daily pipelines with GitHub Actions.

## What it does

Collects hourly weather data (Open-Meteo) for several cities

Stores weather as a wide table (one row per hour, many columns)

Aggregates electricity prices to hourly (including 15-minute prices)

Trains an XGBoost model to predict sek_per_kwh

Runs daily weather update + batch inference notebooks automatically

## Files

locations.json: city names + coordinates (sanitized lowercase names)

backfill_weather_data_2.ipynb: backfills historical weather (wide format)

backfill_electricity_prices_hourly.ipynb: backfills electricity prices and aggregates to hourly

training_pipeline.ipynb: trains the model and saves it

daily_feature_pipeline.ipynb: fetches weather forecast and updates feature store

batch_inference_pipeline.ipynb: loads model and generates predictions

## Feature Store (Hopsworks)

Weather feature group: wide hourly weather features, joined via weather_key + time

Electricity feature group: hourly price data (label = sek_per_kwh) with weather_key for joins

## Automation (GitHub Actions)
A scheduled workflow runs:

daily_feature_pipeline.ipynb

batch_inference_pipeline.ipynb

## Secrets needed

HOPSWORKS_API_KEY

HOPSWORKS_PROJECT

## Notes

All timestamps are handled in UTC to avoid mismatch.