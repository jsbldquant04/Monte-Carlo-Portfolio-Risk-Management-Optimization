# Monte Carlo Portfolio Risk Management & Optimization

## Overview

This project uses **Monte Carlo simulation** to measure and manage portfolio tail risk using **Value at Risk (VaR)** and **Expected Shortfall (ES)**.

The main business question is:

> **How much could the portfolio lose, which assets drive the risk, and how can the allocation be adjusted to reduce potential losses?**

## Portfolio

A hypothetical **₱1 billion portfolio** containing:

| Asset | Initial Weight |
| ----- | -------------: |
| SPY   |            40% |
| TLT   |            25% |
| GLD   |            15% |
| QQQ   |            20% |

## Methodology

1. Download historical market data using `yfinance`
2. Calculate daily asset returns
3. Estimate the covariance matrix
4. Generate **500,000 Monte Carlo scenarios**
5. Calculate:

   * 99% VaR
   * 99% Expected Shortfall
   * Portfolio risk contribution
6. Apply a stress scenario
7. Optimize portfolio weights to minimize **99% Expected Shortfall**
8. Compare the original and optimized allocations

## Risk Metrics

### Value at Risk

VaR estimates a loss threshold at a chosen confidence level.

> At 99% confidence, VaR estimates a loss level that is exceeded in approximately 1% of simulated scenarios.

### Expected Shortfall

ES measures the **average loss in the worst 1% of scenarios**.

This makes ES useful for understanding the severity of extreme losses.

## Portfolio Optimization

The project minimizes:

$$
\min_w ES_{99\%}(w)
$$

subject to:

$$
\sum_i w_i = 1
$$

and

$$
5\% \leq w_i \leq 60\%
$$

The result is described as an **ES-minimizing allocation**, rather than universally "the optimal portfolio."

## Business Applications

The analysis can support:

* Risk-limit monitoring
* Portfolio allocation
* Position sizing
* Hedging decisions
* Capital planning
* Stress testing
* Tail-risk management

## Visualizations

The project includes:

* Simulated loss distribution
* Correlation matrix
* Risk contribution
* Original vs. optimized allocation
* VaR and ES comparison
* Portfolio risk landscape

## Tech Stack

* Python
* NumPy
* Pandas
* SciPy
* Matplotlib
* Seaborn
* yfinance

## Key Takeaway

This project demonstrates how **Monte Carlo simulation can transform statistical estimates into practical portfolio risk-management decisions**.
