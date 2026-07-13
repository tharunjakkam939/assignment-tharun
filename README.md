# German Electricity Demand Forecasting

**Author:** Avusali Mani Harshith  
**Student ID:** 24005856

## Overview

This repository contains a reproducible forecasting workflow for German electricity demand. The analysis uses hourly electricity-load data from Open Power System Data and historical Berlin temperature data from Open-Meteo.

The project evaluates statistical benchmarks, seasonal time-series models, feature-based machine-learning models, and an hourly Long Short-Term Memory network.

## Objectives

The workflow is designed to:

- prepare and validate German electricity-demand data;
- examine trend, seasonality and stationarity;
- compare benchmark forecasting methods;
- build and diagnose SARIMA and SARIMAX models;
- evaluate temperature and holiday variables;
- train Random Forest and Gradient Boosting regressors;
- train and tune an hourly LSTM network;
- compare models using consistent forecast protocols and evaluation metrics;
- export plots, predictions, diagnostics and metric tables.

## Data sources

### Electricity demand

- **Provider:** Open Power System Data
- **Dataset:** `time_series_60min_singleindex.csv`
- **Target column:** `DE_load_actual_entsoe_transparency`
- **Analysis period:** January 2015 to September 2020

The notebook downloads the official file automatically when a complete local copy is not available.

### Temperature

Historical Berlin temperature data are retrieved from the Open-Meteo archive API.

### Holidays

German public-holiday variables are generated with the `holidays` Python package.

## Forecasting methods

### Benchmarks

- Mean
- Naive
- Seasonal naive
- Drift

### Statistical models

- SARIMA
- SARIMAX with temperature
- SARIMAX with holiday count
- SARIMAX with temperature and holiday count

### Machine-learning models

- Random Forest Regressor
- Gradient Boosting Regressor

### Neural-network model

- Hourly LSTM
- Feature-group comparison
- Lookback-window comparison
- Architecture comparison
- Units, dropout and learning-rate tuning
- Early stopping and model checkpointing

## Forecast protocols

The notebook reports results separately for:

- a single-origin, multi-step weekly forecast over the two-year holdout;
- rolling one-week-ahead weekly forecasts;
- rolling one-hour-ahead hourly forecasts;
- conditional forecasts that use observed temperature from the evaluation period.

Results from different protocols are not combined into one ranking because their forecast horizons and information sets differ.

## Evaluation

The notebook calculates:

- Root Mean Squared Error
- Mean Absolute Error
- Symmetric Mean Absolute Percentage Error
- Mean Absolute Scaled Error
- Coefficient of Determination

It also includes:

- residual diagnostics;
- forecast intervals;
- interval coverage;
- Diebold-Mariano comparisons for compatible rolling forecasts;
- seasonal, holiday and temperature-group robustness analysis.

## Primary environment

The notebook was developed and executed in **Kaggle**.

Recommended Kaggle settings:

- Internet access enabled
- GPU accelerator enabled for LSTM training
- standard CPU resources for SARIMA, SARIMAX and scikit-learn models

### Run in Kaggle

1. Create or open a Kaggle Notebook.
2. Upload `24005856_German_Electricity_Forecasting.ipynb`.
3. Enable Internet access.
4. Enable a GPU accelerator.
5. Restart the session.
6. Run all cells from the beginning.
7. Review the generated files in the `results` directory.

The full workflow can take considerable time because it includes an exhaustive SARIMA search and multiple LSTM experiments.

## Alternative environments

The notebook can also run in:

- Google Colab with Internet access;
- JupyterLab on a local computer with the required Python packages.

Install the dependencies with:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Then open the notebook in JupyterLab:

```bash
jupyter lab
```

## Repository structure

```text
german-electricity-demand-forecasting/
├── 24005856_German_Electricity_Forecasting.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── results/
    ├── figures/
    ├── tables/
    ├── models/
    │   └── .gitkeep
    ├── hourly_german_load.csv
    ├── daily_german_load.csv
    ├── weekly_german_load.csv
    ├── berlin_daily_temperature.csv
    ├── berlin_hourly_temperature.csv
    └── weekly_temperature_holidays.csv
```

## Generated outputs

### `results/figures`

Contains visual outputs for:

- hourly, daily and weekly demand;
- calendar demand patterns;
- seasonal decomposition;
- stationarity transformations;
- ACF and PACF;
- benchmark forecasts;
- SARIMA diagnostics and forecasts;
- SARIMAX comparisons;
- machine-learning forecasts and importance;
- LSTM training and forecasts;
- weekly model comparisons.

### `results/tables`

Contains CSV outputs for:

- data-quality summaries;
- descriptive statistics;
- stationarity tests;
- benchmark predictions and metrics;
- SARIMA search and validation;
- SARIMA diagnostics and intervals;
- SARIMAX coefficients, predictions and metrics;
- machine-learning tuning and feature comparisons;
- LSTM experiments, predictions and metrics;
- statistical comparisons;
- robustness analysis;
- operational decision support.

### `results/models`

Contains model artefacts produced during execution. Large model files are excluded from Git tracking because they can be recreated by running the notebook.

## Reproducibility controls

The notebook uses:

- a constant random seed;
- chronological train, validation and test partitions;
- training-only scaling;
- time-series cross-validation;
- lagged and rolling features based only on earlier observations;
- recursive prediction for long weekly forecast paths;
- separate reporting for different forecast protocols;
- explicit labelling of temperature-based conditional forecasts;
- automated validation of forecast coverage, convergence and output generation.

## Notes

- The raw electricity dataset is not stored in this repository because the notebook downloads it from the official source.
- Observed temperature from the evaluation period is used only in analyses labelled as conditional.
- The `results` directory is regenerated when the notebook is executed.
- The notebook should be run sequentially from the first cell to preserve reproducibility.
