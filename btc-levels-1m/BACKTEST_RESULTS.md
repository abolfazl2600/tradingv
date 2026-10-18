# BTC Levels 1m — Experimental backtests

**وضعیت:** هیچ نتیجه واقعی هنوز ثبت نشده است. این دفتر مستقل از `../btc-futures/BACKTEST_RESULTS.md` است.

## Baseline v1.2

| Setting | Default |
| --- | --- |
| Source | `btc-levels-1m/strategy.pine` v1.2 |
| Instrument | BTC linear USD/USDT perpetual (record exact ticker) |
| Chart | 1m / standard candles only |
| Rolling high/low windows | 1h, 3h, 6h, 9h, 12h, 24h, 48h; 14 plotted levels, signal bar excluded |
| Triangle overlay | ON; base 48h high/low at now-1440, apex latest 24h midpoint at now; 90% transparent; green/bull if Mid24 > Mid48, red/bear if below, gray/neutral if equal |
| Triangle direction filter | ON (separate from display). Bullish permits Long only, bearish permits Short only; neutral or insufficient 48h history blocks both. Requires confirmed matching D+W. |
| Entry periods | All 7 enabled, including new 1h |
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
| — | v1.2 | — | — | 1h lines and optional aligned triangle entry gate; no verified results | — | — | — | — | — | Not yet tested |

## v1.2 validation plan (no observed backtest results)

- On a standard 1-minute BTC chart with at least 48 hours of complete history, inspect the schematic triangle from current rolling 48h high/low levels (left base, shown 24h ago) to the midpoint of the current 24h high/low levels (apex at last bar).
- Switch ONLY triangle display ON/OFF and change opacity; trading results must remain unchanged when the triangle direction FILTER remains fixed. With insufficient data or on a non-1m chart the triangle should not appear.
- Realtime updates should reuse four lines + fill + direction label without allocating duplicates. Pine compile and visual rendering still require TradingView verification.

## Additional v1.2 test cases — not yet verified

- **1h:** 60 prior completed M1 bars define high1/low1. Signal and display toggles are independent. A close near an enabled 1h level qualifies for the level gate.
- **Triangle bullish:** High48=110, Low48=90, Mid48=100; High24=110, Low24=100, Mid24=105: green. Only Long can pass triangle filter, and only if daily+weekly bullish.
- **Triangle bearish:** High48=110, Low48=90, Mid48=100; High24=100, Low24=90, Mid24=95: red. Only Short can pass triangle filter, and only if daily+weekly bearish.
- **Neutral / data missing:** Mid24=Mid48, or no 48h history: block both entries with the filter ON; allow direction from confirmed D+W alone with filter OFF.
- **Toggle independence:** disable triangle *display* while filter stays ON, and vice versa; demonstrate expected entry counts. Disabling the near-line entry requirement must not bypass the triangle gate.
- Compare with fixed symbol, date span, slippage, commission, TP/SL and size. Test the triangle filter ON/OFF as the only altered variable, and 1h line source ON/OFF as a separate test.

## Controlled tests to run

1. On a fixed, verified BTC perpetual symbol and a stable historical range, check Pine compilation, drawn lines, allowed trend direction and whether the strategy emits any trades.
2. With all other Inputs/Properties identical: require-close-near-level **ON vs OFF**, and log order counts/costs. OFF can generate excessive trades; use caution.
3. With the requirement ON, enable **only one** horizon at a time (1h/3h/6h/9h/12h/24h/48h), then enable all six.
4. Explore distance thresholds 0%, 0.02%, 0.05% and 0.10%, altering only this setting at a time. An exact 0% may generate no trades.
5. Test TP/SL changes separately and evaluate net win rate, Profit Factor, largest loss, margin calls and out-of-sample stability. Record actual `strategy.commission`, `slippage`, `syminfo.mincontract` and instrument market type.

For each run, attach the TradingView **Overview, Performance Summary, List of Trades** or exported CSV and screenshots of Inputs + Properties. Never invent missing results.
