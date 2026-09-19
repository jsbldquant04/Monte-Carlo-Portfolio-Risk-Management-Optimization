# Monte Carlo Portfolio Risk Management & Optimization

## Overview

This project uses **Monte Carlo simulation** to measure portfolio tail risk using **Value at Risk (VaR)** and **Expected Shortfall (ES)**.

### Business Question

> How much could the portfolio lose, which assets drive the risk, and how can the allocation be adjusted to reduce potential losses?

## Portfolio

Hypothetical **₱1 billion portfolio**:

| Asset | Weight |
| ----- | -----: |
| SPY   |    40% |
| TLT   |    25% |
| GLD   |    15% |
| QQQ   |    20% |

## Methodology

1. Download historical market data using `yfinance`
2. Calculate daily returns
3. Estimate the covariance matrix
4. Generate 500,000 Monte Carlo scenarios
5. Calculate 99% VaR and ES
6. Measure portfolio risk contribution
7. Run a stress scenario
8. Optimize the portfolio allocation
9. Compare the original and optimized portfolios

## Risk Metrics

### Value at Risk

VaR estimates a potential loss threshold at a chosen confidence level.

At 99% confidence, the VaR represents a loss level exceeded in approximately 1% of simulated scenarios.

### Expected Shortfall

ES measures the **average loss in the worst 1% of scenarios**.

This provides additional information about the severity of extreme losses.

## Portfolio Optimization

The objective is to minimize 99% Expected Shortfall:

```text
Minimize ES(99%)
```

Subject to:

```text
Sum of portfolio weights = 100%

5% <= each asset weight <= 60%
```

The resulting portfolio is called an **ES-minimizing allocation**, rather than universally "the optimal portfolio."

## Business Applications

This analysis can support:

* Risk-limit monitoring
* Portfolio allocation
* Position sizing
* Hedging
* Capital planning
* Stress testing
* Tail-risk management

## Visualizations

The project includes:

* Monte Carlo loss distribution
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

This project demonstrates how **Monte Carlo simulation can transform statistical risk estimates into practical portfolio risk-management decisions**.
