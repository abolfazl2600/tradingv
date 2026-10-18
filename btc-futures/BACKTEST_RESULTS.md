# BTC Futures — Backtest results

ثبت آزمایش‌های TradingView برای مقایسه کانفیگ‌ها. نتیجه‌ها فقط از بک‌تست یا شواهد واقعی کاربر ثبت می‌شوند؛ مقدارهای نامعلوم با «تأیید نشده» مشخص می‌شوند.

## Baseline — strategy.pine v1.7

| Setting | Default |
| --- | --- |
| Symbol | BTC linear USD/USDT perpetual — specify exact TradingView symbol |
| Execution chart | 5m, standard candles (required for true 5m exit) |
| Entry timeframe | 15m only, at confirmed close; EMA, RSI, Donchian, spike, RVOL and 24h range in 15m context |
| Confirmed trend | Monthly EMA 6/12 + Weekly EMA 10/30 (both aligned) |
| Entry signal | EMA crossover 21/55; RSI 14; Long RSI ≥52, Short RSI ≤48 |
| Alternative signal | Donchian breakout 20 |
| Long/Short leverage | 1x / 1x |
| Equity allocation | 95% |
| TP/SL (price move) | 0.5% / 1.0% |
| Profitable opposite 5m candle exit | Enabled; opposite 5m close AND strategy.openprofit > 0 (gross unrealized PnL) |
| 24h high/low proximity block | Enabled; no new entry when confirmed 15m close is within 1.0% of either rolling 24h extreme (96 confirmed bars, including signal bar) |
| Volume Spike | Enabled; at most 3 × SMA(volume, 20)[1] |
| RVOL | Enabled; minimum 0.8; 14 prior same-time daily slots |
| Estimated Delta | Enabled; minimum 10 one-minute candles; 80% volume coverage |
| Initial capital | 100 USD |
| Commission / side | 0.05% |
| Slippage | 2 ticks |
| Bar Magnifier | Record whether enabled |
| Backtest date interval | Record UTC range |

## Experiment log

| Run ID | Code version | Symbol/exchange | Backtest period | Config changes | Trades | Win rate % | Net profit % | Profit factor | Max drawdown % | Avg trade | Evidence / Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| BT-001 | v1.5 | Binance BTC/USDT perpetual (exact ticker unverified) | 2026-08-01 to 2026-10-18 (date selector; timezone unverified) | 15m EMA cross; equity 95%; L 3x / S 1x; TP 1.0%, SL 1.0%; opposite-candle exit ON; monthly EMA 6/12, weekly EMA 10/30; **no 24h filter in v1.5** | 21 | 33.33 | +0.43 | 1.381 | 0.89 | Not reported | User screenshot (2026-10-18); partial config; see details below |
| BT-002 | v1.7 | Binance BTCUSDT perpetual (TradingView display BTCUSDT.P; exact ticker to confirm) | Not shown in screenshot | 5m chart; 50% allocation; 1x/1x; TP 0.5%, SL 0.5%; monthly EMA 1/2 and weekly EMA 4/10 shown in open Inputs (application unverified); opposite 5m profitable exit checked | 0 (as displayed) | N/A | N/A | N/A | N/A | N/A | No trades; monthly regime shown neutral, weekly bullish. Details below |
| BT-003 | v1.7 | Binance BTCUSDT (chart says Bitcoin/TetherUS; likely spot; verify) | Sep 28, 2026 – Oct 18, 2026 (date selector, timezone unverified) | Donchian M15; chart 5m; allocation 50%; leverage L/S 1x/1x; TP/SL 1%/1%; monthly EMA 3/6, weekly EMA 4/10; opposite profitable 5m exit ON; Spike ON (15,3x); RVOL ON (14 days, >=0.8); Delta OFF; 24h high/low ON (0.4%) | 18 | 94.44 (17/18) | +1.22 | 2.293 | 1.08 | Expectancy +0.04 USD (+0.07%) shown on Distribution | New screenshots 2026-10-18: Net PnL +1.22 USD, Gross Profit 2.17 USD, Gross Loss 0.94 USD, Buy & Hold -2.43%, 0 margin calls; see details below |
| — | v1.7 | — | — | Baseline, execution 5m / entry M15, 1x/1x, profitable 5m reverse-candle exit ON | — | — | — | — | — | — | Not yet tested |

## BT-001 — Screenshot-derived details (2026-10-18)

- **Version:** `BTC Futures Bot | FA | v1.5` (visible both in chart legend and Strategy Tester). This is **not** the newer v1.6 filter evaluation.
- **Market:** TradingView displays *Bitcoin / TetherUS PERPETUAL CONTRACT · 15 · Binance*; exact exchange symbol ID and whether standard candles were used were not supplied explicitly.
- **Selected testing dates:** Aug 1, 2026 — Oct 18, 2026; date-selector timezone is not shown.
- **Visible settings:** EMA crossover, enable Long/Short, exit on opposite closed M15 candle enabled, capital allocation 95%, Long leverage 3x, Short leverage 1x, TP 1% of price, SL 1% of price, monthly EMA 6/12 and weekly EMA 10/30. Other filters' enabled/disabled states, Strategy Properties overrides, and whether edits in the open Inputs dialog have been applied are **not independently verified**.
- **Observed outputs:** initial capital 100 USD; net PnL +0.43 USD (+0.43%); gross profit +1.57 USD; gross loss 1.14 USD; 7 winning / 21 total closed trades (33.33%); Profit Factor 1.381; max drawdown 0.90 USD (0.89%); Buy & Hold +22.98% as displayed.
- **Not captured:** List of Trades, individual entry/exit reasons, full active-filter states, Commission/Slippage from Properties, Bar Magnifier, average trade, out-of-sample evaluation. Do **not** treat these unknowns as confirmed defaults.
- **Comparison warning:** BT-001 uses different short leverage and TP than v1.5/v1.6 defaults. To isolate the new 24h exclusion filter, hold *all other Inputs and Properties* identical between v1.6-filter-ON and v1.6-filter-OFF trials.

## BT-002 — No trades / input troubleshooting (2026-10-18)

- **Source:** user screenshot of `BTC Futures Bot | FA | v1.7` on a Binance BTCUSDT perpetual 5-minute chart. Strategy Tester shows **“Trades are required to see strategy report”** and no recorded trades. Exact backtest date range is not visible.
- **Visible open Inputs values:** 50% equity allocation, Long leverage 1x, Short leverage 1x, TP 0.5%, SL 0.5%, monthly EMA fast 1 / slow 2, weekly EMA fast 4 / slow 10, Long/Short enabled and profitable opposite 5m candle exit enabled. Because the Inputs dialog was still open, **whether edited values had been committed with OK is not fully verifiable**.
- **Chart dashboard:** monthly **neutral**, weekly **bullish**, trade mode **halted**, currently no position; volume spike, RVOL and Delta displayed as off; 24-hour extreme filter says close to 24h high. Dashboard reflects active chart settings, which may differ from unsaved dialog edits.
- **Definitive invalid configuration if applied:** monthly fast EMA length **1** equals the last confirmed monthly Close. Strict conditions `mClose > mFast` and `mClose < mFast` can never hold, so both Long and Short directional gates remain closed. Weekly EMA fast=1 would create the same issue.
- **Possible independent quantity gate:** with initial capital 100 USD, allocation 50%, leverage 1x and a BTC price ~81,700 USD, order size is approximately 0.00061 BTC, then rounded down to `syminfo.mincontract`. If the contract minimum is 0.001 BTC, the computed order size becomes zero and is rejected; **actual `syminfo.mincontract` has not been verified**.
- **Next controlled trial:** monthly EMA 2/4, weekly EMA 4/10; use initial capital 1000 USD and keep 50% allocation / 1x / TP 0.5% / SL 0.5%; confirm Inputs with OK and backtest range. This is **a debugging configuration, not evidence of profitability**. Compare on the same date window only after confirming any order-size constraint.

## BT-003 — Donchian breakout, high win rate, small expected profit (2026-10-18)

- **Evidence:** three screenshots sent by the user. TradingView shows `BTC Futures Bot | FA | v1.7` on a `BTCUSDT` 5-minute Binance chart. Title is *Bitcoin / TetherUS · 5 · Binance* (no "PERPETUAL CONTRACT" in the title, unlike BT-001). This may be the **spot chart**, not `BTCUSDT.P`; verify the exact chart ticker **before treating this as a futures test**.
- **Date selector:** Sep 28, 2026 – Oct 18, 2026; timezone not visible. Initial capital shown in Strategy Tester: 100 USD.
- **Trade stats (updated from three further screenshots, 2026-10-18):** 18 total trades; 17 winners (94.44%), 1 loser (5.56%), 0 breakevens. **Net PnL +1.22 USD (+1.22%)**, **Profit Factor 2.293**, **Max Drawdown 1.08 USD (1.08%)**, **Gross Profit 2.17 USD**, **Gross Loss 0.94 USD**. Buy & Hold **-2.43%**, strategy outperformance **+3.65 percentage points**. Earlier Distribution screenshot displayed *Expectancy +0.04 USD / +0.07%* and *Average profit 0.08%*; these fields come from that different report section and should not be conflated with net profit divided by trade count. Commission breakdown, per-trade loss and actual order fills still unverified.
- **Visible Inputs (open dialogs):** model Donchian channel breakout (15m signal); Long and Short permitted; profitable exit on opposite closed 5m candle ON; allocation 50% of equity; L/S leverage 1x/1x; TP 1%, SL 1%; monthly EMA fast 3/slow 6, weekly EMA fast 4/slow 10 (weekly slow appears in strategy label; confirm manually); spike filter ON, average 15 candles, max 3x; RVOL ON with 14 same-time days, minimum 0.8; estimated Delta OFF; near 24h high/low exclusion ON at 0.4%.
- **Live snapshot on screenshot:** dashboard monthly bullish, weekly bullish, **Long only** permitted, no open position, opposite-5m-profitable exit enabled, volume spike normal, RVOL **0.17**, Delta off and 24h range entry permitted. RVOL 0.17 is **below** entry threshold 0.8 at the snapshot, but this does not retroactively invalidate the 18 historical trades.
- **Verification limits:** Open Inputs windows show values but may include unsaved changes; chart legend/dashboard corroborate several values. The exact Donchian, RSI, 15m EMA lengths and Strategy Properties commission/slippage/Bar Magnifier not independently documented. Profit figures are not enough to evaluate net risk-adjusted performance; trade list and Overview screenshots are needed.
- **Further diagnostic screenshots:** Margin Usage shows **average margin used 3.34 USD**, **margin efficiency 0.37 USD**, **margin calls 0**, and a utilization plot; Streaks show longest win streak **15 trades**, longest losing streak **1 trade**. **These data do not establish why a separate 2x-leverage run may have zero trades**: Properties margin settings and the actual symbol contract specification at 2x remain unknown. Zero margin calls in this run does not imply rejected-entry orders could not occur in another run.
- **Next controlled comparisons:** On the same **perpetual** symbol, historical window, Properties and other Inputs, run v1.7 with the profitable opposite-5m exit ON vs OFF (one variable changed). Capture Net Profit, Profit Factor, Max Drawdown, winners/losers and CSV List of Trades to understand tiny average profits and the single losing trade. Separately test RVOL/24h filters only after establishing this baseline. A win rate of 94.44% from 18 trades does **not** establish robust profitability.

## Reporting format

برای هر اجرای بک‌تست یک Run ID بدهید و این داده‌ها را بفرستید. **از v1.7 به بعد حتماً چارت 5m بگذارید، سیگنال ورود همچنان M15 است و خروج با کندل مخالف فقط در صورت سود باز مثبت انجام می‌شود. نتایج نسخه‌های v1.5/v1.6 با اجرا روی 15m را بدون ذکر تفاوت تایم‌فریم با v1.7 مستقیم مقایسه نکنید.** برای ارزیابی اثر فیلتر جدید، یک آزمایش با **24h proximity ON / 1.0%** و یک آزمایش با **OFF**، روی همان نماد، بازه و سایر تنظیمات اجرا کنید:


- نماد دقیق، صرافی، تاریخ آغاز و پایان و تایم‌فریم، نوع نمودار (کندل استاندارد یا غیره).
- نسخهٔ کد، مدل سیگنال، فیلترهای روشن/خاموش، پارامترهای تغییرکرده، اهرم/سرمایه و TP/SL.
- Strategy Properties: initial capital، order size، commission، slippage، و Bar Magnifier / fill settings.
- نتایج: تعداد معاملات بسته‌شده، درصد برد، سود/زیان خالص (USD و %)، Profit Factor، Max Drawdown، Avg trade و در صورت امکان آمار Long/Short.
- اسکرین‌شات Overview، Performance Summary، List of Trades یا خروجی CSV؛ مشخص کنید اطلاعات In-sample یا Out-of-sample است.

برای جلوگیری از overfitting، ابتدا آزمایش‌ها را روی دوره ثابت انجام دهید، هر بار فقط یک گروه پارامتر را تغییر دهید و سپس روی دوره خارج از نمونه اعتبارسنجی کنید.
