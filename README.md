# German Electricity Demand Forecasting

**Statistical, machine-learning and LSTM forecasting of German electricity load**

**Student:** Avusali Mani Harshith  
**Student ID:** 24005856

## Project overview

This repository contains the complete reproducible workflow used to model and forecast German electricity demand with benchmark forecasts, SARIMA/SARIMAX, feature-based tree ensembles and LSTM neural networks.

The analysis uses chronological holdouts and distinguishes between fixed-origin, rolling one-step-ahead, conditional and recursive forecasting. This separation is important because models evaluated with different horizons or information sets should not be ranked as though they solve the same operational problem.

## Research objective

The central question is whether increasingly complex forecasting methods provide meaningful out-of-sample improvement over a strong seasonal-naive benchmark while respecting the information genuinely available at each forecast origin.

## Data

- **Electricity source:** Open Power System Data, 60-minute single-index time-series package.
- **Target variable:** `DE_load_actual_entsoe_transparency`.
- **Study period:** 1 January 2015 to September 2020.
- **Exogenous weather:** Berlin temperature from the Open-Meteo Historical Weather API.
- **Calendar information:** German public-holiday indicators.
- **Frequencies used:** hourly, daily and complete weekly aggregates.

The notebook downloads the public electricity and weather data and recreates the processed datasets. Raw and processed data are therefore not committed to the repository.

## Assignment coverage

The repository includes evidence for all major assignment tasks:

- data retrieval, cleaning, validation and aggregation;
- exploratory plots and seasonal decomposition;
- ADF, KPSS, ACF, PACF and differencing analysis;
- mean, naive, seasonal-naive and drift benchmarks;
- the required 147-combination SARIMA `p,d,q` search;
- seasonal SARIMA refinement, residual diagnostics and forecast intervals;
- SARIMAX models with temperature and holiday covariates;
- Random Forest and Gradient Boosting regression models;
- leakage-safe lag, rolling and calendar features;
- hourly LSTM feature, lookback and architecture tuning;
- rolling and strict recursive forecasting protocols;
- RMSE, MAE, sMAPE, MASE, R-squared and Diebold-Mariano comparisons;
- operational recommendations, limitations and future work;
- an aligned eight-page report and reproducibility documentation.

## Forecasting protocols

| Protocol | Resolution | Information available | Purpose |
|---|---|---|---|
| Fixed-origin | Weekly | Information available at the September 2018 forecast origin | Two-year operational stress test |
| Rolling one-step | Weekly | Demand observed through the previous week | Short-term weekly operation |
| Rolling one-step | Hourly | Demand observed through the previous hour | Short-term LSTM evaluation |
| Conditional fixed-origin | Weekly | Observed future temperature | Explanatory analysis, not a true operational forecast |
| Recursive fixed-origin | Hourly | Previous model predictions only | Long-horizon LSTM stress test |

Results from different protocols are reported separately because their horizons and information sets are not equivalent.

## Methods

- Mean, naive, seasonal-naive and drift forecasts
- SARIMA and SARIMAX
- Random Forest and Gradient Boosting
- Hourly LSTM
- ADF and KPSS stationarity tests
- ACF, PACF and seasonal decomposition
- Residual diagnostics and interval-coverage analysis
- Diebold-Mariano forecast comparison
- Training-only time-series cross-validation
- Leakage-safe recursive and rolling evaluation

## Key results

| Protocol | Model | RMSE | Interpretation |
|---|---|---:|---|
| Fixed weekly | Seasonal naive | 2,988.25 | Strongest fully operational long-horizon weekly model |
| Fixed weekly conditional | SARIMAX temperature + holiday | 2,619.10 | Lower RMSE, but uses observed future temperature |
| Rolling weekly | Gradient Boosting | 2,401.37 | 8.55% better than rolling seasonal naive; DM `p = 0.032` |
| Rolling weekly | Seasonal naive | 2,626.00 | Rolling benchmark |
| Rolling hourly | LSTM | 1,785.23 | Strong one-hour-ahead performance |
| Rolling hourly | Persistence naive | 2,489.99 | Hourly benchmark |
| Fixed-origin recursive hourly | LSTM | 13,787.93 | Errors accumulated over the two-year recursive horizon |

The operational recommendation depends on the forecast horizon:

- use seasonal naive for transparent long-horizon weekly planning;
- use rolling Gradient Boosting for short-term weekly updates when recent demand is available;
- use the LSTM only for one-hour-ahead operation with reliable real-time inputs, monitoring and scheduled retraining;
- treat fixed-origin temperature models as conditional unless genuine weather forecasts are supplied at the forecast origin.

## Repository structure

```text
.
├── .github/workflows/ci.yml
├── data/
│   ├── README.md
│   ├── raw/
│   ├── interim/
│   └── processed/
├── docs/
│   ├── assignment_mapping.md
│   ├── execution_evidence.md
│   ├── methodology.md
│   ├── model_card.md
│   └── reproducibility.md
├── notebooks/
│   ├── 24005856_German_Electricity_Forecasting.ipynb
│   └── README.md
├── reports/
│   └── 24005856_German_Electricity_Forecasting_Report.docx
├── results/
│   ├── figures/
│   ├── models/
│   ├── tables/
│   └── README.md
├── scripts/
│   ├── compose_report_figure5.py
│   └── validate_repository.py
├── src/electricity_forecasting/
│   ├── baselines.py
│   ├── config.py
│   ├── data.py
│   ├── evaluation.py
│   ├── features.py
│   └── plotting.py
├── tests/
├── CITATION.cff
├── LICENSE
├── environment.yml
├── pyproject.toml
├── requirements-core.txt
└── requirements.txt
```

## Included results and repository storage policy

The repository contains everything needed to inspect the submitted analysis and reproduce the reported findings:

- the complete executed notebook with saved outputs;
- all final figures used to support the report;
- the exact composite used as Report Figure 5;
- the final model-comparison metrics table;
- the aligned report;
- reusable source modules, unit tests and validation scripts;
- environment and dependency files;
- execution, methodology and reproducibility documentation.

The following generated artifacts are intentionally not committed:

- the large raw electricity and weather datasets;
- regenerated interim and processed datasets;
- every exploratory or intermediate CSV table produced during development;
- large trained `.keras` and `.joblib` model binaries.

These files are not required to verify the submitted report because the notebook downloads the public data, rebuilds the processed datasets, retrains the models and regenerates the intermediate tables and model files. Excluding large generated artifacts keeps the repository reviewable and avoids unnecessary duplication. The final figures and numerical results cited in the report are included.

For archival use, large generated outputs may be attached separately as a GitHub Release or supplementary submission without changing the repository code.

## Reproducing the analysis

### Recommended: Kaggle

1. Upload `notebooks/24005856_German_Electricity_Forecasting.ipynb` to Kaggle.
2. Enable internet access for the electricity and weather downloads.
3. Enable a GPU for the LSTM sections if available; SARIMA/SARIMAX use CPU.
4. Restart the session and run all cells in order.
5. Confirm that the final validation table contains only `True` values.

### Local environment

Use Python 3.10 or 3.11.

With Conda:

```bash
conda env create -f environment.yml
conda activate german-load-forecasting
```

With `venv`:

```bash
python -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Then start Jupyter and run the notebook:

```bash
jupyter notebook
```

Internet access is required for the first data download. A complete rerun may take substantial time because of the SARIMA grid search and LSTM training.

## Reproducibility and leakage safeguards

- All train, validation and test splits are chronological.
- No random train-test shuffling is used.
- Scalers and model selection are fitted on training data only.
- Demand lags are shifted before use.
- Rolling statistics exclude the target observation.
- Fixed-origin tree forecasts recursively use earlier predictions rather than actual test demand.
- LSTM sequences predicting hour `t` end at hour `t-1`.
- Recursive fixed-origin LSTM evaluation does not read actual test-period load.
- Holiday indicators are known in advance.
- Forecasts using observed future temperature are labelled conditional.
- Weekly and hourly results are evaluated and interpreted under separate protocols.

## Kaggle execution provenance

The notebook was executed end-to-end in Kaggle before export. The final validation cell reported that all 48 executable checks passed.

Kaggle retained the cell outputs in the downloaded notebook but omitted the stored `execution_count` values. GitHub or Jupyter may therefore show `[ ]` beside code cells even though the outputs are present. This is an export-format artefact. Execution counters were not manually fabricated.

Kaggle run evidence is documented in [`docs/execution_evidence.md`](docs/execution_evidence.md).

A new full run is necessary only after changing notebook code, data processing, model logic or dependencies.

## Figure provenance and Report Figure 5

The files in `results/figures/` were taken from the executed notebook outputs. The report's Figure 5 is a layout-only composite of three direct notebook figures and does not introduce new predictions or metrics.

The exact image used in the report is stored at:

```text
results/figures/26_report_figure_5_composite.png
```

Recreate and verify it with:

```bash
python scripts/compose_report_figure5.py --verify
```

The source-image checksums and provenance are recorded in:

```text
results/figures/figure5_manifest.json
```

## Testing and validation

Install the lightweight core dependencies and run:

```bash
pip install -r requirements-core.txt
pytest -q
python scripts/validate_repository.py
```

The tests cover:

- complete-week aggregation;
- leakage-safe feature construction;
- benchmark forecast behaviour;
- evaluation metric calculations;
- repository structure and result consistency.

## Main files for assessment

- **Notebook:** `notebooks/24005856_German_Electricity_Forecasting.ipynb`
- **Report:** `reports/24005856_German_Electricity_Forecasting_Report.docx`
- **Final metrics:** `results/tables/model_metrics_summary.csv`
- **Assignment mapping:** `docs/assignment_mapping.md`
- **Execution evidence:** `docs/execution_evidence.md`
- **Reproducibility guide:** `docs/reproducibility.md`

## Academic integrity

This repository is an academic submission. The student should personally verify the code, figures, numerical results and written interpretation, retain authorship of the analysis, and comply with the university's rules on generative AI, plagiarism, collusion and assessment offences.
