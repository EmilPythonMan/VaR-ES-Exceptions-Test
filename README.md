# VaR-ES-Back-Test
Tests the historical VaR and ES breaches of a portfolio of EUR denominated US Equities, Japan Equities, US Treasuries and EM Bonds. Tests for probability of  VaR exceptions (Kupiec Test), independence of VaR exceptions (Christoffersen Test) and conditional VaR coverage. Also test the observed ES against simulated values with Acerbi-Szekely Test. 

## Setup
 - Market Data from 2010-07-23 to 2026-09-28 loaded using yfinance in Python.
 - Portfolio consisting of S&P 500 (^GSPC), Nikkei 225 (^N225), iShares 7-10 Year Treasury Bond ETF (IEF), and VanEck JP Morgan EM Local Currency Bond ETF (EMLC). All denominated in EUR.
 - Portfolio weights selected to give each asset a portfolio value share of 25% at inception. Weights(^GSCP, ^N225, IEF, EMLC) = (0.01241, 0.126648, 0.209178, 0.651764).

## Workflow
 1. Load data from Yahoo Finance using yfinance
 2. Calculate portfolio value in EUR for each historical date.
 3. Calculate daily percentage returns.
 4. For each date, calculate the rolling VaR and ES using the preceding 250 days.
 5. Compare the VaR and ES with the realized returns and count exceptions of both measures.
 6. Using the assigned probability level for VaR (typically 1%), perform Kupiec test for the observed probabilities.
 7. Using the realized returns and rolling VaRs, perform the Christoffersen Test for independence of exceptions.
 8. Perform the conditional coverage test with the sum of the Kupiec and Christoffersen test statistics.
 9. Perform the Acerbi-Szekely test using the predicted VaRs and ESs, by fitting a GARCH(1,1) model with student t innovations on the daily returns to calculate the distribution of the test statistic.

## Results
**Figure 1: PF VaR and ES over time**

<img width="585" height="435" alt="image" src="https://github.com/user-attachments/assets/bc3dfcd4-0e4c-4f96-8287-f18ae734caae" />

This plot shows the rolling VaR and ES value of the portfolio over time.

**Figure 2: Cumulative number of PF VaR exceptions**

<img width="585" height="435" alt="image" src="https://github.com/user-attachments/assets/763bdbb7-0d6b-4f81-ac2e-fb57f755c7e5" />

This plot shows the cumulative number of exceptions of the PF VaR and ES over time. The total VaR exceptions were 48 and the total ES exceptions were 44.

The hypotheses testing resulted in the following p-values.

| Type | Kupiec Test p-value | Christoffersen Test p-value | Conditional Coverage Test p-value | Acerbi-Szekely Test p-value |
| :--- | :--- | :--- | :--- | :--- |
| VaR | 0.1987 | 0.0025 | 0.0046 | --- |
| ES | --- | --- | --- | 0.1449 |

Based on the p-value and a significance level of 5%, we get the following hypotheses rejections/acceptances
| Type | Kupiec Test | Christoffersen Test | Conditional Coverage Test | Acerbi-Szekely Test | 
| :--- | :--- | :--- | :--- | :--- |
| VaR | Rejection | No Rejection | Rejection |
| ES | --- | --- | --- | No Rejection |

## Conclusion
 - We reject the hypothesis that the model has a VaR exception probability equal to 1%.
 - We do not reject the hypothesis of independence between exceptions for VaR.
 - We reject the hypotheses of unconditional coverage for VaR, and the model is therefore statistically bad at predicting future extreme losses. 
 - We do not reject the hypothesis that average losses on VaR exception days match the predicted ES values.

## Disclaimer
 - Portfolio weights were not adjusted over time leading to ballooning portfolio value shares of equities.
