# BTC Regime-Adaptive Trading Framework

**A multi-layer systematic discretionary trading model built by a commodity trader.**

> *Signals big, stops late, re-entries rare.*

---

## Performance Summary (2018–2026 Backtest)

| Metric | Strategy | Buy & Hold |
|--------|----------|------------|
| Total Return | **2.2x** vs B&H | Baseline |
| CAGR | **32.1%** | ~22% |
| Sharpe Ratio | **0.81** | 0.42 |
| Calmar Ratio | **0.73** | 0.27 |
| Max Drawdown | **-43.7%** | -81.5% |
| Profit Factor | **3.09** | — |
| Cycle Top/Bottom Accuracy | **92%** (11/12) | — |

---

## Architecture

The model operates through four sequential decision layers:

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1: MACRO REGIME                                      │
│  M2 Growth · DXY Trend · VIX Level · HY Spread             │
│  → EXPANSION | STABLE | TIGHTENING | BLACK_SWAN             │
├─────────────────────────────────────────────────────────────┤
│  LAYER 2: MARKET PHASE                                      │
│  Momentum · Valuation · Regime context                      │
│  → BULL_EARLY | BULL_LATE | BEAR                            │
├─────────────────────────────────────────────────────────────┤
│  LAYER 3: COMPOSITE SCORE                                   │
│  On-chain (MVRV, NUPL, SOPR) + Sentiment (FnG)             │
│  + Macro (VIX, DXY) + Momentum (4W, 12W)                   │
│  Regime-conditional weights · Contrarian 0-100              │
├─────────────────────────────────────────────────────────────┤
│  LAYER 4: RISK MANAGEMENT                                   │
│  Phase-specific trailing stops · Pre-crisis early warning   │
│  BLACK_SWAN forced exit · STABLE regime stop-loss           │
└─────────────────────────────────────────────────────────────┘
```

---

## Key Design Principles

1. **Regime First, Signal Second** — The macro environment determines position size boundaries. The composite score provides micro-adjustments within those boundaries.

2. **Core Long Conviction** — In bull regimes, hold a large base position and let it compound. Score-driven entry/exit systems systematically underperform because they cut winners too early.

3. **Separate Entry Logic from Risk Logic** — Signals determine entry/exit. Stops only fire when the thesis is structurally broken.

4. **Minimize Oscillation** — Once positioned, minimum holding periods and cooldowns prevent whipsaw from noise.

5. **Protect from Peak** — Risk is measured from equity high-water mark, not from entry price.

---

## Data Sources

| Category | Source | Coverage |
|----------|--------|----------|
| On-chain | BGeometrics API | 2022–present |
| On-chain (backfill) | CoinMetrics Community | 2017–2022 |
| Sentiment | Alternative.me F&G | 2018–present |
| Macro | FRED (VIX, M2, HY Spread) | 2015–present |
| Price | Yahoo Finance | 2017–present |
| Derivatives | Deribit API | 2020–present |
| ETF Flows | Farside Investors | 2024–present |

---

## Known Limitations

- **In-sample backtest only.** Walk-forward OOS R² is negative. Directional accuracy (92%) and trade profitability are robust, but exact return prediction is not possible.
- **BLACK_SWAN detection is reactive.** VIX spikes from 17→37 in one day leave no lead time. Pre-crisis early warning system (VIX 23 crossover + 200d MA + dd90) implemented with 30-day cooldown; reduces but does not eliminate tail risk.
- **Structural long bias.** BTC appreciated 12x during the test period. Framework's edge in a prolonged bear/sideways market is less validated.
- **On-chain signal degradation.** Post-ETF institutional flows bypass on-chain metrics (see ForeDex Research, 2026). Framework mitigates this via regime-conditional weighting. MVRV and NUPL backfilled to 2017 using CoinMetrics community data.

---

## Development Roadmap

- [ ] Probability-based regime scoring (continuous vs. discrete classification)
- [ ] Walk-forward validation with rolling OOS windows
- [ ] Alt-coin rotation layer (BTC dominance timing)
- [ ] Options overlay for BLACK_SWAN tail hedging
- [ ] Cross-asset macro regime expansion (rates, credit, commodities)

---

## Background

Built by a 12-year commodity trader (Samsung C&T benzene/crude → TRIPTIK TRADING SA (Geneva) petrochemicals) pursuing an MBA at USC Marshall. The framework applies commodity trading principles from both physical and derivatives markets — regime-dependent positioning, spread-based mean reversion, inventory cycle analysis — to crypto markets.

The core insight: **the same price signal means different things in different regimes.** MVRV at 3.0 during M2 expansion is a hold. MVRV at 3.0 during liquidity tightening is an exit. Encoding this context-dependency is what separates this framework from single-indicator approaches.

---

## Disclaimer

This repository contains a research framework description only. No trading code, specific thresholds, or model parameters are shared. Past backtest performance does not guarantee future results. This is not investment advice.

*© 2026 Onesun Lee*
