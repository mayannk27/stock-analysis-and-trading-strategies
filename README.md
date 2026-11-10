# Stock Analysis and Trading Research

A collection of Jupyter notebook experiments covering stock-market analysis, feature engineering, machine learning, time-series methods, and trading-strategy research.

The work centres mainly on **Palo Alto Networks (PANW)** and also includes separate experiments on **Square/Block (SQ)** and **Tesla (TSLA)**.

> **Research status:** This repository organises the supplied research notebooks. It is not a production trading system, and the strategies and reported performance have not been independently validated. Review the limitations below before relying on any result.

## Repository contents

```text
stock-analysis-trading-research/
├── README.md
├── requirements.txt
├── .gitignore
├── .env.example
├── data/
│   └── README.md
└── notebooks/
    ├── README.md
    ├── 01_panw_feature_engineering_and_machine_learning.ipynb
    ├── 02_panw_exploration_lstm_and_macro_features.ipynb
    ├── 03_panw_kalman_garch_and_trading_strategies.ipynb
    ├── 04_square_kalman_trading_experiment.ipynb
    └── 05_tesla_price_distribution_experiment.ipynb
```

## Notebook guide

| Notebook | Focus |
|---|---|
| `01_panw_feature_engineering_and_machine_learning.ipynb` | PANW and peer-stock exploration, feature database construction, feature importance/selection, and regression model comparisons |
| `02_panw_exploration_lstm_and_macro_features.ipynb` | PANW price exploration, moving averages/returns, LSTM experiment, macroeconomic variables and additional modelling experiments |
| `03_panw_kalman_garch_and_trading_strategies.ipynb` | PANW GARCH and Kalman-filter experiments, prediction signals and trading-rule experiments |
| `04_square_kalman_trading_experiment.ipynb` | Kalman-filter-based long/short and buy-and-hold experiments using SQ |
| `05_tesla_price_distribution_experiment.ipynb` | TSLA adjusted-close distribution and monthly price exploration using KDE |

The first three notebooks are related PANW research, with some overlap between the exploration notebooks. They are kept separate to preserve the original work. SQ and TSLA are separate experiments, not part of the PANW analysis.

## Setup

Python 3.10 or 3.11 is a reasonable starting point for this older collection of packages.

```bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**
```powershell
.venv\Scripts\Activate.ps1
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

Start Jupyter:

```bash
jupyter lab
```

Open a notebook from the `notebooks/` folder and run cells in order. Some notebooks were originally developed in Google Colab and may need small path or environment adjustments when run locally.

## Data and credentials

- Market data is retrieved in notebooks using Yahoo Finance (`yfinance`).
- Macroeconomic data uses FRED (`fredapi`).
- Fama-French data is retrieved through `pandas-datareader`.
- Some PANW feature-engineering cells expect `ADS_Index_Most_Current_Vintage.xlsx`.
- The ADS Excel file was not among the supplied files. Obtain it from its original source and place it in a local data folder, then update the relevant notebook path.
- Data downloads can change over time. Record the retrieval date if you reproduce results.

### FRED API key

The hard-coded FRED key found in the supplied notebooks has been removed from the repository copy. Set your own key in the environment before running the relevant cells.

1. Copy `.env.example` to `.env`.
2. Add your own FRED API key to `.env`.
3. Load it in a notebook with `python-dotenv`, or set `FRED_API_KEY` in your shell.

Example notebook setup:

```python
import os
from dotenv import load_dotenv
from fredapi import Fred

load_dotenv()
fred = Fred(api_key=os.getenv("FRED_API_KEY"))
```

Never commit `.env` or paste API keys into notebooks. Because a key was present in the original uploaded notebooks, revoke/rotate that key with FRED if it is still active.

## Important research limitations

These notebooks are research experiments and need validation before their outputs are interpreted as evidence of a tradable strategy. In particular:

- Some workflows need chronological train/test separation and careful prevention of look-ahead/data leakage.
- A Kalman smoother uses information from later observations; it should not be used as if it were a real-time signal without redesigning the experiment around filtering.
- The LSTM preprocessing should fit scalers on training data only.
- The original SQ backtest contains accounting/return-calculation issues and should not be treated as a validated performance result.
- Model metrics from different notebooks may use different targets and test periods, so they are not directly comparable.
- Any claimed strategy return, including a return above 62%, should be re-established with a corrected, leakage-aware backtest including transaction costs and realistic execution assumptions.

See `notebooks/README.md` for the notebook inventory and `data/README.md` for data handling notes.

## Disclaimer

This repository is for educational and research purposes only. It is not investment advice or a recommendation to buy or sell securities. Past or simulated performance does not guarantee future results.
