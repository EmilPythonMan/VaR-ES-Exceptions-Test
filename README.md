# VaR-ES-Exceptions-Test
Tests the historical VaR and ES breaches of a portfolio of EUR denominated US Equities, Japan Equities, US Treasuries and EM Bonds. Tests for probability of exceptions (Kupiec Test), independence of exceptions (Christoffersen Test) and conditional coverage.

## Setup
 - Market Data from 2010-07-23 to 2026-09-28 loaded using yfinance in Python.
 - Portfolio consisting of S&P 500 (^GSPC), Nikkei 225 (^N225), iShares 7-10 Year Treasury Bond ETF (IEF), and VanEck JP Morgan EM Local Currency Bond ETF (EMLC). All denominated in EUR.
 - Portfolio weights selected to give each asset a portfolio value share of 25% at inception. Weights(^GSCP,^N225,IEF,EMLC) = (0.014158,0.001655,0.235923,0.748264).

## Workflow
 1. Load data from Yahoo Finance using yfinance
 2. Calculate portfolio value in EUR for each historical date.
 3. Calculate daily percentage returns.
 4. For each date, calculate the rolling VaR and ES using the preceding 250 days.
 5. Compare the VaR and ES with the realized returns and count exceptions of both measures.
 6. Using the assigned probability levels for VaR and ES (typically 1% and 2.5% respectively), perform Kupiec test for the observed probabilities.
 7. Using the realized returns and rolling VaRs and ESs, perform the Christoffersen Test for independence of exceptions.
 8. Perform the conditional coverage test with the sum of the Kupiec and Christoffersen test statistics.

## Results
**Figure 1: PF VaR and ES over time**
<img width="576" height="435" alt="image" src="https://github.com/user-attachments/assets/8757ea10-91cd-4128-8904-6bac35d9718d" />

This plot shows the rolling VaR and ES value of the portfolio over time.

**Figure 2: Cumulative number of PF VaR and ES exceptions**
<img width="545" height="435" alt="image" src="https://github.com/user-attachments/assets/3197a78b-23e3-44af-80eb-c0ed338e70d9" />

This plot whos the cumulative number of exceptions of the PF VaR and ES. The total VaR exceptions were 54 and the total ES exceptions were 47.

The hypotheses testing resulted in the following p-values.

| Type | Kupiec Test p-value | Christoffersen Test p-value | Conditional Coverage Test p-value |
| :--- | :--- | :--- | :--- |
| VaR | 0.0303 | 0.2154 | 0.0445 |
| ES | 3.499 * 10^-9 | 0.5882 | 2.298 * 10^-8 |

Based on the p-value and a significance level of 5%, we get the following hypotheses rejections/acceptances
| Type | Kupiec Test | Christoffersen Test | Conditional Coverage Test |
| :--- | :--- | :--- | :--- |
| VaR | Rejection | No Rejection | Rejection |
| ES | Rejection | No Rejection | Rejection |

## Conclusion
 - We reject the hypotheses that the VaR and ES have exception probabilities equal to 1% and 2.5% respectively.
 - We do no reject the hypotheses of independence between exceptions for both VaR and ES.
 - We reject the hypotheses of unconditional coverage for both VaR and ES, and the models are therefore statistically bad at predicting future losses. 
