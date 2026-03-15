# 📊 Trading Dashboard

> A real-time algorithmic trading dashboard built with Python + JavaScript — lightweight, fast, and fully customizable.

![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green?style=flat-square&logo=fastapi)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow?style=flat-square&logo=javascript)
![Binance](https://img.shields.io/badge/Binance-API-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

---

## 🖥️ Overview

A browser-based trading platform that streams live market data directly from Binance, renders professional candlestick charts, generates BUY/SELL signals via custom Python indicators, and runs backtests — all on your local machine.

**Data usage: ~3–4 MB/hour** (vs TradingView's 200MB+)

---

## ✨ Features

### 📈 Live Chart Engine
- Real-time candlestick chart powered by [Lightweight Charts](https://github.com/tradingview/lightweight-charts)
- EMA 7 / 25 / 99 overlay lines
- Multi-timeframe: `1m` `5m` `15m` `1h` `4h` `1D`
- Infinite scroll — loads historical candles on demand (Lazy Loading)
- Candle countdown timer per timeframe
- Symbol search — any USDT pair from Binance

### 🔍 Custom Indicator System
- **Plugin architecture** — drop a `.py` file → auto-loads in dashboard
- Toggle indicators on/off from UI
- Signals rendered as arrows directly on chart
- Included indicators:
  - **Rejection Power Meter v2** — Pine Script translated to Python
    - Wick pressure analysis
    - Candle pattern detection (Hammer, Engulfing, Doji, Harami...)
    - Volume confluence
    - RSI + EMA context
    - Conflict filter + next-candle confirmation
  - **Chart Patterns** — Double Top/Bottom, H&S, Flags, Triangles

### 📊 Backtesting Engine
- Date range selector (7D / 30D / 90D / All)
- Optional **Stop Loss** %
- Optional **Take Profit** %
- Trading **Fee** per side
- Custom **Initial Balance**
- Position reversal logic (no duplicate entries)
- **Metrics:** Win Rate · PnL% · Final Balance · Total Fees
- Trade history with exit reason: `SIG` / `REV` / `SL` / `TP`

### ⚡ Performance
| Feature | This Dashboard | TradingView |
|---------|---------------|-------------|
| Data usage | ~3–4 MB/hr | ~200 MB/hr |
| App size | ~10 MB | N/A (web) |
| Custom Python logic | ✅ | ❌ |
| Local processing | ✅ | ❌ |
| Internet required | Binance only | Always |

---

## 🗂️ Project Structure

```
dashboard/
├── frontend/
│   ├── index.html                          # Main UI (chart + modals)
│   └── lightweight-charts.standalone.js   # Chart library (local)
│
└── backend/
    ├── main.py                             # Startup orchestrator
    ├── data/
    │   ├── binance_ws.py                   # Binance WebSocket feed
    │   ├── historical_download.py          # Historical data download
    │   ├── gap_filler.py                   # Fill missing candles
    │   └── candles.db                      # SQLite database
    ├── aggregator/
    │   └── aggregator.py                   # 1m → 5m/15m/1h/4h/1d
    ├── server/
    │   └── ws_server.py                    # FastAPI + WebSocket server
    └── custom_indicators/
        ├── __init__.py                     # Auto-loader
        ├── rejection_power_meter.py        # Built-in indicator
        └── chart_patterns.py              # Built-in indicator
```

---

## 🚀 Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/trading-dashboard.git
cd trading-dashboard
```

### 2. Install dependencies
```bash
cd backend
pip install fastapi uvicorn websockets requests
```

### 3. Download Lightweight Charts library
Save this file to `frontend/` folder:
```
https://unpkg.com/lightweight-charts@4.1.3/dist/lightweight-charts.standalone.production.js
```

### 4. Run
```bash
cd backend
python main.py
```

### 5. Open browser
```
http://localhost:8000
```

---

## 🔧 Create Your Own Indicator

Add a `.py` file to `backend/custom_indicators/`:

```python
NAME        = "My Strategy"
DESCRIPTION = "EMA crossover signals"

def calculate(candles):
    """
    candles: list of {t, o, h, l, c, v}
    return:  list of signals
    """
    signals = []
    
    for i, candle in enumerate(candles):
        if i < 10:
            continue
        
        # Your logic here
        if buy_condition:
            signals.append({
                "time":  candle["t"],
                "type":  "BUY",
                "label": "My Signal ↑",
                "score": 75,
                "color": "#26a69a",
            })
    
    return signals
```

Restart server → indicator appears in dashboard automatically.

---

## 📊 Backtest Sample Result

```
Period:    7 days (1m timeframe)
Indicator: Rejection Power Meter v2
Trades:    53
Win Rate:  69.8%
PnL:       +13.3%
Balance:   $1000 → $1133.64
Fees:      $16.93 (0.01% per side)
SL/TP:     None
```

> ⚠️ Past results do not guarantee future performance. Always validate with larger datasets.

---

## 🛣️ Roadmap

- [ ] Max Drawdown metric
- [ ] Walk-forward testing
- [ ] Multi-symbol support
- [ ] Alert system (Telegram / Email)
- [ ] Exchange API integration (auto trading)
- [ ] PWA mobile app
- [ ] User authentication

---

## 🧰 Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.12, FastAPI, Uvicorn |
| WebSocket | websockets, FastAPI WS |
| Database | SQLite |
| Data Source | Binance REST + WebSocket API |
| Frontend | Vanilla JavaScript (ES6) |
| Charts | Lightweight Charts (TradingView) |
| Indicators | Pure Python (no TA-Lib dependency) |

---

## ⚠️ Disclaimer

This project is for **educational and research purposes only**.
It is not financial advice. Trading involves significant risk of loss.
Always do your own research before making any trading decisions.

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🤝 Connect

Built by **Adnan** — IT Infrastructure Specialist transitioning to Cybersecurity & Fintech

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/YOUR_PROFILE)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat-square&logo=github)](https://github.com/YOUR_USERNAME)
