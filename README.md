# ASML Abnormal Returns and Market Drivers

## Overview
This project analyzes ASML’s abnormal returns relative to the Euro Stoxx 50 and studies how they relate to broader market, sector, FX, volatility, and rates signals. The analysis focuses on identifying which external drivers matter most for ASML, how those relationships evolve over time, and whether they differ across volatility regimes.

---

## Repository structure
- `asml_abnormal_returns_market_drivers.ipynb` — main notebook with the full workflow
- `requirements.txt` — Python dependencies
- `README.md` — project summary, methodology, and key findings

---

## Research Question
Which external market signals are most strongly associated with **ASML’s abnormal daily return**, and how stable are those relationships across time and volatility regimes?

### Signals included
- **SOX** (Philadelphia Semiconductor Index)
- **NDX** (Nasdaq-100)
- **EUR/USD**
- **VIX** daily change
- **U.S. 10Y Treasury yield** daily change

---

## Data
Daily market data is downloaded using `yfinance` for the following period:

- **Start:** 2022-01-01  
- **End:** 2025-07-01  

### Assets used
- ASML
- EURO STOXX 50
- SOX
- Nasdaq-100
- EUR/USD
- VIX
- U.S. 10Y Treasury yield

## Target Variable

The main variable of interest is:

**ASML abnormal return_t = ASML return_t − EURO STOXX 50 return_t**

This measures ASML’s performance relative to the broader European equity market.

---

## Methodology

### 1. Feature construction
- **Log returns** are used for price-based series:
  - ASML
  - EURO STOXX 50
  - SOX
  - NDX
  - EUR/USD
- **Daily changes** are used for level-based series:
  - VIX
  - U.S. 10Y Treasury yield

### 2. Descriptive analysis
The notebook evaluates:
- summary statistics,
- skewness,
- kurtosis,
- distribution shape,
- and extreme positive / negative abnormal-return days.

### 3. Autocorrelation
A 20-lag autocorrelation analysis is used to test whether ASML abnormal returns show meaningful short-run dependence on their own past.

### 4. Same-day correlation analysis
Contemporaneous correlations are computed between `ASML_abnormal` and each external driver.

### 5. Outlier robustness
The same-day correlation exercise is repeated after trimming the top and bottom 1% of abnormal-return observations.

### 6. Lagged correlation analysis
One-day lagged versions of the drivers are added to test whether the structure is mainly contemporaneous or whether weak predictive effects exist.

### 7. VIX-regime analysis
The sample is split into:
- **Low-VIX regime:** bottom 20% of VIX levels
- **High-VIX regime:** top 20% of VIX levels

The driver correlations are then compared across these two environments.

### 8. Rolling correlations
Rolling correlations are computed using:
- **90-day windows**
- **120-day windows**

This makes it possible to study how relationships evolve through time, especially around major macro and policy events.

### 9. Pearson vs Spearman robustness
Rolling Pearson and rolling Spearman correlations are compared for the key drivers.

### 10. Lead-lag profiles
Correlations are computed for lags from **-5 to +5** trading days to test whether the drivers lead ASML or whether the strongest relationships are concentrated at lag 0.

---

## Main Results

### Descriptive statistics of ASML abnormal return
- **Mean:** 0.000037
- **Std. dev.:** 0.025133
- **Min:** -0.168639
- **Max:** 0.099043
- **Skewness:** -0.308
- **Kurtosis:** 3.746

### Interpretation
- The distribution is close to zero-mean.
- Downside moves are somewhat heavier than upside moves.
- Tails are fatter than under a normal benchmark.

### Same-day correlations

| Driver | Correlation |
|---|---:|
| EURUSD_return | 0.6941 |
| NDX_return | 0.5486 |
| SOX_return | 0.4770 |
| VIX_change | -0.0211 |
| US10Y_change | -0.2511 |

This suggests that ASML’s benchmark-relative daily performance is most tightly linked to:
1. **EUR/USD**
2. **Nasdaq-100**
3. **SOX**

### Outlier-trimmed robustness

| Driver | Correlation |
|---|---:|
| EURUSD_return | 0.6824 |
| NDX_return | 0.5115 |
| SOX_return | 0.4535 |
| VIX_change | -0.0522 |
| US10Y_change | -0.2167 |

The ranking remains broadly intact, so the result is **not driven only by a handful of extreme days**.

### Lagged correlations

| Driver | Lag-1 corr. |
|---|---:|
| SOX_return_lag1 | 0.1453 |
| NDX_return_lag1 | 0.1343 |
| VIX_change_lag1 | 0.0823 |
| EURUSD_return_lag1 | -0.0809 |
| US10Y_change_lag1 | -0.1168 |

The key message is that the structure is mostly **contemporaneous**, not strongly predictive one day ahead.

### Low-VIX vs High-VIX regimes

#### Low-VIX regime
- EURUSD_return: **0.7858**
- NDX_return: **0.5631**
- SOX_return: **0.5250**
- VIX_change: **0.0368**
- US10Y_change: **-0.4360**

#### High-VIX regime
- EURUSD_return: **0.6064**
- NDX_return: **0.2683**
- SOX_return: **0.1534**
- US10Y_change: **0.0546**
- VIX_change: **-0.0181**

The main pattern is clear: the correlation structure becomes **weaker and noisier in high-stress periods**.

---

## Key Findings
- ASML abnormal returns show **little meaningful autocorrelation**.
- The strongest same-day drivers are **EUR/USD**, **Nasdaq-100**, and **SOX**.
- **Lagged** drivers are much weaker than same-day drivers.
- The relationships are **state dependent**:
  - in **low-VIX periods**, correlations are stronger and cleaner,
  - in **high-VIX periods**, they weaken materially.
- The lead-lag analysis suggests that the strongest relationships sit **at or near lag 0**.

---

## Why this project matters
This project:
- defines a **benchmark-relative target**,
- combines descriptive statistics with market interpretation,
- checks robustness to outliers,
- studies **regime dependence**,
- and evaluates **time-varying relationships**.

That makes it relevant for:
- **equity research**
- **market risk analysis**
- **macro-to-market transmission research**
- **quantitative exploratory analysis**

## Tech Stack
- `pandas`
- `numpy`
- `yfinance`
- `matplotlib`
- `seaborn`
- `statsmodels`

---

## How to run
1. Install the required Python packages.
2. Open the notebook.
3. Run the cells from top to bottom.
4. The notebook downloads data directly from Yahoo Finance, so an internet connection is required.

## Possible extensions
- Add multivariable regression or factor-model layers
- Run formal event studies around policy or tariff announcements
- Add rolling beta estimation instead of rolling correlation only
- Compare ASML with other semiconductor companies
- Test whether the relationships have forecasting value, not just descriptive value
