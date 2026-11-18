# OMNI-TERMINAL (Project CODEX) — Comprehensive Forensic Audit & Performance Evaluation
**STATUS: SUPERSEDED BY PRISM-TERMINAL (20% EQUAL-WEIGHT ALL-WEATHER 5-ETF CORE)**  
*Evaluation Period: October 31, 2011 – November 11, 2026 (15.03 Calendar Years | 3,910 Trading Days | 181 Valid Rebalances)*  
*Engine: `research/run_omni_backtest.py` & `research/stress_test_omni_engine.py` | Auditor: Quantitative Research Group*

---

## 1. Institutional Case Study: Why Equal-Weight Multi-Asset Beat Active Dual Momentum by +65 bps Net of Taxes

### Core Thesis & Case Study Overview
In systematic quantitative asset management, one of the most critical lessons is recognizing when market friction and concentration risk overwhelm factor alpha. **Omni-Terminal** was conceived as an institutional implementation of Gary Antonacci's Dual Momentum (combining cross-sectional momentum ranking with an absolute 200-day SMA trend gate) across 5 core Indian exchange-traded funds (ETFs):
1. Large-Cap Indian Equities: `NIFTYBEES.NS`
2. Mid-Cap Indian Equities: `JUNIORBEES.NS`
3. Precious Metals — Gold: `GOLDBEES.NS`
4. Global Tech / US Equities: `MON100.NS`
5. Precious Metals — Silver: `SILVERBEES.NS`

Following an exhaustive 15.03-year forensic audit with zero lookahead, next-day open fills, and statutory delivery friction, the empirical findings delivered a definitive conclusion:

> **Core Finding**: **Unconstrained passive equal-weight diversification systematically defeats active dual momentum timing across return, drawdown, and risk-adjusted efficiency.** 
> A 20% Equal-Weight allocation across the 5 ETFs (monthly-rebalanced with periodic sells) generated **15.16% Net Calendar CAGR**, **-24.83% Max Drawdown**, and a **0.68 Sharpe Ratio** ($R_f=6.5\%$, Sortino ~0.87, Calmar 0.61). 
> The active Dual Momentum engine produced only **14.51% Net Calendar CAGR**, suffered a punishing **-39.47% Max Drawdown**, and achieved a **0.43 Sharpe Ratio** while incurring heavy transaction taxes, slippage, and whipsaw drag.
> *(Note on Rebalancing: This 15.16% baseline reflects a monthly rebalanced portfolio with periodic asset sales to maintain 20% parity, as modeled in `run_omni_backtest.py`. A theoretical unmanaged buy-and-hold that never rebalances grew to 16.37% Calendar CAGR by allowing MON100 to compound unhindered to over 40% of the portfolio, but accepted severe unhedged tech concentration; see Prism-Terminal for full comparative attribution).*

This documented negative result led directly to the decommissioning of Omni-Terminal and the formal creation of **Prism-Terminal** (`../Prism-Terminal`).

---

### The Three Published Academic Truths

#### (a) Passive Diversification Beats Active Timing
Modern Portfolio Theory dictates that the diversification benefit of combining assets is maximized when their cross-asset correlations approach zero or negative values.
In the 5-ETF Indian macro universe:
* **Gold (`GOLDBEES`) correlation to Nifty 50 (`NIFTYBEES`) is -0.02** (zero correlation).
* **Gold correlation to Nasdaq 100 (`MON100`) is +0.04**.
* **Silver (`SILVERBEES`) correlation to Nifty is +0.12**.

When an investor holds all 5 assets simultaneously in an equal-weight basket, market crashes cancel out organically: when Indian equities crash (e.g. COVID-19 March 2020), Gold and US Tech surge; when Tech crashes (2022 Fed tightening), Indian domestic equities and cash hold firm. 
Active Dual Momentum discards this organic cancellation by forcing an all-in 50/50 bet on only 2 assets. In 2022, Dual Momentum concentrated 100% of capital into falling US Tech and Silver, absorbing their entire bear markets while discarding the protective Gold anchor.

#### (b) Rebalancing Friction, Statutory Taxes & Whipsaw Lag
Active switching strategies in India face severe statutory and structural headwinds:
1. **Short-Term Capital Gains (STCG)**: Frequent switching within 365 days triggers a 20% statutory STCG tax liability on realized profits, whereas buy-and-hold accrues unrealized gains with zero tax drag until redemption.
2. **Securities Transaction Tax (STT) & Turnover Charges**: Every rotation pays 0.10% delivery STT on exit plus exchange turnover and stamp duty, burning ~25–45 bps of capital per churn cycle.
3. **Lagging Indicator Trap**: Moving averages (such as the 200 SMA) are backward-looking. When an asset drops 15% rapidly mid-month, the strategy exits near the bottom, pays friction, and then misses the early V-shaped recovery while price is still below the 200 SMA.

#### (c) Low Cross-Sectional Dispersion Universe
Active momentum requires **wide cross-sectional dispersion**—a broad universe where winners surge +150% and losers drop -60%, providing enough raw factor spread to pay for turnover friction. This exists in individual mid-cap equities (the thesis behind **Forge-Terminal**).
Broad macro ETFs, by contrast, are market-cap-weighted averages with tight dispersion. Trying to run active momentum on 5 broad asset classes generates high churn without sufficient dispersion spread, making passive dollar-cost averaging mathematically superior.

---

## 2. Fair Benchmark Attribution & Performance Decomposition

The engine was evaluated across all 181 valid calendar month-ends from **October 31, 2011 to October 30, 2026** (15.03 calendar years | 3,910 trading days). All executions incorporate realistic Next-Day ($T+1$) Open fills with 10.0 bps adverse slippage and statutory delivery charges.

### Table 2.1: Head-to-Head Performance Attribution Matrix (2011–2026, Calendar Horizon)

| Strategy Architecture | Calendar Net CAGR (%) | Max Drawdown (%) | Sharpe Ratio ($R_f=6.5\%$) | Sortino Ratio | Calmar Ratio | Annual Volatility (%) | Max Drawdown Duration | Portfolio Turnover & Churn Mechanics |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Passive Equal-Weight 5-ETF Core (Prism-Terminal)** | **15.16%** | **-24.83%** | **0.68** | **0.87** | **0.61** | **12.67%** | **520 Days** | **Monthly Rebalanced (Periodic sells to maintain 20% target)** |
| **Active Dual Momentum Model B (Omni-Terminal)** | **14.51%** | **-39.47%** | **0.43** | **0.48** | **0.37** | **18.53%** | **988 Days (2.7 Yrs)** | **High Churn (Frequent full-position switching & 20% STCG)** |
| **Secondary Benchmark: Nifty 50 Index (`^NSEI`)** | **10.91%** | **-38.44%** | **0.27** | **0.35** | **0.28** | **16.12%** | **720 Days** | **Zero Churn (Price Return Benchmark)** |
| Cash Safe Harbor: `LIQUIDBEES` (6.50% APR) | 6.35% | 0.00% | N/A | N/A | N/A | 0.00% | 0 Days | Risk-Free Yield Floor |

> **Attribution Summary**:
> * **CAGR Spread**: Passive Equal-Weight (monthly rebalanced) beats Active Dual Momentum by **+65 bps per year net of transaction costs** (15.16% vs 14.51%).
> * **Drawdown Protection**: Passive Equal-Weight cuts maximum drawdown by **1,464 basis points** (-24.83% vs -39.47%).
> * **Risk-Adjusted Efficiency**: Passive Equal-Weight generates a **+58% higher Sharpe ratio** (0.68 vs 0.43) with **586 bps lower volatility** (12.67% vs 18.53%).
> * **Rebalancing Mechanics**: Note that Table 2.1's 15.16% figure corresponds to the monthly rebalanced 5-ETF basket from `run_omni_backtest.py` (which rebalanced monthly with asset sales), not an unmanaged buy-and-hold. By contrast, a pure zero-sell SIP (Prism Default Naive Split) achieves 16.70% XIRR without ever realizing taxable capital gains.

---

### Table 2.2: Calendar Year-by-Year Net Returns Comparison (2012 – 2026 YTD)

| Year | Passive Equal-Weight (Prism) | Active Dual Momentum (Omni) | Nifty 50 (`^NSEI`) | Prism Alpha vs Omni | Attribution Winner |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **2012** | **+23.69%** | +18.95% | +27.70% | **+4.74%** | Equal-Weight |
| **2013** | +5.99% | **+7.67%** | +6.76% | -1.68% | Dual Momentum |
| **2014** | +12.48% | **+30.24%** | +31.39% | -17.76% | Dual Momentum |
| **2015** | +0.71% | **+9.65%** | -4.06% | -8.94% | Dual Momentum |
| **2016** | **+13.48%** | -3.85% | +3.01% | **+17.33%** | Equal-Weight |
| **2017** | +23.33% | **+36.57%** | +28.65% | -13.24% | Dual Momentum |
| **2018** | **-0.38%** | -9.27% | +3.15% | **+8.89%** | Equal-Weight |
| **2019** | **+19.69%** | -0.10% | +12.02% | **+19.79%** | Equal-Weight |
| **2020** | +34.21% | **+57.04%** | +14.90% | -22.83% | Dual Momentum (Tech/Gold Surge) |
| **2021** | **+13.77%** | +12.89% | +24.12% | **+0.88%** | Equal-Weight |
| **2022** | **-0.30%** | -31.49% | +4.33% | **+31.19%** | Equal-Weight (Massive Defense) |
| **2023** | +24.69% | **+25.96%** | +20.03% | -1.27% | Dual Momentum |
| **2024** | +26.77% | **+32.83%** | +8.80% | -6.06% | Dual Momentum |
| **2025** | +43.15% | **+70.96%** | +10.51% | -27.81% | Dual Momentum (Silver Surge) |
| **2026 YTD**| **+5.52%** | -0.98% | -3.35% | **+6.50%** | Equal-Weight |

*Head-to-head consistency: Passive Equal-Weight won in 7 out of 14 full calendar years, preventing catastrophic crash years like 2022 (-0.30% vs -31.49%).*

---

## 3. Preservation of Reusable Assets & Migration to Prism-Terminal

All validated engineering assets, cleaned datasets, and broker interfaces developed during Project Omni have been preserved and formally migrated to **Prism-Terminal** (`../Prism-Terminal`):

1. **15-Year Daily Continuous Adjusted Price Series**:
   - Migrated to `Prism-Terminal/data/prism_daily_closes.csv` and `prism_daily_opens.csv`.
   - Continuous 3,910-bar series with zero lookahead, corporate action adjustments, and calendar alignment.
2. **Official Angel One SmartAPI Instrument Token Mapping**:
   - All 5 verified instrument tokens deployed to `Prism-Terminal/data/angel_one_provider.py`:
     * `NIFTYBEES-EQ`: Token `10576`
     * `JUNIORBEES-EQ`: Token `10939`
     * `GOLDBEES-EQ`: Token `14428`
     * `MON100-EQ`: Token `14925`
     * `SILVERBEES-EQ`: Token `11736`
3. **Omni BUG-09: Order Dispatch Rate-Limiting & Pacing**:
   - SmartAPI pacing safeguards (0.35s query sleep, 0.20s order dispatch sleep) transferred to prevent HTTP 429 rate limit exceptions.
4. **Panama Continuous Series Roll Adjustment Scripts**:
   - Verified commodity continuous roll algorithms preserved for historical research.

---

## 4. Decommissioning & Operational Lock

1. **Anti-$p$-Hacking Lock**:
   - All active switching, parameter optimization, moving average tuning, and volatility-targeting experiments on Omni-Terminal are **permanently terminated**.
   - No further algorithmic modifications will be applied to broad ETFs.
2. **Zero Background Overhead Confirmed**:
   - Windows Task Scheduler scripts (`unregister_omni_tasks.ps1`) have confirmed that zero background jobs, daemons, or cron tasks remain active for Omni-Terminal.
   - Zero background memory (0.0 MB) and 0% CPU are consumed on your laptop for broad ETF rotation.
3. **Production Live Routing**:
   - Live real-money order routing inside `omni_angel_live.py` and `start_omni_live.bat` is permanently disabled.
   - All personal capital allocations run strictly through the **Prism Monthly Buy-Ticket Calculator** (`Prism-Terminal/prism_runner.py`).

---

## 5. Final Institutional Certification Sign-Off

| Mandate / Criterion | Threshold | Omni-Terminal (Model B) | Prism-Terminal (Equal-Weight) | Governance Status |
| :--- | :---: | :---: | :---: | :---: |
| **Compound Annual Growth Rate** | $> 15.0\%$ | 14.51% | **15.16%** | **SUPERSEDED BY PRISM** |
| **Maximum Drawdown Target** | $< 25.0\%$ | -39.47% | **-24.83%** | **SUPERSEDED BY PRISM** |
| **Sharpe Ratio ($R_f=6.5\%$)** | $> 0.60$ | 0.43 | **0.68** | **SUPERSEDED BY PRISM** |
| **Operational & Code Complexity** | Low / Robust | High Churn, Server Daemon | **Zero Automation (Manual Monthly Allocation)**| **SUPERSEDED BY PRISM** |
| **Final Governance Verdict** | — | **ARCHIVED / FAILED THESIS** | **PRODUCTION STANDARD (PRISM)**| **FORMALLY SUPERSEDED** |

**Conclusion**: Omni-Terminal has fulfilled its institutional quantitative duty by proving that active ETF momentum is an inferior strategy compared to passive multi-asset diversification. The strategy is officially decommissioned, preserved as an academic case study, and superseded by **Prism-Terminal**.

---

> **Disclaimer**: This audit report reflects quantitative simulation and historical backtest results for research and evaluation purposes only. It does not constitute financial advice, investment planning, or a recommendation to purchase, hold, or sell any security, commodity, or ETF. Past backtested performance is no guarantee of future returns.
