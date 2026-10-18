# BTC Levels 1m — Experimental backtests

**وضعیت:** هیچ نتیجه واقعی هنوز ثبت نشده است. این دفتر مستقل از `../btc-futures/BACKTEST_RESULTS.md` است.

## Baseline v1.0

| Setting | Default |
| --- | --- |
| Source | `btc-levels-1m/strategy.pine` v1.0 |
| Instrument | BTC linear USD/USDT perpetual (record exact ticker) |
| Chart | 1m / standard candles only |
| Rolling high/low windows | 3h, 6h, 9h, 12h, 24h, 48h; 12 plotted levels, signal bar excluded |
| Entry periods | All 6 enabled |
| Require close near any selected line | ON, within 0.05% of price level |
| Direction | Confirmed last DAILY close > prior daily close AND WEEKLY close > prior weekly close = Long; both < = Short |
| Minimum bars between entries | 15 |
| Take-profit / Stop-loss price | 0.30% / 0.60% |
| Initial capital / allocation | $1000 / 50% equity |
| Leverage | Long 1x / Short 1x |
| Margin requirement | 10% per side in code (Properties may override) |
| Commission / Slippage | 0.05% each side / 2 ticks |
| Historical data | At least 2880 continuous prior 1m bars for enabled 48h window |
| Strategy Tester/Pine compilation | NOT YET VERIFIED |

## Experiment log

| Run ID | Version | Exact symbol | Date range UTC | Changes from baseline | Trades | Win Rate (after costs) | Net PnL % | Profit Factor | Max DD % | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| — | v1.0 | — | — | Baseline | — | — | — | — | — | Not yet tested |

## Controlled tests to run

1. On a fixed, verified BTC perpetual symbol and a stable historical range, check Pine compilation, drawn lines, allowed trend direction and whether the strategy emits any trades.
2. With all other Inputs/Properties identical: require-close-near-level **ON vs OFF**, and log order counts/costs. OFF can generate excessive trades; use caution.
3. With the requirement ON, enable **only one** horizon at a time (3h/6h/9h/12h/24h/48h), then enable all six.
4. Explore distance thresholds 0%, 0.02%, 0.05% and 0.10%, altering only this setting at a time. An exact 0% may generate no trades.
5. Test TP/SL changes separately and evaluate net win rate, Profit Factor, largest loss, margin calls and out-of-sample stability. Record actual `strategy.commission`, `slippage`, `syminfo.mincontract` and instrument market type.

For each run, attach the TradingView **Overview, Performance Summary, List of Trades** or exported CSV and screenshots of Inputs + Properties. Never invent missing results.
