# Big Data Analytics - Load Forecasting Project

## Instructions
For the best experience viewing this project, we recommend cloning this repository and opening it in VSCode, which provides full interactivity and high-resolution figures for optimal analysis.
Alternatively, you can view the HTML rendering of the notebook (project_main.html) directly in your browser for a quick overview of the complete analysis.


## Overview

This project explores machine learning approaches for short-term electrical load forecasting, a critical application for power grid management and safety. Using historical load data from PJM's MIDATL service area (2018-2023) combined with weather data from 5 regional cities, we implement and compare three different forecasting models.

## Authors

- **Ben Menko** - bmmenko@seas.upenn.edu
- **Mukul Anand** - mukula@sas.upenn.edu  
- **Karthikeya Jayarama** - jkarthik@seas.upenn.edu


## Project Motivation

Load forecasting is literally a matter of life and death in electrical grid management. Power companies must precisely predict electricity demand to:
- Prevent blackouts that can be life-threatening (e.g., Northeast blackout of 2003 resulted in ~100 deaths)
- Optimize generation scheduling and costs
- Maintain grid stability and safety

Weather is one of the strongest predictors of load demand, making weather-based forecasting models essential for short-term (7-14 day) planning horizons.

## Dataset

- **Load Data**: PJM MIDATL service area, 2018-2023 (hourly resolution)
- **Weather Data**: 5 cities (Philadelphia, Scranton, Erie, Pittsburgh, Washington D.C.)
- **Features**: Temperature, humidity, pressure, cloud cover, wind speed, solar radiation, precipitation
- **Size**: 52,584 rows × 74 columns after preprocessing

## Models Implemented

### 1. Linear Regression (Baseline)
- **Approach**: Simple linear model with one-hot encoded temporal features
- **Performance**: RMSE 2,964.88, MAPE 8.3%, R² 23.01%
- **Strengths**: Captures general periodic trends
- **Limitations**: Ignores weather features, lacks precision for short-term forecasting

### 2. XGBoost with Feature Engineering
- **Approach**: Gradient boosting with engineered features including:
  - Lagged load values (1hr, 24hr, 48hr, 168hr)
  - Rolling averages (24hr, 168hr windows)
  - Temperature-temporal interaction features
  - Fourier transform features for periodicity
- **Performance**: RMSE 1,454.41, MAPE 3.65%, R² 81.47%
- **Strengths**: Excellent accuracy, interpretable, matches industry standards
- **Key Innovation**: Recursive forecasting methodology for multi-step predictions

### 3. Auto-regressive LSTM (Deep Learning)
- **Approach**: Deep neural network optimized for time series
- **Performance**: RMSE 1,297.02, MAPE 3.15%, R² 87.89% (24-hour horizon)
- **Strengths**: Superior accuracy within its operational window
- **Limitations**: Restricted to 24-hour forecasts due to gradient instability

## Key Technical Contributions

### Time Series Cross-Validation
- Custom validation approach using 26 consecutive 2-week windows
- Prevents data leakage and ensures robust hyperparameter selection
- Mimics real-world deployment scenarios

### Feature Engineering
- **Fourier Analysis**: Implemented harmonic features that implicitly learn load periodicity
- **Interaction Features**: Temperature-temporal combinations capture weather-dependent usage patterns
- **Recursive Architecture**: Dynamic lag feature calculation for multi-step forecasting

### Industry Comparison
- Direct comparison with PJM's operational forecasting system
- Our XGBoost model performs remarkably similar to industry standards
- Validates approach and demonstrates practical applicability

## Results Summary

| Model | RMSE | MAPE | R² | Forecast Horizon |
|-------|------|------|----|----|
| Linear Regression | 2,964.88 | 8.3% | 23.01% | 14 days |
| **XGBoost** | **1,454.41** | **3.65%** | **81.47%** | **14 days** |
| LSTM | 1,297.02 | 3.15% | 87.89% | 24 hours |

**XGBoost emerges as the optimal solution** for practical deployment due to its combination of accuracy, interpretability, and operational flexibility.

## Key Insights

1. **Weather Integration**: Unlike linear regression, XGBoost successfully incorporates weather patterns for improved accuracy
2. **Temporal Patterns**: All models capture daily/weekly seasonality, but with varying precision
3. **Industry Validation**: Close alignment with PJM's forecasts confirms model reliability
4. **Practical Constraints**: LSTM's horizon limitations highlight the importance of considering operational requirements

## Future Research Directions

1. **XGBoost Enhancement**: Additional engineered features, expanded hyperparameter search
2. **LSTM Early Warning**: Develop 24-hour system for blackout risk alerts
3. **Ensemble Methods**: Combine model strengths for improved robustness
4. **Extended Horizons**: Investigate medium-term forecasting approaches
