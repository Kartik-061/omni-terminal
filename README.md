# Omni-Terminal: Multi-Asset Macro Engine (Project CODEX)
> **FINAL GOVERNANCE STATUS: ARCHIVED (DOCUMENTED NEGATIVE RESULT)**  
> **Governance Action**: Shelved for automated deployment. Replaced by a passive **Equal-Weight Multi-Asset ETF Benchmark** for research evaluation. All computational, memory, and API resources allocated to **Forge-Terminal** (Sector-Terminal having been retired).

---

## Executive Summary & Architectural Post-Mortem

Following an exhaustive 15.0-year forensic audit (2011–2026), **Omni-Terminal has been permanently shelved as an active algorithmic strategy for Project CODEX.**

### Why It Was Shelved (The Quantitative Proof):
1. **Underperforms Passive Allocation**: An **Equal-Weight (20% each) Monthly-Rebalanced** basket across the same 5 ETFs (`NIFTYBEES`, `JUNIORBEES`, `GOLDBEES`, `MON100`, `SILVERBEES`) achieved **15.16% Net Calendar CAGR**, **-24.83% Max Drawdown**, and a **0.68 Sharpe Ratio** ($R_f = 6.5\%$). Active Dual Momentum achieved only **14.51% Calendar CAGR**, a brutal **-39.47% Max Drawdown**, and a **0.43 Sharpe Ratio**.
2. **The Trend Gate Destroys Alpha**: Adding 200 SMA trend gates to monthly macro ETFs creates lagging whipsaws: selling near market bottoms, paying STT + slippage, and missing explosive early recoveries.
3. **System Resource Preservation**: Running an active algorithm on 5 broad ETFs consumes background memory and API bandwidth on your laptop without generating alpha. Documenting this negative result protects capital and eliminates operational fragility.

### Research Finding: Passive Multi-Asset Baseline Performance (Not Financial Advice)
In the 15-year historical backtest (2011–2026), an **Equal-Weight (20% each) Multi-Asset Basket** was evaluated as a passive baseline across the same 5 ETFs:
- 20% `NIFTYBEES.NS` (Large-Cap Domestic Equity)
- 20% `JUNIORBEES.NS` (Mid-Cap Domestic Equity)
- 20% `GOLDBEES.NS` (Precious Metals — Gold)
- 20% `MON100.NS` (Global Tech / US Equity)
- 20% `SILVERBEES.NS` (Precious Metals — Silver)

**Empirical Finding**: When rebalanced monthly, this static allocation compounded at **15.16% Net Calendar CAGR** with a **-24.83% Max Drawdown** and **0.68 Sharpe Ratio** ($R_f = 6.5\%$, Sortino 0.87, Calmar 0.61), outperforming the active dual-momentum model on returns, drawdown, and transaction efficiency.

> **Disclaimer**: This documentation reflects quantitative backtest results for research and historical evaluation purposes only. It does not constitute investment advice, financial planning, or a recommendation or solicitation to buy, hold, or sell any security or ETF. Past performance is no guarantee of future returns.

---

## 1. System Overview & Core Thesis

**Omni-Terminal** is a systematic, multi-asset dual-momentum engine built for Indian markets as part of Project CODEX. 

Unlike single-stock momentum, economic cycles cause massive capital rotations across macroeconomic asset classes:
* **2020**: US Tech & Precious Metals surged during global central bank liquidity injections.
* **2021**: Indian Mid-Caps and cyclical equities outperformed.
* **2022**: Global tech crashed (-31.4% in Nasdaq 100) while Cash & Commodities held firm.
* **2023–2026**: Indian equities entered a secular bull market while Gold and Silver surged to all-time highs.

By applying **Gary Antonacci's Dual Momentum** (Relative Momentum to pick the strongest 2 macro assets + Absolute Dual-Horizon Trend Gate), Omni-Terminal was extensively tested across 15.0 years of real exchange data (2011–2026). The forensic audit demonstrated that active switching underperformed passive diversification.

---

### Core Strategy Rules & Historical Architecture (Archived)

1. **Macro Asset Universe**:
   - `NIFTYBEES.NS`: Large-Cap Indian Equities (Nifty 50)
   - `JUNIORBEES.NS`: Mid-Cap Indian Equities (Nifty Next 50)
   - `GOLDBEES.NS`: Precious Metals - Gold
   - `MON100.NS`: Global Tech / US Equities (Nasdaq 100 ETF)
   - `SILVERBEES.NS`: Precious Metals - Silver
   - `LIQUIDBEES.NS`: Cash Safe Harbor (earning **6.50% APR risk-free yield**)
   - `^NSEI`: Benchmark (Nifty 50 Index)
2. **Relative Momentum Scoring**:
   $$\text{Composite Score} = 0.50 \times \text{Return}_{3M} + 0.50 \times \text{Return}_{6M}$$
   $$\text{Volatility-Adjusted Score} = \frac{\text{Composite Score}}{\sigma_{30d}}$$
   Rank all 5 macro ETFs by Vol-Adjusted Score; select the **Top 2 Trending Macro Assets**.
3. **Volatility-Parity Capital Allocation (Inverse Sizing)**:
   $$\text{Weight}_i = \frac{1/\sigma_i}{\sum (1/\sigma)}$$
   Weights are inversely proportional to 30-day realized volatility. Low-volatility Gold receives ~60%–65%, while high-beta Silver/Tech receives ~35%–40%, compressing portfolio drawdown.
4. **Dual-Horizon Trend Gate (The 2022 Fed Drawdown Fix)**:
   - **Full Pass**: Asset satisfies $\text{Close} > \text{SMA}_{200}$, $\text{Return}_{6M} > 0.0\%$, and $\text{Close} > \text{EMA}_{50}$. Holds 100% of its Vol-Parity slot.
   - **Early-Warning Trim**: Asset satisfies $\text{Close} > \text{SMA}_{200}$ and $\text{Return}_{6M} > 0.0\%$, but drops below its **50-day EMA**. Instantly trims position by **50% into Liquid Bees**.
   - **Trend Failure**: Asset breaches **200-day SMA** or has negative 6M return. Exits **100% into Liquid Bees** (6.50% APR).
5. **Mid-Month Catastrophic Circuit Breaker**:
   - Evaluated daily at 15:35 IST: If any held ETF drops **> 7.5% below its 200-day SMA** ($\text{Close} < 0.925 \times \text{SMA}_{200}$), it is liquidated immediately mid-month into Liquid Bees without waiting for month-end.
6. **Rebalance Drift Buffer ($\pm 2.5\%$ Band)**:
   - If an existing held asset's portfolio weight sits within $\pm 2.5\%$ of target, shares are **100% untouched** (0 churn, 0 STT, 0 brokerage).
7. **Daily Cash Yield Accrual Engine**:
   - Compounds **6.50% APR** daily on idle cash / Liquid Bees pools ($Balance \times \frac{0.065}{365}$).
8. **Statutory Delivery Friction Model**:
   - Modeled at 10.0 bps adverse slippage per side + statutory STT, stamp duty, SEBI, exchange turnover, and brokerage fees (0.2485% on buys, 0.2335% on sells).
9. **Rebalance Schedule Architecture (Archived)**:
   - Designed for the **Final Trading Day of each month at 15:10 IST** (Archived research rule; live schedule retired).

---

## 2. 15-Year Performance & Forensic Audit Summary (2011–2026)

| Performance Metric | Passive Equal-Weight 5-ETF (Monthly Rebalanced) | Omni Model B (T+1 Open) | Omni Model A (EOD Close) | Nifty 50 Benchmark |
| :--- | :---: | :---: | :---: | :---: |
| **Full 15-Year Calendar Net CAGR (2011–2026)** | **15.16%** | **14.51%** | 17.02% | 10.91% |
| **Full Sample Max Drawdown** | **-24.83%** | **-39.47%** | -35.57% | -38.44% |
| **Sharpe Ratio ($R_f = 6.5\%$)** | **0.68** | **0.43** | 0.58 | 0.27 |
| **Sortino Ratio** | **0.87** | **0.48** | 0.66 | 0.35 |
| **Calmar Ratio** | **0.61** | **0.37** | 0.48 | 0.28 |
| **Turnover & Operational Churn** | **Monthly Rebalanced (Periodic sells to maintain 20% target)** | High Active Churn (Frequent full-position rotations) | Theoretical EOD | Zero Churn (Price Return Benchmark) |
| **Tax Drag (20% STCG)** | **Capital Gains on Rebalance Sells** | Severe STCG Drag | Severe Drag | 0% (Passive Index) |

> **Conclusion**: Passive Equal-Weight (monthly-rebalanced) dominates Active Dual Momentum on CAGR (+65 bps net), Max Drawdown (+1,464 bps safer), and Sharpe ratio (+0.25 higher efficiency). See [OMNI_TERMINAL_AUDIT_REPORT.md](OMNI_TERMINAL_AUDIT_REPORT.md) for the comprehensive forensic breakdown.

---

### Historical Crisis Stress Resilience

* **2011 European Debt Crisis**: Omni **+2.30%** vs Nifty **-13.18%** (Alpha: **+15.48%** | Postured in 50% Gold, 50% Nasdaq).
* **2016 Demonetization**: Omni **-8.53%** vs Nifty **-5.11%** (Postured in Midcaps & Nasdaq).
* **2018 IL&FS Credit Crisis**: Omni **-8.05%** vs Nifty **-11.08%** (Alpha: **+3.03%** | Postured in Nifty & Nasdaq).
* **2020 COVID-19 Collapse**: Omni **+16.07%** vs Nifty **-19.07%** (Alpha: **+35.13%** | Nifty MaxDD: **-38.44%**).
* **2022 Global Tech Crash**: Omni Model B suffered **-31.49%** (MaxDD **-28.10%**) vs Nifty **+4.33%** and Passive Equal-Weight **-0.30%** (the experimental upgraded variant with 50 EMA gate showed -8.03% MaxDD in Model A, but is not the baseline Model B).

---

## 3. Archived Paper Portfolio Snapshot (Angel One Official NSE Feed)

* **Snapshot Status**: Archived Paper Snapshot (Zero live capital at risk)
* **Broker Data Gateway**: **Angel One SmartAPI (Official NSE Gateway)**
* **Client Code**: `[REDACTED_CLIENT_CODE]` (Credentials stored in gitignored local configuration)
* **Base NAV**: **₹99,753.12** (Initial Capital: ₹100,000.00)
* **Invested Equity**: **₹99,328.56** (99.6%)
* **Liquid Bees Cash Pool**: **₹424.56** (0.4%) earning **6.50% APR daily yield**
* **Active Holdings (Volatility-Parity Sizing)**:
  - `NIFTYBEES.NS`: 189 shares @ ₹286.54 | ₹54,156.10 (**54.3%** | 50 EMA: ₹267.59, 200 SMA: ₹272.57 | **FULL PASS**)
  - `JUNIORBEES.NS`: 57 shares @ ₹792.50 | ₹45,172.50 (**45.3%** | 50 EMA: ₹760.31, 200 SMA: ₹756.21 | **FULL PASS**)
* **Disqualified Assets Rotated to Cash**:
  - `MON100.NS` (Nasdaq 100): 6M Return negative (-6.15%) $\rightarrow$ Exited
  - `GOLDBEES.NS` (Gold): Breached 200 SMA $\rightarrow$ Exited
  - `SILVERBEES.NS` (Silver): Breached 200 SMA $\rightarrow$ Exited
* **Next Scheduled Rebalance**: **Archived / Decommissioned** (Live scheduling discontinued; Sector-Terminal retired).

---

## 4. Automation & Background Execution Runbook

### Option A: Interactive Launcher Menu (`start_omni_live.bat`)
Double-click `start_omni_live.bat` to launch the interactive CLI management menu:
* **`[1]` View Live Portfolio Status Dashboard (`--status`)**: Displays ASCII portfolio table, rankings, and countdown.
* **`[2]` Sync Latest Market Prices, Yield & Circuit Breakers (`--sync`)**: Accrues 6.50% yield and verifies safety buffers.
* **`[3]` Background Daemon**: Disabled (archived).
* **`[4]` Force Rebalance Now (`--rebalance-now`)**: Immediate portfolio rebalance execution (paper mode only).
* **`[5]` Test Angel One SmartAPI Feed & Live LTP Quotes**: Verifies official NSE candles and live LTP.
* **`[6]` Live Broker Order Routing**: Permanently DISABLED (Forensic Audit Protection).
* **`[7]` Open Visual Master Cockpit Dashboard**: Launches interactive 5-tab HTML dashboard in your browser.
* **`[8]` Run Automated Test Suite**: Executes 13 pytest unit and integration tests.
* **`[9]` Run Forensic Stress Testing Suite**: Runs adversarial 2022 fix, Vol-Parity, calendar jitter, and friction tests.
* **`[10]` Configure Angel One Credentials (Setup Wizard)**: Interactive onboarding wizard.
* **`[11]` Reset Portfolio State**: Resets ledger to clean baseline cash.

### Option B: 24/7 Background Daemon (Decommissioned)
The 24/7 background scheduler (`--daemon`) has been permanently retired to eliminate background compute and API usage on your laptop. Zero background daemons or scheduled tasks run for Omni-Terminal.

### Option C: Task Scheduler Decommissioning (Archived)
All background jobs for Omni-Terminal have been uninstalled via:
```powershell
powershell -ExecutionPolicy Bypass -File .\unregister_omni_tasks.ps1
```
*Confirmed: Zero scheduled tasks or daemons remain active.*

---

## 5. Angel One SmartAPI Production Setup (Locked & Migrated)

> **SAFETY NOTICE**: Live execution of Omni Dual Momentum has been **permanently disabled** following the audit results.
> All production broker connections, token mappings, and live execution tools have been migrated to [Prism-Terminal](../Prism-Terminal) for the Passive 5-ETF Core portfolio (`prism_runner.py`).

Inspection only (Dry-Run Mode):
```powershell
python omni_angel_live.py --capital 1000000 --dry-run
```

---

## 6. Verification Test Suite

Run the automated test suite anytime:
```powershell
pytest -v tests/
```
All 13 test suites validate:
* Momentum scoring formula ($0.50 \times R_{3M} + 0.50 \times R_{6M}$)
* Volatility-adjusted normalization ($\text{Score} / \sigma_{30d}$)
* Baseline trend gate criteria ($\text{Close} > \text{SMA}_{200}$ and $R_{6M} > 0$)
* Dual-Horizon Early Warning (50 EMA trim)
* Mid-Month Catastrophic Circuit Breaker (> 7.5% drop below 200 SMA)
* Volatility-Parity inverse-variance weights
* Rebalance Drift Buffer ($\pm 2.5\%$)
* Anomaly repair logic for ETF feed scale glitches
* Broker tick rounding (₹0.05)
* Official Angel One NSE instrument tokens
* Portfolio state initialization
* Daily 6.50% APR Liquid Bees accrual math
* Scheduled rebalance timing on month-end trading days at 15:10 IST

---
*Project CODEX — Quantitative Trading & Multi-Asset Research Engine*

