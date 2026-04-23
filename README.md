# 📊 Portfolio Tail Risk & Stress Correlation Analyzer

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.24-013243?logo=numpy&logoColor=white)
![yfinance](https://img.shields.io/badge/yfinance-0.2-green)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

> A quantitative stress-testing framework that analyzes how portfolio correlations and tail risk metrics shift across major market crises — exposing the hidden assumption failures that standard risk models miss.

---

## 🧠 The Core Insight

Modern portfolio theory assumes correlations are stable. **They are not.**

This project stress-tests a 5-asset ETF portfolio across three distinct market regimes — the 2008 Financial Crisis, COVID-19 (2020), and the 2022 Rate Hike Cycle — and measures exactly how much the diversification assumptions a risk desk relies on break down under pressure.

**The most significant finding:**

> During the 2022 rate hike cycle, bond-equity correlation turned **positive** — the direct opposite of what occurred in 2008. This means a risk desk running a standard 60/40 hedge in 2022 would have seen *both* legs move against them simultaneously, a scenario their historical correlation model would have labelled as near-impossible.

---

## 📁 Repository Structure

```
StressPeriodAnalysis/
│
├── 01_data_setup.ipynb            # Data ingestion, cleaning, return computation
├── 02_var_cvar.ipynb              # VaR & CVaR at 95% and 99% confidence levels
├── 03_correlation_breakdown.ipynb # Rolling correlation analysis across stress regimes
├── 04_combined_analysis.ipynb     # Full integrated analysis with equity drawdown overlays
│
├── Portfolio.csv                  # Raw portfolio price data (SPY, QQQ, XLE, GLD, TLT)
├── requirements.txt               # Python dependencies
└── README.md
```

---

## 🗂️ Notebook Breakdown

### `01_data_setup.ipynb` — Data Pipeline
- Pulls historical OHLCV data for **SPY, QQQ, XLE, GLD, TLT** via `yfinance`
- Computes daily log returns and cumulative returns
- Tags each trading day with a **market regime label** (Normal / Crisis)
- Outputs clean `Portfolio.csv` used across all downstream notebooks

### `02_var_cvar.ipynb` — Tail Risk Measurement
- Computes **Value at Risk (VaR)** at 95% and 99% confidence using historical simulation
- Computes **Conditional VaR (CVaR / Expected Shortfall)** — the expected loss *given* the tail is breached
- Runs metrics at the **individual asset level** and **portfolio level**
- Stress-period comparison: 2008 vs COVID vs 2022 CVaR side-by-side
- Equity drawdown overlays showing peak-to-trough losses per crisis

### `03_correlation_breakdown.ipynb` — Correlation Regime Analysis
- Computes **rolling 60-day correlation matrices** across the full time series
- Extracts correlation snapshots at peak stress for each of the three crises
- Produces **heatmaps** showing correlation structure shift across regimes
- Key output: bond-equity (TLT vs SPY) correlation flips from **-0.7x in 2008** to **+0.6x in 2022**

### `04_combined_analysis.ipynb` — Integrated Risk Dashboard
- Full synthesis: VaR, CVaR, rolling correlations, and drawdown in one view
- Portfolio-level stress scenario comparison table
- Outputs structured summary tables formatted for desk-ready reporting

---

## 📈 Key Results

| Metric | 2008 Crisis | COVID 2020 | 2022 Rate Hike |
|---|---|---|---|
| Portfolio Max Drawdown | ~51% | ~34% | ~22% |
| SPY CVaR (99%) | ~-4.8% daily | ~-5.1% daily | ~-2.9% daily |
| TLT–SPY Correlation | **-0.70** (negative hedge ✅) | **-0.45** (partial hedge) | **+0.62** (hedge failure ❌) |
| GLD–SPY Correlation | ~-0.20 | ~-0.10 | ~+0.30 |
| Diversification | Effective | Partial | Broke down |

> **Takeaway for a risk desk:** GLD held up as a partial diversifier in 2022 when TLT failed entirely — suggesting commodity exposure deserves re-weighting in rising-rate regimes.

---

## ⚙️ Setup & Usage

### 1. Clone the repository
```bash
git clone https://github.com/rambhutada20/StressPeriodAnalysis.git
cd StressPeriodAnalysis
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run notebooks in order
```bash
jupyter notebook
```
Open and run notebooks in sequence: `01` → `02` → `03` → `04`

> **Note:** `01_data_setup.ipynb` must be run first as it generates `Portfolio.csv` used by all downstream notebooks. If `Portfolio.csv` is already present in the repo, you can start from `02` directly.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| `yfinance` | Market data ingestion (SPY, QQQ, XLE, GLD, TLT) |
| `pandas` | Return computation, regime labelling, data wrangling |
| `numpy` | VaR/CVaR percentile calculations |
| `matplotlib` | Drawdown plots, correlation heatmaps, time series charts |
| `seaborn` | Styled correlation heatmaps |
| `scipy` | Statistical distribution fitting |

---

## 📐 Methodology Notes

**VaR Approach:** Historical simulation (non-parametric) — no distribution assumptions, reflects actual return fat tails

**CVaR:** Mean of returns below the VaR threshold; captures tail severity not just threshold breach

**Rolling Correlation Window:** 60 trading days (~3 months) — captures regime shifts without over-smoothing

**Stress Period Definitions:**
- 2008 Crisis: Sep 2008 – Mar 2009
- COVID-19: Feb 2020 – Apr 2020
- 2022 Rate Hike: Jan 2022 – Dec 2022

---

## 💡 Why This Matters for a Trading Desk

Standard risk models (parametric VaR, static correlation matrices) are calibrated on normal market periods. This project demonstrates three concrete failure modes:

1. **Correlation instability** — hedges that worked in 2008 inverted in 2022
2. **Fat tails** — parametric VaR systematically underestimates tail losses during crises
3. **Regime blindness** — a model with no regime awareness treats 2022 as a noisy version of 2019

These are not theoretical concerns — they directly affect P&L on a trading desk running correlated multi-asset books.

---

## 👤 Author

**Ram Bhutada**
M.S. Finance (STEM) — Illinois Institute of Technology, Stuart School of Business
📧 rbhutada@hawk.illinoistech.edu
🔗 [LinkedIn](https://linkedin.com/in/ram-bhutada-156695246/) | [GitHub](https://github.com/rambhutada20)
