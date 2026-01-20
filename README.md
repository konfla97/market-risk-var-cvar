### Market Risk Case Study: Historical VaR & CVaR (ETF Portfolio)

### Objective
Estimate **1-day market risk** for a diversified ETF portfolio using **Historical Value at Risk (VaR)** and **Conditional Value at Risk (CVaR / Expected Shortfall)**.  
This project is designed as a practical market-risk case study with clear assumptions, reproducible code, and transparent risk interpretation.

---

### Business Questions
1. What is the portfolio’s **1-day 95% and 99% VaR**?
2. What is the portfolio’s **Expected Shortfall (CVaR)** beyond VaR?
3. How do tail-risk measures compare to the **worst historical outcomes**?
4. How would the portfolio behave under **stress scenarios**? *(next step)*

---

### Portfolio
ETFs used (multi-asset diversification):

- **SPY** (S&P 500 / US equities)
- **QQQ** (NASDAQ / growth equities)
- **IWM** (Russell 2000 / small caps)
- **TLT** (US Treasuries / duration exposure)
- **GLD** (Gold)
- **HYG** (High-yield credit)

Weights (fixed, static):

| Ticker | Weight |
|-------:|-------:|
| SPY | 0.20 |
| QQQ | 0.20 |
| IWM | 0.15 |
| TLT | 0.20 |
| GLD | 0.15 |
| HYG | 0.10 |

---

### Data
- Source: **Stooq** (daily prices)
- Frequency: **Daily**
- Period: from **2015-01-01** to most recent available date
- Price field used: daily **Close** (stored as `adj_close`)

Saved dataset:
- `data/prices.csv` with columns: `date`, `ticker`, `adj_close`

---

### Methodology
1. Download daily prices for the selected ETFs.
2. Compute **daily arithmetic returns** per asset.
3. Compute portfolio daily return:
   \[
   R_{p,t} = \sum_i w_i R_{i,t}
   \]
4. Convert returns to **losses**:
   \[
   L_t = -R_{p,t}
   \]
5. Estimate Historical risk measures:
   - **VaR (α)**: α-quantile of the loss distribution  
   - **CVaR (α)**: average loss conditional on losses exceeding VaR

Confidence levels:
- **95%**
- **99%**

---

### Results (Historical)
Based on the historical loss distribution, the estimated risk measures are:

- **Historical 95% VaR:** ~ **1.09%**
- **Historical 95% CVaR:** ~ **1.75%**
- **Historical 99% VaR:** ~ **1.97%**
- **Historical 99% CVaR:** ~ **2.98%**

Interpretation:
- VaR represents a **loss threshold**, not a worst-case outcome.
- CVaR is materially larger than VaR, indicating **meaningful tail risk**.
- Extreme historical losses exceed VaR, highlighting the limitations of threshold-based risk measures.

---

### Discussion
Historical VaR is intuitive and assumption-free but **backward-looking** and dependent on the chosen sample period.  
For robust market-risk assessment, VaR and CVaR should be complemented with **stress testing** and scenario analysis.

---

### Project Structure

```text
market-risk-var-cvar/
│
├── data/
│   └── prices.csv              # Daily ETF prices (date, ticker, adj_close)
│
├── notebook/
│   └── market_risk_var_cvar.ipynb
│
├── README.md                   # Project overview, methodology, results
└── requirements.txt            # Python dependencies
```
