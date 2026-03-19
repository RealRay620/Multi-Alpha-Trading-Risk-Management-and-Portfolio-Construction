# Multi-Alpha Systematic Trading, Risk Management and Portfolio Construction Strategy

## Overview
This repository implements a **research-grade, cross-sectional equity strategy** that evaluates 101 formulaic alphas and constructs a production portfolio using statistically validated signals. The framework mirrors institutional workflows used in systematic equities and quant research:

- Data engineering and investable universe construction  
- Alpha generation (delay-1; no look-ahead bias)  
- Cross-sectional evaluation (IC / Rank IC)  
- Robust inference (Newey–West HAC)  
- Factor pricing (Fama–MacBeth)  
- Portfolio construction and backtesting  

The objective is to isolate **persistent, economically meaningful signals** and aggregate them into a **diversified, capacity-aware portfolio**.

---

## Research Motivation
Cross-sectional return prediction is characterized by **low signal-to-noise ratios** and **non-IID structure**. Individual signals are weak, but **ensemble methods** can produce stable alpha when:

- Signals are **statistically validated**  
- Redundancy is controlled via **correlation filtering**  
- Portfolio construction respects **signal strength and dispersion**  

This project formalizes that pipeline end-to-end.

---

## Data Engineering

### Sources
- Yahoo Finance
- Bloomberg Terminal  
- Polygon API  

### Data Model
All features are represented as **T × N matrices**:

$$
T = \text{trading days}, \quad N = \text{securities}
$$

This enables efficient cross-sectional operators and vectorized computation.

---

## Return Construction

$$
\log(1 + R_t) = \log\left(\frac{F_t P_t + D_t}{P_{t-1}}\right)
$$

$$
F_t = 1 + B_t + S_t
$$

$$
D_t = \text{dividends}
$$

- Avoids reliance on adjusted prices  
- Ensures time-additivity  
- Preserves economic interpretability  

---

## Investable Universe

The daily universe is filtered to ensure implementability:

$$
\frac{1}{10} \sum_{k=0}^{9} AMO_{t-k,i} > 10^8
$$

$$
ListingPeriod_{t,i} \geq 60
$$

This mitigates microstructure noise and capacity constraints.

---

## Feature Engineering

Core operators:

$$
\text{rank}(x_{t,\cdot})
$$

$$
\text{delay}_k(x_t) = x_{t-k}
$$

$$
\text{corr}_L(x, y)
$$

$$
\text{neutralize}(x)
$$

These operators standardize signals, encode temporal structure, and remove systematic biases.

---

## Alpha Generation

- 96 formulaic alphas implemented ( 6 were delay 0)  
- Restricted to delay-1 variants  

Signal families include:
- Momentum / reversal  
- Volatility / dispersion  
- Volume / liquidity  

---

## Alpha Evaluation

### Information Coefficient (IC)

$$
IC_t = \text{corr}_{i \in U_t}(a_{t-1,i}, r_{t,i})
$$

### Rank IC

$$
\text{Rank IC}_t = \text{corr}_{i \in U_t}(\text{rank}(a_{t-1,i}), \text{rank}(r_{t,i}))
$$

Typical signal strength:

$$
IC \approx 0.01 - 0.02
$$

---

## Statistical Validation (HAC)

$$
\hat{\Omega}_{NW} = \hat{\gamma}_0 + 2 \sum_{l=1}^{L} w_l \hat{\gamma}_l
$$

$$
w_l = 1 - \frac{l}{L+1}
$$

This produces consistent standard errors under autocorrelation and heteroskedasticity.

---

## Factor Pricing (Fama–MacBeth)

$$
r_t = X_t \beta_t + \epsilon_t
$$

$$
\bar{\beta} = \frac{1}{T} \sum_{t=1}^{T} \hat{\beta}_t
$$

$$
X_t = \text{factor exposures}
$$

$$
\beta_t = \text{factor returns}
$$

---

## Alpha Selection

Filtering pipeline:

$$
\text{Alpha} \rightarrow \text{HAC} \rightarrow \text{IC Filter} \rightarrow \text{Correlation Filter}
$$

---

## Portfolio Construction

### Signal Aggregation

Signals are combined using IC-based scaling and normalization.

### Softmax Weighting

$$
w_i = \frac{e^{s_i}}{\sum_j e^{s_j}}
$$

---

## Performance

| Metric | Value |
|------|------|
| Start NAV | 100,000,000 |
| End NAV | 142,749,181 |
| Total Return | 42.75% |
| Annualized Return | 42.89% |
| Volatility | 28.85% |
| Sharpe Ratio | 1.49 |
| Max Drawdown | -24.99% |

---

## Key Findings

- Only a small subset of alphas survives robust testing  
- Signals are weak individually but strong in aggregation  
- Correlation control improves portfolio efficiency  
- Volatility significantly impacts performance  

---

## Limitations
  
- Potential multiple testing bias  
- No regime segmentation  
- Execution constraints not modeled  

---

## Tech Stack

- Python: Pandas, NumPy, SciPy  
- Data: Bloomberg, Polygon, Yfinace
- Visualization: Matplotlib, Seaborn  

---

## Contact

If you have questions about this project or are interested in collaborating on quantitative research, trading strategies, or data-driven investment projects, feel free to reach out.

I am always open to discussing ideas, refining models, and working on impactful projects in quantitative finance.

- **Name:** Ryan Landey  
- **Email:** rjlandey@ncsu.edu  

Looking forward to connecting and building together.
