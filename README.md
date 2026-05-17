# BTC Cycle Strategy V18

> **A systematic framework applying commodity trading discipline to digital asset cycle dynamics.**

[![White Paper](https://img.shields.io/badge/Download-White_Paper_(PDF)-1B2B5E?style=flat-square)](BTC_Cycle_Strategy_V18_WhitePaper.pdf)
[![Live Page](https://img.shields.io/badge/Live-Interactive_Page-B8860B?style=flat-square)](https://onesun0714.github.io/btc-regime-adaptive-framework/)

---

## Headline Results

8-year backtest, January 2018 to May 2026

| Metric | Strategy | BTC Buy & Hold | Vol-Matched BTC |
|---|---:|---:|---:|
| **Total Return** | **+769.3%** | +443.8% | +253.7% |
| **CAGR** | **+29.5%** | +22.3% | +18.2% |
| **Sharpe Ratio** | **0.88** | 0.64 | 0.64 |
| **Max Drawdown** | **-52.4%** | -81.5% | -59.0% |
| **Calmar Ratio** | **0.56** | 0.27 | 0.31 |
| **Profit Factor** | **3.41** | N/A | N/A |

Initial capital of $100,000 grows to **$869,282** (vs $543,800 for buy-and-hold, 1.60x multiple).

---

## Core Thesis

Most BTC alpha models reduce, on inspection, to variations of long bias. They refine entry timing or add exit signals while preserving directional exposure. This research begins from a different question.

> **In Bitcoin as a cycle-driven asset, once mechanical beta exposure is stripped away, where does genuine risk-adjusted alpha originate? And can it be statistically distinguished from chance?**

Eight years of data analysis points to two distinct sources:

1. **Regime-conditional position sizing** during liquidity transitions
2. **Phase distinction within seemingly homogeneous regimes** — the EXPANSION regime separates into EARLY (signal unconfirmed) and MATURE (signal confirmed) phases with materially different forward risk-adjusted returns

---

## The Core Discovery: EXPANSION Phase Separation

Earlier model iterations treated EXPANSION as a single regime. Closer examination reveals two distinct patterns within what appears to be one regime.

| Phase | Definition | 90d Forward Sharpe | Examples |
|---|---|---:|---|
| **EARLY** | First 60 days of EXPANSION (prior regime different) | 0.35 | Early 2018, Late 2022 |
| **MATURE** | EXPANSION sustained >60 days | **2.12** | Late 2020, Late 2024 |

**Statistical validation**: t-statistic 4.74, p-value < 0.0001. Within identical composite score bands (50-65), EARLY vs MATURE return differential is +32 percentage points — confirming phase information adds value beyond score alone.

---

## Validation Summary

Three independent forms of out-of-sample validation:

### 1. Walk-Forward Sub-Period Analysis
6 sub-periods tested. MATURE Sharpe consistently exceeded EARLY across **all 6 sub-periods**. Mean Sharpe: EARLY 0.35, MATURE 2.12.

### 2. True Out-of-Sample Test
- **Fit period (2018-2022)**: EARLY Sharpe 0.66, MATURE Sharpe 1.44
- **Test period (2023-2026, OOS)**: EARLY Sharpe -0.25, MATURE Sharpe **1.44** (held)

### 3. Bootstrap Test (n=5,000)
Random position reordering produces mean CAGR +16.4% (std 7.5%). Strategy actual CAGR +29.5% ranks at the **95.7th percentile**, statistically significant at p < 0.05.

---

## Methodology — Three-Notebook Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  NB1: Market Regime Dashboard                                   │
│  Inputs: M2, DXY, VIX, HY spread, BTC 200d, BTC 12w momentum    │
│  Output: 4-state regime (EXPANSION / STABLE / TIGHTENING / BS)  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  NB2: Cycle Backtest                                            │
│  Validates regime classification against 12 known cycle pivots  │
│  Accuracy: 10/12 = 83%                                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  NB3: Trading Strategy Engine                                   │
│  Inputs: regime + composite score + EXPANSION phase             │
│  Output: daily target position with trailing/hard stops         │
└─────────────────────────────────────────────────────────────────┘
```

### Composite Score Construction

Seven signals, weights derived from R-squared optimization:

| Signal | Weight | Interpretation |
|---|---:|---|
| Fear & Greed Index | 37.6% | Aggregate market sentiment |
| VIX | 27.0% | Risk appetite (inverse) |
| MVRV Z-Score | 15.2% | Deviation from realized cap |
| MVRV | 6.0% | Market value to realized value |
| DXY | 5.8% | Dollar strength (inverse) |
| NUPL | 5.6% | Net Unrealized Profit/Loss |
| SOPR | 2.8% | Spent Output Profit Ratio |

### Position Sizing Rules

| Score Range | EARLY Position | MATURE Position |
|---|---:|---:|
| ≥ 70 | 0.80 | 1.00 |
| 55 - 70 | 0.65 | 1.00 |
| 40 - 55 | 0.50 | 0.90 |
| 20 - 40 | 0.35 | 0.80 |
| < 20 | 0.30 (floor) | 0.70 (floor) |

V18 adds a Short-Term Holder (STH) Realized Price ratio overlay within MATURE EXPANSION. The signal — current price divided by the average cost basis of holders active in the past 155 days — captures momentum state independent of the composite score (partial IC +0.228 vs MVRV Z-Score, p<0.05). Applied as a position multiplier, it improved backtest Sharpe by +0.061 over the 2022-2026 test period (bootstrap p=0.875).

---

## Honest Limitations

This section is here because it should be. The white paper documents these in full.

### Sample Concentration
**13 cycle trades over 8 years.** Top 3 trades account for 94% of winning P&L. This is below the typical 100+ trades benchmark for systematic strategies. However, it reflects Bitcoin's four-year halving cycle structure — meaningful trend opportunities are limited to 1-2 per cycle. Inflating trade frequency would constitute over-trading.

### Marginal Jensen's Alpha
Daily regression alpha: +13.9% annualized, t-stat 1.83, p=0.067 (marginally significant). Conventional alpha frameworks struggle with high-noise daily data. Bootstrap test (95.7th percentile) and risk-adjusted metrics are more appropriate evaluation tools.

### 200-Day MA Comparison
| Strategy | CAGR | Sharpe | MaxDD | Calmar |
|---|---:|---:|---:|---:|
| 200-Day MA Crossover | +31.9% | 0.86 | -63.6% | 0.50 |
| **This Strategy (V18)** | +29.5% | 0.88 | -52.4% | 0.56 |

Comparable risk-adjusted returns. The strategy's edge over simple trend-following lies in:
- **11pp drawdown improvement** (compounds with leverage — 16.5pp at 1.5x)
- **Signal attribution** — every trade traceable to specific triggering signals
- **Forward robustness** — multi-signal architecture more adaptive to post-ETF regime change

### STH Data Coverage
The V18 STH overlay was tested on the 2022-2026 period only. Full 2018-2026 validation is pending expanded data access and scheduled for the next iteration.

### Production Infrastructure
Not yet built: live execution infrastructure, realistic slippage modeling, capital capacity analysis, portfolio risk overlay, drawdown protocol, real-time monitoring, forward live tracking record. This is research output, not production system.

---

## Repository Structure

```
btc-regime-adaptive-framework/
├── README.md                                  # This file
├── BTC_Cycle_Strategy_V18_WhitePaper.pdf      # Full white paper (English)
├── docs/
│   └── index.html                             # GitHub Pages site
├── notebooks/
│   ├── nb1_market_regime_dashboard.ipynb      # Regime classification
│   ├── nb2_cycle_backtest.ipynb               # Cycle validation
│   └── nb3_trading_strategy_engine.ipynb      # Position sizing & backtest
├── data/
│   └── (data sources documented in white paper)
└── results/
    ├── strategy_v18_equity.csv                # Daily equity curve
    └── charts/                                # Performance charts
```

### Data Sources

| Type | Source |
|---|---|
| Macro (M2, DXY, VIX, HY spread) | FRED |
| Price (BTC, BTC 200d) | yfinance |
| On-chain (MVRV, NUPL, SOPR, MVRV Z, STH Realized Price) | BGeometrics, bitcoin-data.com |
| Sentiment (Fear & Greed) | Alternative.me |
| ETF flow | Farside Investors |

---

## Roadmap

### Q2 2026
- Live execution begins with personal capital
- Trade-by-trade attribution documented publicly
- Initial slippage measurements vs backtest assumptions

### Q3 2026
- ETH cross-asset validation
- Derivatives signal R&D (funding rate, basis, OI, options skew)
- STH full-period validation (2018-2026) as data access expands

### Q4 2026
- 6-month live tracking results disclosed
- Forward Sharpe vs backtest Sharpe analysis

### 2027
- Depending on results: paper trading scale expansion, or institutional partnership exploration

---

## About

**Onesun Lee**

12+ years global commodity trader.

- **Samsung C&T** — Petrochemical trading, ~$2-3B portfolio, largest single-product PnL
- **TRIPTIK Trading SA** (Geneva) — Senior trader, Asian petrochemicals + naphtha-oil spreads + intercontinental arbitrage
- **USC Marshall IBEAR MBA**, 2026

Open to dialogue with traders, founders, and allocators who think about cycle-driven asset trading and systematic risk management.

---

## Contact

- 📧 **Email**: onesun1220@gmail.com
- 💼 **LinkedIn**: [linkedin.com/in/onesun-lee](https://www.linkedin.com/in/onesun-lee)
- 📄 **Full Paper**: [Download PDF](BTC_Cycle_Strategy_V17_WhitePaper.pdf)

---

## Citation

If you reference this work in research or commentary:

```
Lee, O. (2026). BTC Cycle Strategy V18: A systematic framework applying
commodity trading discipline to digital asset cycle dynamics.
https://github.com/onesun0714/btc-regime-adaptive-framework
```

---

## Disclaimer

This repository is provided for informational and research purposes only. It does not constitute investment advice or solicitation. Backtest results are simulations based on historical data and do not guarantee future performance. All investments carry the risk of loss; digital asset investments in particular involve high volatility and potential total loss.

All content (analytical framework, methodology, code, charts, data interpretation, and text) is the intellectual property of Onesun Lee. Unauthorized reproduction, redistribution, or commercial use without prior written consent is prohibited. Academic citation is welcomed with clear source attribution.

© 2026 Onesun Lee. All rights reserved.

WSL/Linux workflow test successful.
