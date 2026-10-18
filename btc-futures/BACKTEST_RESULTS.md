# BTC Futures — Backtest results

ثبت آزمایش‌های TradingView برای مقایسه کانفیگ‌ها. نتیجه‌ها فقط از بک‌تست یا شواهد واقعی کاربر ثبت می‌شوند؛ مقدارهای نامعلوم با «تأیید نشده» مشخص می‌شوند.

## Baseline — strategy.pine v1.9

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
| 24h inside preceding 48h entry gate | Enabled. Last 96 confirmed M15 highs/lows must fit within **the preceding** 192 M15 bars' extremes. Requires continuous 72h history. Disabled gate permits entries without this check. |
| Chart range lines | Show 3h High/Low (fuchsia), 6h High/Low (blue), 24h High/Low (aqua) and previous non-overlapping 48h High/Low (yellow); each pair separately toggled, default ON |
| New 3h/6h level rejection strategy | OFF by default. When ON it replaces EMA/Donchian signals; 3h and 6h entry sources each enabled by default and can be disabled. Strict monthly+weekly direction still required. |
| 3h/6h proximity threshold | 0.15% default from touched historical level to the confirmed 15m close; Long touch low and close back above, Short touch high and close below. |
| Historical level windows | 3h: previous 12 M15 candles [1..12]; 6h: previous 24 [1..24]; exclude signal candle and fail closed when data incomplete. |
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
| BT-004 | v1.7 | Binance BTCUSDT (chart Bitcoin/TetherUS, likely spot; exact ticker unverified) | Sep 28, 2026 – Oct 18, 2026 (selector timezone unverified) | Donchian M15, 5m chart; 50% allocation; L/S 1x; TP 2%, SL 1%; opposite-profitable-5m exit OFF; monthly EMA 3/6; weekly EMA 4/10; signal EMA 2/5, RSI 9 (Long min 8, Short max 4), Donchian 17; Spike ON (24, 2x); RVOL ON (14 days, >=0.8); Delta OFF; 24h range ON 0.4% | 6 | 50.00 (3/6) | +1.25 shown in Key stats | 1.674 | 1.53 | Expectancy +0.19 USD / 0.40% as displayed | Gross profit 2.83 USD, gross loss 1.69 USD; all six closed trades LONG; possible open PnL/report timing difference; see details below |
| — | v1.9 | — | — | Baseline v1.9, new 3h/6h lines ON, range-rejection strategy OFF; historical user results BT-001–BT-004 unchanged | — | — | — | — | — | — | Not yet tested |

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

## BT-004 — Donchian, 5m profitable exit OFF, TP 2% / SL 1% (2026-10-18)

- **Evidence:** four user screenshots of `BTC Futures Bot | FA | v1.7`, Binance chart `BTCUSDT` at 5m, date selector **Sep 28–Oct 18, 2026** with **100 USD** initial capital. This looks like *Bitcoin / TetherUS* **spot**, not the perpetual instrument; exact ticker and Strategy Properties commission/margin settings were not provided.
- **Visible configuration:** Donchian breakout of confirmed M15 signal; Long/Short allowed; **profit-only opposite 5m candle exit OFF**; allocation **50%**; long leverage **1x**, short leverage **1x**; TP **2%** and SL **1%**; monthly trend EMAs **3/6**, weekly **4/10**; M15 signal EMA **2/5**, RSI length **9**, Long RSI min **8**, Short RSI max **4**, Donchian **17**. Volume Spike **ON**: average **24**, maximum **2x**; RVOL **ON**, previous **14** days, threshold **0.8**; Delta **OFF**; 24h high/low proximity **ON** with **0.4%** threshold. Inputs dialog is open in screenshots; values were visible but whether any last-second edits had been committed using OK remains uncertain. The legend also supports several settings.
- **Key stats (first screenshot):** **Total PnL +1.25 USD (+1.25%)**, **maximum drawdown 1.56 USD (1.53%)**, **3 winners / 6 closed trades (50%)** over the period. **Distribution/Breakdown (fourth screenshot):** gross profit **2.83 USD**, gross loss **1.69 USD**, Profit Factor **1.674**, displayed *Expectancy* **0.19 USD / 0.40%**. All signal PnL appears on `L` (Long) in the Breakdown panel; no Short trade is shown.
- **Data reconciliation warning:** The displayed first-screenshot total PnL of **1.25 USD** is not equal to the difference between reported gross profit and gross loss (**2.83−1.69 = 1.14 USD**). This may reflect an open position/unrealized PnL, chart report states at different instants, or another reporting detail; **not verified**. The fourth screenshot's right-hand All signals value also appears near **+1.22 USD**, so do not assume every value represents precisely the same calculation snapshot. Capture **Overview, List of Trades, open-position details, and Properties** before reconciling.
- **Comparison with BT-003:** v1.7 on a similar Binance BTCUSDT 5m chart and Sep 28–Oct 18 window yielded **18 trades, 94.44% wins, net +1.22%, Profit Factor 2.293, drawdown 1.08%**, with exit ON, TP 1%, Spike 15/3x. **BT-004 changed multiple variables at once**: exit ON→OFF, TP 1→2%, Spike 15/3x→24/2x, plus signal EMA/RSI/Donchian details not previously verified. Hence this is **not** a controlled experiment isolating the 5m exit rule.
- **Leverage troubleshooting:** This run visibly uses **1x for both sides**, so it does **not** establish whether 2x can trade with otherwise identical settings. For 2x, keep all other Inputs and Properties unchanged, change only leverage, and capture Strategy Properties + exact symbol specification. No strategy code changes were made for this log entry.
- **Next experiment:** on a fixed symbol (ideally `BINANCE:BTCUSDT.P`, once a separate futures baseline is established) and fixed date range, start with this BT-004 config and toggle **only profitable opposite-5m exit OFF/ON**, then compare trades, realized Net PnL, Profit Factor, drawdown and equity. Next test 1x vs 2x **changing only leverage**. Hold commissions, margin settings, slippage and instrument constant.

## v1.8 verification plan (not a backtest result)

- **Non-overlapping periods:** 24h = 96 recent confirmed M15 bars [0..95]; comparison 48h = the immediately preceding 192 M15 bars [96..287]. A conventional last-48h window that *includes* the last-24h window would make the containment check always true; this design deliberately avoids that tautology.
- **TradingView review:** Set standard chart to **5m**; wait for sufficient continuous 15m history (288 bars). Verify four stair-step lines (aqua 24h, yellow older 48h), dashboard status and long/short entry gate.
- **Logic cases:** (1) High24 100, Low24 90; HighPrev48 110, LowPrev48 80 => allowed. (2) High24 115 => blocked. (3) Low24 75 => blocked. (4) either boundary equal => allowed. (5) missing or gapped 72h history => blocked if enabled, bypassed if disabled.
- **Isolation test:** Compare v1.8 gate ON/OFF on the same symbol, 5m chart, period, other inputs, commission, slippage and margin. Disabling the *separate* near-24h-extreme filter for this controlled test helps distinguish which gate suppressed a potential entry.
- **No results claim:** Pine compilation and Strategy Tester results remain unverified until the user tests them. Existing BT-001 through BT-004 remain historical and must not be re-labelled as v1.8.

## v1.9 level-entry validation plan — no results claimed

- **New source windows:** 3h = 12 confirmed M15 bars [1..12]; 6h = 24 confirmed M15 bars [1..24]. The signal candle [0] is not part of these levels, preventing automatic inclusion of its own new high/low.
- **Entry scenarios:** (1) prior 3h low = 80,000, signal 15m low = 79,990, close = 80,050 => touches and closes 0.0625% above → eligible Long if monthly+weekly bullish; (2) prior 6h high = 82,000, signal high = 82,010, close = 81,930 => touches and closes 0.0854% below → eligible Short if monthly+weekly bearish. Both still require all enabled independent gates, sufficient qty and correct 5m chart.
- **Invalid cases:** a touch without a close back inside the previous level, close farther away than 0.15%, inconsistent monthly/weekly trend, missing 15m bars, or both 3h/6h source switches disabled → no level-model signal.
- **A/B tests:** Compare identical symbol, historical period, leverage, commission, slippage and unrelated filters with `useLevelEntry=OFF` vs `ON`. Within ON, compare sources 3h-only / 6h-only / both, and proximity thresholds 0.05% / 0.15% / 0.25%, one dimension at a time.
- **Risk of overlap:** The independent 24h-extreme avoidance gate and 24-in-prior-48 gate can block a valid 3h/6h level signal; document the state of each toggle in every experiment.
- **Status:** Pine compilation in the TradingView editor and Strategy Tester results for v1.9 are NOT yet verified. Existing BT-001–BT-004 remain historical and must not be labelled as v1.9.

## Reporting format

برای هر اجرای بک‌تست یک Run ID بدهید و این داده‌ها را بفرستید. **از v1.7 به بعد حتماً چارت 5m بگذارید، سیگنال ورود همچنان M15 است و خروج با کندل مخالف فقط در صورت سود باز مثبت انجام می‌شود. نتایج نسخه‌های v1.5/v1.6 با اجرا روی 15m را بدون ذکر تفاوت تایم‌فریم با v1.7 مستقیم مقایسه نکنید.** برای ارزیابی اثر فیلتر جدید، یک آزمایش با **24h proximity ON / 1.0%** و یک آزمایش با **OFF**، روی همان نماد، بازه و سایر تنظیمات اجرا کنید:


- نماد دقیق، صرافی، تاریخ آغاز و پایان و تایم‌فریم، نوع نمودار (کندل استاندارد یا غیره).
- نسخهٔ کد، مدل سیگنال، فیلترهای روشن/خاموش، پارامترهای تغییرکرده، اهرم/سرمایه و TP/SL.
- Strategy Properties: initial capital، order size، commission، slippage، و Bar Magnifier / fill settings.
- نتایج: تعداد معاملات بسته‌شده، درصد برد، سود/زیان خالص (USD و %)، Profit Factor، Max Drawdown، Avg trade و در صورت امکان آمار Long/Short.
- اسکرین‌شات Overview، Performance Summary، List of Trades یا خروجی CSV؛ مشخص کنید اطلاعات In-sample یا Out-of-sample است.

برای جلوگیری از overfitting، ابتدا آزمایش‌ها را روی دوره ثابت انجام دهید، هر بار فقط یک گروه پارامتر را تغییر دهید و سپس روی دوره خارج از نمونه اعتبارسنجی کنید.
