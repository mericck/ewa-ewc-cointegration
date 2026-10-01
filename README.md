# ewa-ewc-cointegration
Testing whether the historical EWA–EWC cointegration relationship persists in recent markets using Engle–Granger and Johansen tests
# Revisiting EWA–EWC Cointegration

## Overview

This project investigates whether the historically documented cointegration relationship between the Australian and Canadian equity markets has persisted in recent years.

The analysis is motivated by the EWA–EWC pair discussed in Ernest P. Chan's *Algorithmic Trading*, where the two ETFs were presented as an example of a cointegrated pair.

Rather than assuming that a historically observed statistical relationship remains stable, this project revisits the pair using recent market data from 2022 to 2026.

The main research question is:

> **Does the historical EWA–EWC cointegration relationship still hold in the recent market regime?**

---

## Economic Motivation

EWA (iShares MSCI Australia ETF) and EWC (iShares MSCI Canada ETF) provide exposure to the Australian and Canadian equity markets.

Australia and Canada share several macroeconomic characteristics. Both economies have meaningful exposure to commodities, natural resources, and resource related industries.

This provides an economic rationale for investigating whether their equity markets may share a stable long-run relationship.

Importantly, the pair was selected based on this economic hypothesis and its historical use as a cointegration example rather than through a large scale search for statistically significant pairs.

---

## Data

Daily adjusted price data for:

- **EWA** — iShares MSCI Australia ETF
- **EWC** — iShares MSCI Canada ETF

Sample period:

**January 2022 – August 2026**

Data was downloaded using `yfinance`.

The analysis was conducted in Python using:

- `pandas`
- `numpy`
- `matplotlib`
- `yfinance`
- `statsmodels`

---

## Methodology

The analysis follows a cointegration-testing workflow inspired by Ernest Chan's *Algorithmic Trading*.

### 1. Price Relationship

The adjusted prices of EWA and EWC are first inspected to understand their recent co-movement.

Visual co-movement or high correlation alone does not establish cointegration. Two non-stationary price series can move together and still produce a spurious relationship.

### 2. OLS Regression

The first relationship is estimated as:


EWC_t = α + β EWA_t + ε_t


OLS estimates:


α = -22.5667
β = 2.7284
R² = 0.918


The estimated residual spread is:


ε_t = EWC_t - α - β EWA_t


A high R² is not interpreted as evidence of cointegration. The relevant question is whether the residual series is stationary.

### 3. Engle–Granger / CADF Test

A residual-based cointegration test is applied to:


EWC ~ EWA


Result:


CADF Statistic: -2.8961
p-value:         0.1370
5% Critical:    -3.3414


At the 5% significance level, the null hypothesis of no cointegration cannot be rejected.

### 4. Reverse Regression

Because the Engle–Granger procedure is asymmetric, the regression direction is reversed:


EWA ~ EWC


Result:


CADF Statistic: -3.2060
p-value:         0.0689
5% Critical:    -3.3414
10% Critical:   -3.0481


The null hypothesis cannot be rejected at the 5% level, but it can be rejected at the 10% level.

This demonstrates the sensitivity of the Engle–Granger approach to regression direction.

### 5. Johansen Cointegration Test

The Johansen test is then used as a system-based approach to examine the relationship without choosing one ETF as the dependent variable.

The specification follows the lag and deterministic setup used in the reference example.

#### Trace Test


r = 0 statistic: 14.3796

90% critical value: 13.4294
95% critical value: 15.4943
99% critical value: 19.9349


#### Maximum Eigenvalue Test


r = 0 statistic: 12.7337

90% critical value: 12.2971
95% critical value: 14.2639
99% critical value: 18.5200


Both Johansen statistics reject the no-cointegration hypothesis at the **10% level**, but not at the **5% level**.



## Results

<img width="960" height="273" alt="image" src="https://github.com/user-attachments/assets/4159a102-193d-4fc5-ae75-7a53b01680c0" />


At the conventional **5% significance level**, the recent sample does not provide sufficient evidence that EWA and EWC remain cointegrated.

However, the reverse Engle–Granger specification and both Johansen statistics provide weaker evidence of cointegration at the **10% significance level**.

---

## Interpretation

The results suggest that the EWA–EWC relationship during 2022–2026 is weaker than the historical relationship documented in the reference example.

An important aspect of the experiment is the difference between the two Engle–Granger regression directions.

The original specification produced:


p = 0.137


while reversing the dependent and independent variables produced:


p = 0.069


This provides a practical example of why the Engle–Granger two-step procedure can be sensitive to regression direction.

The Johansen test provides additional evidence consistent with a weaker or borderline long-run relationship: both the Trace and Maximum Eigenvalue tests reject no cointegration at 10%, but not at 5%.

Using a pre-defined 5% significance threshold, I therefore do **not** treat the pair as sufficiently cointegrated to proceed directly to a mean-reversion trading strategy.

The significance threshold is not relaxed simply because the results are close to statistical significance.



## Key Takeaways

This project highlights several important lessons in quantitative research:

- Economic intuition can motivate a statistical hypothesis, but does not prove it.
- Correlation and high OLS R² do not imply cointegration.
- Historical relationships should not be assumed to persist across market regimes.
- Engle–Granger cointegration results can depend on regression direction.
- Johansen testing provides a system-based alternative for studying cointegrating relationships.
- Statistical thresholds should be defined consistently rather than adjusted after observing results.
- A research process does not need to produce a trading signal to produce a useful result.



## Limitations

This is a small empirical research project rather than a complete trading strategy.

Several limitations remain:

- The analysis covers only the 2022–2026 sample.
- Structural breaks and time-varying cointegration are not explicitly modeled.
- The Johansen specification follows the reference setup rather than performing a full lag-order and deterministic-term selection process.
- Transaction costs, shorting costs, execution constraints, and portfolio construction are not modeled.
- No trading strategy is backtested because the pair did not meet the pre-defined 5% cointegration criterion.

These areas could be investigated in future research.



## Reference

Ernest P. Chan, *Algorithmic Trading: Winning Strategies and Their Rationale*, Wiley.

The EWA–EWC pair and the motivation for comparing CADF and Johansen cointegration tests were inspired by the examples presented in the book.



## Disclaimer

This project is for educational and research purposes only and does not constitute investment advice.
