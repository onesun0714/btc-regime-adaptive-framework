# BTC Regime-Adaptive Trading Framework — v2.0

**[→ Live Report](https://onesun0714.github.io/btc-regime-adaptive-framework/)**

A systematic discretionary framework for trading Bitcoin — built by a commodity trader, not a quant. Applies 12 years of physical and derivatives trading intuition (crude oil, naphtha, benzene) to a multi-layer, walk-forward validated decision architecture.

> "You can't trade a single number. You need to know which regime you're in."

---

## Headline Results — True Walk-Forward OOS (2019–2025)

| Metric | Value |
|---|---|
| OOS Compounded Return | **+638%** |
| Buy & Hold (2018 data start) | +416% |
| Avg Sharpe (OOS) | **0.79** |
| Avg Max Drawdown | **-26%** vs B&H -65% |
| Avg Exposure | 52% |
| OOS Validation Period | 7 years (trained on 2018, tested 2019–2025) |

Outperforms B&H by +222 percentage points with roughly half the drawdown. The edge is risk-adjusted, not a pure return maximizer — 6 of 7 years underperform B&H on annual return, but 2022 (-37% vs B&H -65%) is where the framework earns its keep.

---

## Architecture — Four Decision Layers

**Layer 1 — Macro Regime**
DXY trend, M2 growth, VIX, and HY credit spread classify into four regimes: `EXPANSION`, `STABLE`, `TIGHTENING`, `BLACK_SWAN`. All downstream logic is regime-conditional.

**Layer 2 — Cycle Phase**
MVRV Z-Score and NUPL determine cycle position: `BEAR`, `BULL_EARLY`, `BULL_MID`, `BULL_LATE`. Controls trailing stop width and position cap.

**Layer 3 — Composite Score**
Eight variables (MVRV Z-Score, NUPL, Fear & Greed, SOPR, VIX, DXY, 4-week and 12-week momentum) combined with regime-conditional weights. Score 0–100, contrarian-framed: high score = fear = buy zone. 92% accuracy at historical cycle inflection points (NB2 validation).

**Layer 4 — Position Sizing**
Entry/exit thresholds, exposure floors and caps, 10% trailing stop, and four ETF-era structural signals determine final position. Score below 50 → 0% exposure.

---

## V2 Signal Upgrades (Post-ETF Era)

Four structural signals developed in response to the January 2024 spot ETF approval:

- **`whale_dist`** — Price/MVRV-Z divergence detecting institutional distribution. Applied post-2024 only.
- **`early_warning`** — VIX + MVRV-Z macro stress signal. Fired 76 days in 2022, zero days in 2019/2021/2023/2024/2025.
- **`trending_up`** — STABLE regime momentum filter. Separates genuine bull STABLE periods from chopsolidation.
- **`etf_bull`** — 20-day cumulative ETF flow confirmation (Farside data). Structurally new signal for the institutional era.

---

## Year-by-Year OOS Results

| Period | Strategy | B&H | Max DD | Sharpe | Avg Exp |
|---|---|---|---|---|---|
| 2019 | +40.5% | +82.4% | -46.3% | 0.93 | 61% |
| 2020 | +211.6% | +315.2% | -14.5% | 3.14 | 61% |
| 2021 | +15.5% | +44.1% | -29.1% | 0.46 | 49% |
| 2022 | **-36.9%** | **-65.0%** | -37.5% | -2.24 | 22% |
| 2023 | +57.8% | +153.3% | -18.3% | 1.83 | 61% |
| 2024 | +67.9% | +107.8% | -14.4% | 1.93 | 46% |
| 2025 | -12.7% | -9.7% | -24.3% | -0.49 | 64% |
| **AVG** | | | **-26.4%** | **0.79** | **52%** |

---

## Data Sources

| Signal | Source |
|---|---|
| MVRV Z-Score, NUPL, SOPR | BGeometrics |
| Fear & Greed Index | Alt.me |
| ETF Flows | Farside Investors |
| Stablecoin Supply | DefiLlama |
| Macro (VIX, DXY, M2, HY) | yfinance / FRED |
| BTC Price | yfinance |

---

## Known Limitations

- **2022 January regime lag** — Macro indicators lagged BTC price by ~4 months. `early_warning` partially mitigates but can't fully solve it.
- **Structural long bias** — BTC appreciated significantly during the test period. The real test comes in a prolonged sideways or declining market.
- **Funding rate gap** — 2024 vs 2025 structural difference (retail leverage via funding rates 7x higher in 2024) requires a funding rate momentum filter currently in development.

---

## Background

Built by a commodity trader with 12 years across physical and derivatives markets — Samsung C&T (benzene specialist, ~2M tons/year, ~₩3T revenue) and Triptik Trading SA (Geneva, Asia petrochemicals). Currently completing USC Marshall IBEAR MBA '26.

Personal BTC portfolio since 2019. This framework is the systematic formalization of intuitions developed trading benzene/naphtha spreads — regime-dependency, position sizing under uncertainty, and knowing when not to trade.

---

## Notebooks

| Notebook | Description |
|---|---|
| NB1 — Market Regime Dashboard | Composite score construction, regime classification, signal weights |
| NB2 — Cycle Backtest | Historical turning point validation, 83% accuracy across 12 inflection points |
| NB3 — Trading Strategy | Entry/exit rules, position sizing, walk-forward OOS engine |

---

*For informational purposes only. All results are backtested/simulated and do not represent live trading performance.*
