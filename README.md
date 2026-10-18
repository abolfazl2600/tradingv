# TradingView strategies — tradingv

مخزن اختصاصی استراتژی‌ها، مستندات و بک‌تست‌های **TradingView / Pine Script**.

## Strategies

- **[BTC Levels 1m Bot](btc-levels-1m/README.md)** — Independent Pine Script v6 one-minute strategy with twelve high/low levels (3h–48h), daily+weekly trend gating, and configurable TP/SL.
  - [Source code](btc-levels-1m/strategy.pine)
  - [Separate backtest journal](btc-levels-1m/BACKTEST_RESULTS.md)

- **[BTC Futures Bot](btc-futures/README.md)** — Pine Script v6, current version **v1.9**.
  - [TradingView strategy source](btc-futures/strategy.pine)
  - [Backtest results / experiment log](btc-futures/BACKTEST_RESULTS.md)

## Scope

- این مخزن فقط شامل کد Pine Script، مستندات و داده‌های مرتبط با TradingView است.
- کد اجرایی Python/MetaTrader و سرویس‌های انتشار سفارش یا صرافی جزو این پروژه نیستند.
- **No GitHub Actions workflows or CI/CD automation is included or planned in this repository.**
- تمام مقادیر بازده صرفاً نتایج بک‌تست هستند و تضمین عملکرد واقعی نیستند.

## Migration

Source: [`metatr/tradingview/btc-futures`](https://github.com/abolfazl2600/metatr/tree/main/tradingview/btc-futures).
The latest Pine strategy, Persian documentation and historical experiment log (including BT-001) were moved to [`btc-futures/`](btc-futures/). Historical pull requests/commits stay in the original repository; subsequent changes belong here.

## Quick start — original BTC Futures Bot

For the new **one-minute** bot, use [the separate instructions](btc-levels-1m/README.md).



1. Open a linear BTC USD/USDT perpetual instrument in TradingView.
2. Use a **standard 5-minute candle chart** for this version; entry decisions use confirmed **15-minute** signals and opposite-candle exits are evaluated every **5 minutes**.
3. Paste [`btc-futures/strategy.pine`](btc-futures/strategy.pine) into Pine Editor, add to chart and inspect Strategy Tester before any real-world use.
