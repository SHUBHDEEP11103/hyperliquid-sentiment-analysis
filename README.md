# Hyperliquid Sentiment Analysis: Market Sentiment & Trader Behavior

![Sentiment Overview](output/charts/01_sentiment_overview.png)

## 📌 Project Overview

This project investigates the relationship between **Bitcoin market sentiment (Fear/Greed Index)** and **trader behavior/performance on Hyperliquid**, a decentralized perpetual futures exchange. The analysis uncovers actionable patterns to inform smarter trading strategies.

### Objectives
- Understand how trader performance (PnL, win rate) correlates with market sentiment
- Identify behavioral shifts during Fear vs. Greed market periods
- Segment traders into behavioral archetypes
- Propose data-driven strategy recommendations

---

## 📊 Datasets

### 1. Bitcoin Fear/Greed Index (`data/fear_greed_index.csv`)
| Column | Type | Description |
|--------|------|-------------|
| `timestamp` | Unix (seconds) | Unix timestamp |
| `value` | int 0–100 | Sentiment score |
| `classification` | string | Extreme Fear / Fear / Neutral / Greed / Extreme Greed |
| `date` | YYYY-MM-DD | Calendar date |

- **Rows:** 2,644  •  **Range:** 2018-02-01 → 2025-05-02

### 2. Hyperliquid Trade History (`data/historical_data.csv`)
| Column | Type | Description |
|--------|------|-------------|
| `Account` | string | Trader wallet address |
| `Coin` | string | Asset symbol |
| `Execution Price` | float | Fill price |
| `Size Tokens` | float | Position size in tokens |
| `Size USD` | float | Position size in USD |
| `Side` | BUY/SELL | Trade direction |
| `Timestamp IST` | string | Human-readable IST timestamp |
| `Start Position` | float | Position before this fill |
| `Direction` | string | Open Long / Close Long / Open Short / etc. |
| `Closed PnL` | float | Realised PnL for closing trades |
| `Fee` | float | Trading fee paid |
| `Timestamp` | Unix ms | Unix timestamp in milliseconds (UTC) |

- **Rows:** 211,224  •  **Accounts:** 32  •  **Range:** 2023-03-28 → 2025-06-15

---

## 🔧 Setup & Installation

### Prerequisites
- Python 3.8+
- pip or conda

### Installation Steps

1. **Clone the repository:**
   ```bash
   git clone https://github.com/SHUBHDEEP11103/hyperliquid-sentiment-analysis.git
   cd hyperliquid-sentiment-analysis
   ```

2. **Create a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Data files** are already included in `data/`:
   - `data/fear_greed_index.csv`
   - `data/historical_data.csv`

5. **Run the analysis:**
   ```bash
   jupyter notebook notebooks/sentiment_trader_analysis.ipynb
   ```
   Run all cells in order (Kernel → Restart & Run All).

---

## 📈 Key Outputs

### Part A: Data Preparation
- Dataset validation: shapes, missing values, duplicates
- Timestamp alignment: trades converted from Unix ms (UTC) → daily dates
- Binary sentiment: `Extreme Fear + Fear → "Fear"`;  `Neutral + Greed + Extreme Greed → "Greed"`
- Feature engineering: daily PnL, win rate, trade count, volume USD, long/short ratio

### Part B: Analysis & Insights

#### Insight 1: Performance Differential
- Fear and Greed days show different PnL and win rate profiles
- Statistical significance tested via Welch's t-test and Mann-Whitney U (α = 0.05)
- 95% confidence intervals computed for both groups

#### Insight 2: Behavioral Shifts
- Trade frequency, average position size, and long/short bias all shift with sentiment
- Traders increase long exposure on Greed days (~+7% long ratio vs Fear days)

#### Insight 3: Trader Segmentation
- **High vs Low Volume**: high-volume traders have more concentrated performance during Fear days
- **Frequent vs Infrequent**: frequent traders show different sentiment sensitivity
- **Consistent Winners** (≥55% win rate): ~9% of accounts; maintain profitable performance regardless of sentiment

### Part C: Actionable Recommendations
- **Strategy 1:** Adaptive position sizing — reduce size 20–30% on Fear days; increase for winners on Greed days
- **Strategy 2:** Selective frequency management — boost frequency for frequent high-win-rate traders on Greed days

---

## 📊 Generated Charts

All charts are saved in `output/charts/`:

| Chart | Description |
|-------|-------------|
| `01_sentiment_overview.png` | Mean PnL, win rate, trade count distribution, long ratio — Fear vs Greed |
| `02_segment_analysis.png` | Segment mean PnL across Fear/Greed (3 segments) |
| `03_timeseries_analysis.png` | Daily net PnL, trade count, long ratio over time with sentiment background |
| `04_heatmap_sentiment_pnl.png` | Mean PnL heatmap: sentiment classification × trade size quintile |

---

## 📁 Project Structure

```
hyperliquid-sentiment-analysis/
├── data/
│   ├── fear_greed_index.csv          # Bitcoin Fear/Greed Index
│   └── historical_data.csv           # Hyperliquid trade history
├── notebooks/
│   └── sentiment_trader_analysis.ipynb  # Main analysis notebook (Parts A, B, C)
├── output/
│   ├── charts/                        # Generated PNG charts (4 files)
│   ├── tables/                        # Generated CSV tables (5 files, .gitignored)
│   └── summary.md                     # Executive summary (generated by notebook)
├── README.md
└── requirements.txt
```

---

## 📝 Known Issues Fixed

The original notebook had several issues that have been corrected:

| Issue | Fix |
|-------|-----|
| Hardcoded local file paths | Replaced with relative paths resolved from notebook location |
| Wrong column references (`Date`, `Classification`) | Fixed to use actual column names (`date`, `classification`) |
| `KeyError: 'timestamp'` on trades DataFrame | Trades use `Timestamp` (Unix ms), not `timestamp`; fixed accordingly |
| Timezone misalignment | `Timestamp` (Unix ms, UTC) used for date extraction; IST offset handled correctly |
| Missing binary classification | 5-level → 2-bucket mapping added: Fear+Extreme Fear → "Fear", others → "Greed" |
| No output directory creation | `os.makedirs(..., exist_ok=True)` added at setup |
| Incomplete analysis (only 2 cells) | Complete Parts A, B, and C implemented across 13 code cells |
| Missing statistical tests | Welch's t-test, Mann-Whitney U, and 95% CI added throughout |
| No strategy/export section | Part C generates charts, tables, and `output/summary.md` |

---

## 💡 Strategy Recommendations

### Strategy 1: Adaptive Position Sizing
```
Fear days  → Reduce trade size by 20–30% for all accounts
Greed days → Maintain/increase size for accounts with win rate ≥ 55%
Impact     → Limit drawdowns on risk-off days; preserve upside on risk-on days
```

### Strategy 2: Selective Frequency Management
```
Greed days → Frequent traders (top 50%) with >45% win rate: +15–25% frequency
Fear days  → Low-win-rate traders: reduce or pause trading
Impact     → Capture additional upside on Greed periods; limit Fear-day losses
```

---

## 🤝 Contributing

Suggestions for improvement? Feel free to open an issue or create a pull request!

---

## 📄 License

MIT License — free to use and modify for educational or commercial purposes.

---

## 📧 Contact

For questions or collaboration inquiries, reach out to [SHUBHDEEP11103](https://github.com/SHUBHDEEP11103).

---

**Last Updated:** March 2026  
**Status:** Analysis Complete ✓
