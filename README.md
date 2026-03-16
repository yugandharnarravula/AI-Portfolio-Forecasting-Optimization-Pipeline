# AI Portfolio Forecasting & Optimization Pipeline
![Python](https://img.shields.io/badge/Python-3.12-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-red)
![Supabase](https://img.shields.io/badge/Supabase-Database-green)
![CI](https://img.shields.io/badge/CircleCI-CI%2FCD-orange)
## Project Overview

This project implements an end-to-end machine learning pipeline for forecasting financial asset prices and optimizing portfolio allocation.
The system combines time-series forecasting with quantitative portfolio optimization to generate daily portfolio allocations based on predicted asset returns.

_This project is intended for research and educational purposes only and should not be considered financial advice._

The pipeline includes:

• Automated data ingestion  
• Time-series forecasting using Prophet  
• Portfolio optimization using Modern Portfolio Theory  
• Cloud storage using Supabase  
• CI/CD automation with CircleCI  
• Interactive analytics dashboard built with Streamlit  

**Live Application**: This project is deployed on Streamlit Community Cloud. The optimisation pipeline runs daily at 9:00 CT via GitHub Actions and the dashboard is accessible at: [portfolio-optimisation.com](https://ai-portfolio-forecasting-optimization-pipeline-hf39kcenqrwcfld.streamlit.app/#portfolio-weights)

## Components

### 1. Time Series Forecasting (Prophet)

Asset prices are forecasted using the Prophet time-series model.

The model captures:

- long-term trend components
- seasonal patterns
- holiday effects

Historical asset price data is used to train the model, which generates
next-day forecasts used to estimate expected returns for portfolio optimisation.

### 2. Portfolio Optimization (Modern Portfolio Theory)

Portfolio allocation is computed using **Modern Portfolio Theory (MPT)**, which determines asset weights that balance expected return and risk.

In this project, predicted asset returns from the forecasting model are combined with the historical covariance matrix of asset returns. An optimization algorithm then calculates portfolio weights that maximize **risk-adjusted return**.

## Project Workflow

```
Market Data Ingestion (Yahoo Finance)
        ↓
Data Preprocessing
        ↓
Time Series Forecasting (Prophet)
        ↓
Expected Return Estimation
        ↓
Portfolio Optimization (MPT)
        ↓
Results Stored in Supabase
        ↓
Portfolio Visualization via Streamlit Dashboard
        ↓
Automated Execution via CircleCI
```
## Tech Stack

- Python
- Prophet (time-series forecasting)
- SciPy (optimization solver)
- Supabase (cloud database)
- Streamlit (dashboard visualization)
- CircleCI (automation / CI pipeline)
- Poetry (dependency management)

## Installation

### Standard Installation

```bash
# Install dependencies using Poetry
make install-dev

# Or manually
poetry install
```

### Requirements

- Python 3.12+
  - I recommend installing through [PyEnv](https://github.com/pyenv/pyenv)
  - PyEnv can be installed through [Brew](https://brew.sh/).
- Poetry
  - [Basic usage](https://python-poetry.org/docs/basic-usage/)
- CircleCI account
  - [Setup guide](https://circleci.com/blog/setting-up-continuous-integration-with-github/)
- Supabase account and project
  - [Starting guide](https://supabase.com/docs/guides/getting-started)
- Hostinger VPS
  - [Starting Guide](https://www.hostinger.com/vps-hosting)

## Usage

### Basic Usage

```bash
poetry run python -m src.main
```

Or using the Makefile:

```bash
make run
```

### Configuration

Edit `src/settings.py` to customise:

- **Portfolio Tickers**: Modify `PORTFOLIO_TICKERS` list
- **Risk Aversion**: Adjust `RISK_AVERSION` (higher = more risk averse)
- **Minimum Allocation**: Change `MINIMUM_ALLOCATION` (minimum weight per asset)
- **Date Range**: Update `START_DATE` and `END_DATE` for historical data

Example:

```python
# src/settings.py
PORTFOLIO_TICKERS = ["AAPL", "MSFT", "GOOGL", "TSLA", "AMZN"]
RISK_AVERSION = 3 
MINIMUM_ALLOCATION = 0.05 
START_DATE = "2024-01-01"
```

### Programmatic Usage

```python
from src.main import run_optimisation

result = run_optimisation(
    tickers=["AAPL", "MSFT", "GOOGL"],
    start_date="2024-01-01",
    end_date="2024-12-31"
)

print(f"Optimal Weights: {result['weights']}")
print(f"Predicted Returns: {result['predicted_returns']}")
print(f"Current Prices: {result['current_prices']}")
print(f"Prediction Date: {result['prediction_date']}")
```

### Running the Streamlit Dashboard

The Streamlit dashboard reads from Supabase to display historical predictions, portfolio weights, and performance metrics:

```bash
poetry run streamlit run src/streamlit_app.py
```

Or using the Makefile:

```bash
make dashboard
```

The dashboard allows you to:
- View portfolio weights and predictions for any date
- Analyze individual stock performance over time
- Compare predicted vs actual prices
- Track prediction accuracy metrics

**Note:** The dashboard requires Supabase to be configured and populated with data from previous optimization runs.

