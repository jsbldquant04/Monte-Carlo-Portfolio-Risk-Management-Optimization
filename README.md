# Monte Carlo Portfolio Risk Management & Optimization

## Overview

This project develops a quantitative portfolio risk-management and optimization framework using historical financial data, Monte Carlo simulation, Value at Risk (VaR), Expected Shortfall (ES), stress testing, and constrained portfolio optimization.

The central business question is:

> **Given a multi-asset portfolio, how much could we lose under adverse market conditions, which assets are driving portfolio risk, and how can portfolio allocation be adjusted to reduce tail risk?**

The project uses a hypothetical **₱1 billion portfolio** containing equities, long-term bonds, gold, and technology equities.

The framework goes beyond simply calculating VaR. It connects statistical modeling to portfolio-management decisions by evaluating how different allocations affect potential losses and tail risk.

---

# Business Problem

Financial institutions, investment managers, and risk teams need to continuously answer questions such as:

* How much could the portfolio lose in a single day?
* How severe could losses become during extreme market conditions?
* Which assets contribute most to portfolio risk?
* Does portfolio diversification actually reduce tail risk?
* Is the portfolio within its predefined risk limit?
* How would changing the asset allocation affect risk?
* Can we identify an allocation that minimizes extreme losses subject to investment constraints?

A portfolio's expected return alone cannot answer these questions.

This project therefore models the **distribution of potential portfolio losses** and uses that distribution to support quantitative risk analysis.

---

# Project Objectives

The project has six primary objectives:

1. Estimate portfolio risk from historical market data.
2. Simulate thousands of possible future market scenarios using Monte Carlo simulation.
3. Calculate Value at Risk and Expected Shortfall.
4. Identify which assets contribute most to portfolio risk.
5. Evaluate portfolio losses under a severe stress scenario.
6. Find an allocation that minimizes 99% Expected Shortfall subject to portfolio constraints.

---

# Portfolio

The example portfolio has a total value of:

```text
₱1,000,000,000
```

The initial allocation is:

| Asset | Description         | Initial Weight |
| ----- | ------------------- | -------------: |
| SPY   | US Equities         |            40% |
| TLT   | Long-Term Bonds     |            25% |
| GLD   | Gold                |            15% |
| QQQ   | Technology Equities |            20% |

The portfolio weights sum to:

$$
\sum_{i=1}^{n}w_i=1
$$

---

# Methodology

The complete workflow is:

```text
Historical Market Data
          │
          ▼
     Daily Returns
          │
          ▼
 Mean & Covariance Estimation
          │
          ▼
 Monte Carlo Simulation
          │
          ▼
 Portfolio Loss Distribution
          │
     ┌────┴────┐
     ▼         ▼
    VaR        ES
     │         │
     └────┬────┘
          ▼
   Risk Attribution
          │
          ├───────────────┐
          ▼               ▼
   Stress Testing   Portfolio Optimization
                          │
                          ▼
                  Risk-Minimizing Allocation
                          │
                          ▼
                   Business Analysis
```

---

# 1. Market Data

Historical adjusted closing prices are obtained for the portfolio assets.

Daily returns are calculated as:

$$
R_t = \frac{P_t-P_{t-1}}{P_{t-1}}
$$

where:

* \(P_t\) is the current price
* \(P_{t-1}\) is the previous price

The analysis then estimates:

* average returns
* volatility
* correlations
* covariance

---

# 2. Portfolio Returns

The portfolio return is calculated using:

$$
R_p = \sum_{i=1}^{n}w_iR_i
$$

or in matrix notation:

$$
R_p=w^TR
$$

where:

* \(w\) represents portfolio weights
* \(R\) represents asset returns

This allows individual asset movements to be combined into a portfolio-level outcome.

---

# 3. Covariance Matrix

The covariance matrix captures how assets move together.

Portfolio variance is:

$$
\sigma_p^2=w^T\Sigma w
$$

where:

* \(w\) = portfolio weights
* \(\Sigma\) = covariance matrix

This is important because portfolio risk depends not only on individual asset volatility but also on **relationships between assets**.

Two highly volatile assets can still produce a relatively diversified portfolio if their returns are sufficiently weakly correlated.

---

# 4. Value at Risk

Value at Risk estimates a loss threshold at a specified confidence level.

For example:

```text
99% One-Day VaR = ₱30 million
```

means that, under the model, approximately 1% of one-day outcomes have losses greater than ₱30 million.

VaR therefore provides a useful threshold for:

* risk monitoring
* exposure limits
* portfolio reporting
* capital planning
* risk escalation

---

# 5. Expected Shortfall

Expected Shortfall goes beyond VaR.

Instead of asking:

> "Where does the extreme-loss region begin?"

ES asks:

> **"Once we enter that extreme-loss region, how large is the average loss?"**

For example:

```text
99% VaR = ₱30M
99% ES  = ₱45M
```

The interpretation is that the average loss among the worst 1% of modeled outcomes is approximately ₱45 million.

This makes ES particularly useful for analyzing **tail risk**.

---

# 6. Monte Carlo Simulation

The project generates hundreds of thousands of simulated asset-return scenarios.

The simulation uses estimated:

* mean returns
* covariance
* correlations

to generate correlated asset returns.

For every simulated scenario:

$$
R_p=w^TR
$$

The resulting portfolio return is converted into a monetary loss:

$$
L=-R_pV
$$

where:

* \(L\) = portfolio loss
* \(R_p\) = portfolio return
* \(V\) = portfolio value

The result is a simulated distribution of potential portfolio losses.

---

# 7. Risk Distribution

The Monte Carlo simulation produces a large distribution of potential losses.

Conceptually:

```text
                    Portfolio Loss Distribution

                       Normal outcomes
                            │
                            ▼
        ┌─────────────────────────────────────┐
        │                                     │
        │        █████████████████            │
        │      █████████████████████          │
        │    █████████████████████████        │
        │  █████████████████████████████      │
        │ ███████████████████████████████     │
        └─────────────────────────────────────┘
                                      │
                                      ▼
                              Extreme Loss Tail
                                      │
                              ┌───────┴──────┐
                              ▼              ▼
                             VaR             ES
```

The extreme left/right tail, depending on whether returns or losses are plotted, is particularly important for risk management.

---

# 8. Risk Contribution

Portfolio allocation and portfolio risk contribution are not necessarily the same.

For example:

```text
Portfolio allocation:
Asset A = 20%

Risk contribution:
Asset A = 35%
```

An asset can represent a relatively small portion of the portfolio while contributing disproportionately to risk because of:

* high volatility
* strong correlations
* covariance with other portfolio positions

The project therefore calculates asset-level risk contribution to identify potential sources of portfolio risk.

---

# 9. Stress Testing

Monte Carlo simulation estimates a distribution of outcomes based on statistical assumptions.

However, risk managers also need to ask:

> **"What happens under a specific severe market scenario?"**

The project therefore includes a hypothetical stress scenario involving simultaneous adverse movements across several assets.

For example:

```text
SPY  → -15%
TLT  →  -5%
GLD  →  +5%
QQQ  → -20%
```

The resulting portfolio loss is calculated as:

$$
L_{stress}
=
-\left(\sum_i w_ir_i^{stress}\right)V
$$

Stress testing provides a scenario-based complement to probabilistic risk measures such as VaR and ES.

---

# 10. Portfolio Optimization

The project then moves from **risk measurement** to **risk management**.

The optimization objective is:

> **Minimize 99% Expected Shortfall.**

Mathematically:

$$
\min_w ES_{99\%}(w)
$$

subject to:

$$
\sum_iw_i=1
$$

and:

$$
0.05 \leq w_i \leq 0.60
$$

for each asset.

The constraints prevent the optimizer from producing unrealistic allocations.

The optimization therefore asks:

> **"Among portfolios satisfying our investment constraints, which allocation produces the lowest estimated 99% tail loss under the Monte Carlo model?"**

---

# 11. Why Optimize Expected Shortfall?

There are many possible portfolio objectives.

For example:

### Minimize volatility

$$
\min_w \sigma_p
$$

### Maximize Sharpe ratio

$$
\max_w
\frac{E[R_p]-R_f}{\sigma_p}
$$

### Minimize Value at Risk

$$
\min_w VaR_{99\%}
$$

### Minimize Expected Shortfall

$$
\min_w ES_{99\%}
$$

This project focuses on **Expected Shortfall** because the project is specifically concerned with extreme-loss behavior.

This does not mean an ES-minimizing portfolio is universally the best portfolio. The result depends on:

* the objective
* historical data
* simulation model
* confidence level
* investment constraints
* risk assumptions

Therefore, the project refers to the result as an **ES-minimizing allocation**, rather than simply "the optimal portfolio."

---

# 12. Original vs Optimized Portfolio

The optimization compares:

```text
Original Portfolio
       │
       ▼
Monte Carlo Risk
       │
       ▼
VaR / ES
       │
       ▼
Optimization
       │
       ▼
ES-Minimizing Allocation
       │
       ▼
Recalculate VaR / ES
       │
       ▼
Compare Risk
```

This allows us to measure whether the allocation change reduces modeled tail risk.

The analysis compares:

* original portfolio weights
* optimized portfolio weights
* original VaR
* optimized VaR
* original ES
* optimized ES

---

# 13. Portfolio Risk Landscape

The project also generates thousands of random portfolios.

Each portfolio is evaluated according to:

$$
VaR_{99\%}
$$

and:

$$
ES_{99\%}
$$

This produces a portfolio risk landscape.

Conceptually:

```text
Expected Shortfall
        │
        │          •
        │       •     •
        │    •    X
        │  •
        │ •
        └──────────────────────
                  VaR
```

The optimized portfolio is plotted alongside randomly generated portfolios to visualize its location in the risk space.

---

# 14. Key Visualizations

The notebook produces several visualizations.

### Asset Performance

Shows cumulative asset performance over the historical period.

### Correlation Matrix

Shows how assets have historically moved relative to one another.

### Monte Carlo Loss Distribution

Shows the distribution of simulated portfolio losses and identifies VaR and ES.

### Risk Contribution

Shows which assets contribute most to portfolio risk.

### Original vs Optimized Allocation

Shows how the optimization changes portfolio weights.

### Original vs Optimized Risk

Compares VaR and ES before and after optimization.

### VaR vs ES Portfolio Risk Landscape

Shows thousands of possible portfolios and identifies the ES-minimizing allocation.

### Stress Test

Quantifies the portfolio's loss under a predefined severe market scenario.

---

# Business Decision Framework

The project connects quantitative risk modeling to actual portfolio-management questions.

```text
             Market Data
                  │
                  ▼
          Statistical Model
                  │
                  ▼
         Monte Carlo Scenarios
                  │
                  ▼
           Loss Distribution
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
       VaR                  ES
        │                   │
        └─────────┬─────────┘
                  ▼
           Risk Attribution
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
  Stress Testing       Optimization
                              │
                              ▼
                     Alternative Allocation
                              │
                              ▼
                       Risk Comparison
                              │
                              ▼
                   Management Decision
```

The resulting analysis can support questions such as:

* Is the portfolio within its risk limit?
* Which positions are responsible for portfolio risk?
* How large could losses become during extreme scenarios?
* Does diversification reduce modeled tail risk?
* How does changing portfolio allocation affect VaR and ES?
* Should exposure or hedging strategies be reviewed?
* How much risk capital may be required under the chosen assumptions?

The model provides quantitative information for these decisions. It does not determine the appropriate business decision by itself.

---

# Technology Stack

### Programming

* Python

### Data Analysis

* NumPy
* Pandas

### Statistics

* SciPy

### Visualization

* Matplotlib
* Seaborn

### Market Data

* yfinance

### Quantitative Finance

* Portfolio Theory
* Monte Carlo Simulation
* Value at Risk
* Expected Shortfall
* Covariance Analysis
* Risk Attribution
* Stress Testing
* Portfolio Optimization

---

# Repository Structure

```text
monte-carlo-portfolio-risk/
│
├── README.md
│
├── monte_carlo_var_es.ipynb
│
├── src/
│   ├── data.py
│   ├── simulation.py
│   ├── risk_metrics.py
│   ├── optimization.py
│   └── stress_testing.py
│
├── figures/
│   ├── cumulative_returns.png
│   ├── correlation_matrix.png
│   ├── simulated_losses.png
│   ├── risk_contribution.png
│   ├── allocation_comparison.png
│   ├── risk_comparison.png
│   └── risk_landscape.png
│
└── requirements.txt
```

The complete implementation can initially be maintained in the Jupyter/Google Colab notebook. The `src/` structure can be introduced when the project is converted into a more production-oriented Python package.

---

# Limitations

This project is an educational quantitative-risk implementation rather than a production financial risk system.

The Monte Carlo framework relies on estimated historical relationships between assets.

Real financial markets can exhibit:

* fat-tailed returns
* volatility clustering
* regime changes
* changing correlations
* liquidity constraints
* nonlinear derivative exposures
* structural breaks
* model uncertainty

The basic multivariate-normal Monte Carlo model may therefore underestimate certain forms of extreme risk.

A production implementation could extend the framework using:

* Historical Simulation
* Filtered Historical Simulation
* GARCH volatility models
* Student-t distributions
* Extreme Value Theory
* regime-switching models
* Hidden Markov Models
* copula models
* stochastic volatility
* nonlinear derivative revaluation
* liquidity-adjusted VaR
* reverse stress testing
* VaR/ES backtesting
* model validation

---

# Potential Future Development

## Phase 1 — Current Project

```text
Historical Data
      ↓
Monte Carlo
      ↓
VaR / ES
      ↓
Risk Attribution
      ↓
Stress Testing
      ↓
ES Optimization
```

## Phase 2 — Advanced Risk Modeling

```text
Historical Simulation
        │
        ├── Monte Carlo
        ├── GARCH
        ├── Student-t
        ├── EVT
        └── HMM Regimes
                │
                ▼
          Model Comparison
                │
                ▼
           VaR / ES Backtest
```

## Phase 3 — Production Risk System

```text
Market Data
    ↓
Data Pipeline
    ↓
Risk Engine
    ↓
Portfolio Analytics
    ↓
VaR / ES
    ↓
Stress Testing
    ↓
Optimization
    ↓
Automated Risk Dashboard
```

---

# Conclusion

This project demonstrates how quantitative methods can transform financial-market data into actionable portfolio-risk information.

Rather than treating VaR and Expected Shortfall as isolated statistical calculations, the framework connects:

$$
\boxed{
\text{Market Data}
\rightarrow
\text{Simulation}
\rightarrow
\text{Risk Measurement}
\rightarrow
\text{Risk Attribution}
\rightarrow
\text{Stress Testing}
\rightarrow
\text{Portfolio Optimization}
}
$$

The central idea is that **risk modeling is ultimately useful because it supports decisions about portfolio exposure, diversification, risk limits, and capital allocation.**

The optimization component extends the project from:

> **"How risky is this portfolio?"**

to:

> **"How could we construct an alternative allocation that reduces modeled tail risk under explicit constraints?"**

That makes the project a practical demonstration of **Monte Carlo simulation, quantitative risk management, portfolio analytics, and constrained optimization in Python.**
