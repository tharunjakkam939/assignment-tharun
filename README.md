# German Electricity Demand Forecasting

**Student:** Avusali Mani Harshith  
**Student ID:** 24005856  

A reproducible time-series forecasting study of German electricity demand using statistical benchmarks, SARIMA/SARIMAX, feature-based machine learning, and an hourly Long Short-Term Memory (LSTM) neural network.

---

## Project overview

This project models German electricity demand from **1 January 2015 to September 2020** using the publicly available Open Power System Data time-series dataset. It evaluates several forecasting approaches under clearly separated forecast protocols:

- fixed-origin two-year weekly forecasting;
- rolling one-week-ahead weekly forecasting;
- rolling one-hour-ahead hourly forecasting;
- conditional forecasting using observed future temperature.

The workflow includes data validation, exploratory analysis, stationarity testing, benchmark forecasting, exhaustive SARIMA parameter search, residual diagnostics, uncertainty intervals, exogenous regressors, feature engineering, time-series cross-validation, recursive forecasting, LSTM tuning, statistical comparison, and robustness analysis.

> **Important:** Results from different forecast protocols are reported separately and should not be compared as though they were produced under the same information set or forecast horizon.

---

## Assignment coverage

The notebook implements all required modelling components:

- Data download, validation, cleaning and aggregation
- Hourly, daily and weekly time-series preparation
- Exploratory data analysis and seasonal decomposition
- ADF and KPSS stationarity tests
- ACF, PACF and differencing analysis
- Mean, naive, seasonal-naive and drift benchmarks
- Exhaustive SARIMA search over:
  - `p = 0, ..., 6`
  - `d = 0, 1, 2`
  - `q = 0, ..., 6`
- SARIMA validation, residual diagnostics and confidence intervals
- SARIMAX models using Berlin temperature and German public holidays
- Random Forest and Gradient Boosting regression
- Leakage-safe lag and rolling-window features
- Recursive fixed-origin feature-based forecasting
- Hourly LSTM feature, lookback, architecture and hyperparameter experiments
- RMSE, MAE, sMAPE, MASE and R² evaluation
- Protocol-matched Diebold-Mariano testing
- Robustness and operational decision-support analysis

---

## Dataset

**Source:** Open Power System Data  
**File:** `time_series_60min_singleindex.csv`  
**Official URL:**  
https://data.open-power-system-data.org/time_series/2020-10-06/time_series_60min_singleindex.csv

**Target variable:**  
`DE_load_actual_entsoe_transparency`

The notebook automatically downloads the official dataset if a complete local copy is not available. The raw CSV is intentionally excluded from version control because it is large and can be reproduced from the official source.

Berlin temperature data are retrieved from the Open-Meteo historical archive API. German public-holiday variables are generated using the `holidays` Python package.

---

## Forecasting models

### Statistical benchmarks

- Mean forecast
- Naive forecast
- Seasonal-naive forecast
- Drift forecast

### Statistical time-series models

- SARIMA
- SARIMAX with Berlin temperature
- SARIMAX with German holiday count
- SARIMAX with temperature and holiday count

### Feature-based machine learning

- Random Forest Regressor
- Gradient Boosting Regressor

### Deep learning

- Hourly LSTM
- Feature-set ablation
- Lookback-window comparison
- Architecture comparison
- Units, dropout and learning-rate refinement
- Early stopping and model checkpointing

---

## Forecast protocols

### Fixed-origin weekly forecast

The final two years are forecast from one origin at the end of the training period. No actual test-period demand is supplied to recursive operational models.

### Rolling weekly forecast

Each test week is forecast using information available up to the previous week. This protocol is used for one-step-ahead weekly comparisons.

### Rolling hourly forecast

The LSTM predicts each test hour using the immediately preceding observed sequence. It is compared with hourly persistence and hourly seasonal-naive benchmarks under the same forecast protocol.

### Conditional temperature forecast

Models using observed future test-period temperature are explicitly labelled **conditional forecasts**. They are not presented as fully operational forecasts because future observed temperature is unavailable at the original forecast origin.

---

## Key results from the executed notebook

### Fixed-origin weekly evaluation

| Model | RMSE |
|---|---:|
| Seasonal Naive | **2965.41** |
| Recursive Gradient Boosting | 3285.93 |
| SARIMAX: Holiday Count | 4184.78 |
| SARIMA | 4197.03 |
| Mean | 4380.97 |
| Naive | 4583.43 |
| Drift | 4652.82 |

The seasonal-naive model provides the strongest fixed-origin two-year weekly result.

### Rolling weekly evaluation

| Model | RMSE |
|---|---:|
| Gradient Boosting: Demand and Holidays | **2519.82** |
| Rolling Seasonal Naive | 2616.62 |
| Gradient Boosting: Demand Only | 2664.82 |
| Random Forest | 2679.75 |

The strongest rolling feature ablation improves RMSE over rolling seasonal naive by approximately **3.70%**. The associated Diebold-Mariano result is not statistically significant at the 5% level (`p ≈ 0.359`), so the improvement is interpreted cautiously.

### Rolling hourly evaluation

| Model | RMSE |
|---|---:|
| LSTM | **1347.52** |
| Persistence Naive | 2489.99 |
| Hourly Seasonal Naive | 4287.13 |

The LSTM substantially outperforms both hourly benchmarks under the same rolling one-hour-ahead protocol.

> Exact figures, additional metrics, diagnostics and uncertainty results are available in the executed notebook and generated CSV tables.

---

## Repository structure

```text
german-electricity-demand-forecasting/
├── 24005856_German_Electricity_Forecasting.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── results/
    ├── figures/
    └── tables/
```

Generated model files are stored locally under `results/models/` but are excluded from Git by default because trained neural-network files can be large. They can be reproduced by running the notebook.

---

## Installation

### Recommended environments

- Kaggle Notebook with Internet enabled
- Google Colab with Internet enabled
- Local Jupyter environment with Python 3.10 or 3.11

A GPU is recommended for LSTM training. SARIMA, SARIMAX and scikit-learn models primarily use the CPU.

### Local setup

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd german-electricity-demand-forecasting

python -m venv .venv
```

Activate the environment:

**Windows**

```bash
.venv\Scripts\activate
```

**macOS/Linux**

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter lab
```

Open `24005856_German_Electricity_Forecasting.ipynb` and run all cells sequentially.

---

## Reproduction procedure

1. Enable Internet access.
2. Enable a GPU accelerator where available.
3. Open the final notebook.
4. Select **Restart Session and Run All**.
5. Allow the notebook to download and validate the official electricity dataset.
6. Allow all SARIMA candidates and LSTM experiments to finish.
7. Confirm the final submission-check table contains only `True`.
8. Confirm the notebook prints:

```text
All executable submission checks passed.
```

9. Review generated artefacts under `results/`.

A complete run may take approximately one to three hours depending on CPU speed, SARIMA convergence and GPU availability.

---

## Main generated outputs

### Tables

- `fixed_origin_weekly_metrics.csv`
- `rolling_weekly_metrics.csv`
- `hourly_lstm_metrics.csv`
- `sarima_required_grid_results.csv`
- `sarima_shortlist_validation.csv`
- `protocol_matched_diebold_mariano_tests.csv`
- `fixed_origin_robustness_error_analysis.csv`
- `operational_decision_support.csv`
- `submission_checks.csv`

### Figures

- Fixed-origin benchmark forecasts
- SARIMA and SARIMAX forecasts
- Residual diagnostics
- Feature-model comparisons
- Rolling weekly comparison
- LSTM training history
- Full two-year LSTM forecast
- Final fixed-origin and rolling comparison figures

---

## Reproducibility and leakage controls

The implementation uses:

- a fixed random seed of `42`;
- chronological training, validation and test splits;
- training-only feature scaling;
- time-series cross-validation rather than random cross-validation;
- lagged and rolling demand features constructed from earlier observations only;
- recursive prediction for fixed-origin feature-based forecasts;
- separate reporting of fixed-origin and rolling protocols;
- explicit conditional labelling where observed future temperature is used;
- final executable checks for forecast coverage, convergence, finite metrics and output creation.

---

## Limitations

- Observed future temperature creates a conditional rather than fully operational forecast.
- The selected SARIMA specification may exhibit over-differencing risk despite validation-based selection.
- Long-horizon SARIMA/SARIMAX uncertainty intervals show undercoverage.
- The strongest rolling feature ablation was identified after reviewing held-out ablation results and is therefore interpreted as post-hoc evidence.
- Forecast performance is based on one German national demand series and one representative temperature location.

---

## Academic-use note

This repository supports the accompanying assessed report. The report contains the literature review, critical interpretation, answers to the assignment questions, model recommendation, limitations and future-work discussion.

The notebook, report and repository should be submitted in accordance with the university's academic-integrity and assessment regulations.

---

## Author

**Avusali Mani Harshith**  
**Student ID:** 24005856
