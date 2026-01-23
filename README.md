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

   `R_p,t = Σ_i w_i · R_i,t`

4. Convert returns to **losses**:

   `L_t = − R_p,t`

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

### Stress Testing & Scenario Analysis

Historical VaR and CVaR quantify tail risk under *normal market conditions*, but they do not fully capture portfolio behavior during **extreme or persistent stress events**.  
To address this limitation, we complement VaR/CVaR with **historical stress testing** based on realized market crises and policy shocks.

The following stress tests are considered:

---

### Stress Test A — Worst Historical Single-Day Shocks

This stress test identifies the **worst single-day portfolio losses** observed in the historical sample.

Procedure:
- Rank portfolio daily losses from largest to smallest
- Inspect the top worst-loss days
- Decompose the worst day into **asset-level contributions**

Key findings:
- The worst historical day occurred on **2020-03-12** (COVID market panic)
- Portfolio loss on that day: **6.28%**
- Losses were driven primarily by equity ETFs (SPY, QQQ, IWM)
- Defensive assets (TLT, GLD) partially offset equity losses but could not prevent a large drawdown

This analysis highlights that **VaR thresholds can be breached during crisis days**, reinforcing the need for stress testing.

---

### Stress Test B — COVID Crisis Window (Multi-Day Systemic Stress)

This scenario evaluates portfolio performance during the **COVID market crisis**, capturing the effect of *persistent losses over multiple days* rather than a single shock.

Stress window:
- February–March 2020 (COVID-driven market selloff)

Metrics evaluated:
- Worst single-day loss
- Cumulative portfolio return
- Maximum drawdown (peak-to-trough)

Results:
- Worst daily loss: **6.28%**
- Cumulative return: **−12.33%**
- Maximum drawdown: **−20.26%**

Interpretation:
- Although the worst daily loss matches the historical worst day, cumulative losses were substantially larger
- Drawdown continued to deepen after the initial shock due to consecutive negative returns
- This demonstrates that **path dependency and recovery dynamics** are critical components of risk

COVID illustrates a **systemic stress regime** where losses compound over time.

---

### Stress Test C — Tariff Shock (April 2025 Policy Event)

This scenario examines a **policy-driven stress event** following the announcement of new US trade tariffs in early April 2025.

Stress window:
- **2025-04-02 to 2025-04-10**

Metrics evaluated:
- Worst single-day loss
- Cumulative portfolio return
- Maximum drawdown

Results:
- Worst daily loss: **3.36%**
- Cumulative return: **−4.75%**
- Maximum drawdown: **−8.76%**

Interpretation:
- The tariff shock produced smaller losses than the COVID crisis
- Losses were spread across several days rather than concentrated in a single crash
- Partial recoveries occurred within the window, limiting drawdown depth

This scenario reflects **event-driven risk**, which is typically less severe than systemic crises but still material.

---

### Stress Scenario Comparison

| Scenario | Worst daily loss | Cumulative return | Maximum drawdown |
|--------|-----------------|-------------------|------------------|
| Historical worst (single day) | 6.28% | −6.28% | 6.28% |
| COVID crisis window | 6.28% | −12.33% | −20.26% |
| Tariff shock (Apr 2025) | 3.36% | −4.75% | −8.76% |

Key insights:
- **Worst daily loss alone does not capture total risk**
- Systemic crises (COVID) cause significantly larger drawdowns than isolated shocks
- Policy-driven shocks are meaningful but less destructive than global crises
- VaR captures threshold risk, while stress testing reveals **loss persistence and recovery risk**

---

### Risk Management Implications

- VaR and CVaR should be interpreted as **baseline risk measures**
- Stress testing is essential to understand portfolio behavior under extreme conditions
- Maximum drawdown provides critical insight into **capital erosion and investor survivability**
- Combining VaR, CVaR, and stress testing yields a **more complete risk framework**


---

### Project Structure
---

### Project Structure

```text
market-risk-var-cvar/
│
├── data/
│   └── prices.csv
│       # Daily ETF prices
│       # Columns: date, ticker, adj_close
│
├── notebook/
│   ├── market_risk_var_cvar.ipynb
│   │   # Historical VaR & CVaR estimation
│   │   # Portfolio construction and baseline risk analysis
│   │
│   └── stress_testing.ipynb
│       # Stress Test A: Worst historical single-day shocks
│       # Stress Test B: COVID crisis (multi-day systemic stress)
│       # Stress Test C: Tariff shock (April 2025 policy event)
│       # Drawdowns, cumulative returns, and scenario comparison
│
├── README.md
│   # Project overview, methodology, results, and interpretation
│
└── requirements.txt
│   # Python dependencies (pandas, numpy, matplotlib, etc.)

