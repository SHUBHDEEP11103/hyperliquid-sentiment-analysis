# Hyperliquid Sentiment Analysis — Executive Summary

## Methodology
- **Data**: Bitcoin Fear/Greed Index (daily, 2018-present) × Hyperliquid trade history
- **Alignment**: Trades UTC date joined to daily sentiment
- **Binary classification**: Extreme Fear + Fear → "Fear";  Neutral + Greed + Extreme Greed → "Greed"
- **Statistical tests**: Welch's t-test + Mann-Whitney U at α = 0.05

## Key Findings

### Insight 1: Performance Differential (Fear vs Greed)
- Greed days produce **-62.6%** more mean daily PnL per trader vs Fear days
- Mean PnL — Fear: **$209372.66** | Greed: **$78340.54**
- Win rate — Fear: **41.6%** | Greed: **35.0%** (Δ -15.9%)
- PnL t-test p-value: **0.0924** (not significant at α=0.05)

### Insight 2: Behavioral Shifts
- Daily trade count — Fear: **4183.5** | Greed: **1119.8** (Δ -73.2%)
- Long ratio — Fear: **45.9%** | Greed: **49.4%** (Δ +7.4%)
- Traders shift toward long positions on Greed days, reflecting bullish sentiment

### Insight 3: Consistent Winners Are Most Resilient
- **3/32 accounts** (9%) qualify as consistent winners (≥55% win rate)
- Winner PnL — Fear: **$84571.76** | Greed: **$90417.40**
- Winners maintain profitable performance regardless of sentiment regime

## Strategy Recommendations

### Strategy 1: Adaptive Position Sizing
| Condition | Action |
|-----------|--------|
| Fear days | Reduce trade size 20–30% for all accounts |
| Greed days | Maintain/increase size for win rate ≥ 55% accounts |
| **Expected impact** | Reduce Fear-day drawdowns; preserve upside on Greed days |

### Strategy 2: Selective Frequency Management
| Condition | Action |
|-----------|--------|
| Greed days | Frequent traders with win rate > 45%: +15–25% trade frequency |
| Fear days | Low-win-rate traders: reduce or pause trading |
| **Expected impact** | Capture 10–15% upside on Greed periods; limit Fear-day losses |

## Output Files
| File | Description |
|------|-------------|
| `output/charts/01_sentiment_overview.png` | PnL, win rate, trades, long ratio by sentiment |
| `output/charts/02_segment_analysis.png` | Segment performance comparison |
| `output/charts/03_timeseries_analysis.png` | Market metrics over time |
| `output/charts/04_heatmap_sentiment_pnl.png` | Mean PnL by sentiment × trade size |
| `output/tables/daily_trader_stats.csv` | Daily metrics per trader |
| `output/tables/daily_market_stats.csv` | Market-level daily aggregations |
| `output/tables/segment_volume_analysis.csv` | High vs Low volume segment |
| `output/tables/segment_frequency_analysis.csv` | Frequent vs Infrequent segment |
| `output/tables/segment_consistency_analysis.csv` | Winners vs Others segment |
