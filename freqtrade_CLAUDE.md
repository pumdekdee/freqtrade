# Claude Project Context: freqtrade

This file provides codebase context for the **freqtrade** repository, used as part of an AI trading-knowledge project.

## Project Overview
* **Name**: freqtrade
* **Language**: Python 3.10+
* **Type**: Open-source cryptocurrency algorithmic trading bot
* **Purpose**: Automated crypto trading via configurable strategies, with support for backtesting, hyperparameter optimization, dry-run (paper trading), and live trading on 20+ exchanges.
* **License**: GPL-3.0 (copyleft — be careful with commercial derivatives)

## Core System Boundaries
* **Exchange connectivity**: built on top of `ccxt` for unified exchange access.
* **Bot modes**: `trade` (live/dry-run), `backtesting`, `hyperopt`, `edge`, `webserver`.
* **Notifications**: Telegram bot integration, REST API, and a web UI (FreqUI).
* **ML extension**: `FreqAI` module for training/serving ML models alongside strategies.

## Repository Structure & Key Directories
* `/freqtrade/` - Core application code
  * `/freqtrade/strategy/` - Strategy interface (`IStrategy`), indicator helpers, hyperopt parameter types
  * `/freqtrade/exchange/` - Exchange wrapper classes (built on ccxt)
  * `/freqtrade/optimize/` - Backtesting engine, hyperopt logic
  * `/freqtrade/freqai/` - FreqAI machine learning module (feature engineering, model training/prediction)
  * `/freqtrade/rpc/` - Telegram bot, REST API, webhook handlers
  * `/freqtrade/data/` - Historical data download & conversion utilities
* `/user_data/` - User-owned directory (created on init): `strategies/`, `config.json`, `notebooks/`, `data/`
* `/tests/` - Unit and integration tests, good reference for expected behavior
* `/docs/` - Full documentation (strategy customization, configuration, hyperopt, FreqAI)

## Standard Operational Commands
* **Install**: `pip install -e .` (or via Docker, recommended by upstream)
* **Create user directory**: `freqtrade create-userdir --userdir user_data`
* **New strategy**: `freqtrade new-strategy --strategy MyStrategy`
* **Download historical data**: `freqtrade download-data --exchange binance --pairs BTC/USDT --timeframes 1h`
* **Backtest**: `freqtrade backtesting --config user_data/config.json --strategy MyStrategy`
* **Hyperopt**: `freqtrade hyperopt --hyperopt-loss SharpeHyperOptLoss --strategy MyStrategy`
* **Dry-run / live**: `freqtrade trade --config user_data/config.json --strategy MyStrategy`

## Standard Strategy Implementation Blueprint
```python
from freqtrade.strategy import IStrategy
import talib.abstract as ta

class MyStrategy(IStrategy):
    timeframe = "1h"
    minimal_roi = {"0": 0.05}
    stoploss = -0.10

    def populate_indicators(self, dataframe, metadata):
        dataframe["rsi"] = ta.RSI(dataframe)
        return dataframe

    def populate_entry_trend(self, dataframe, metadata):
        dataframe.loc[(dataframe["rsi"] < 30), "enter_long"] = 1
        return dataframe

    def populate_exit_trend(self, dataframe, metadata):
        dataframe.loc[(dataframe["rsi"] > 70), "exit_long"] = 1
        return dataframe
```

## AI Assistant Guidelines for this Project
1. **Strategy edits**: always implement strategies as a subclass of `IStrategy` inside `user_data/strategies/`, never modify core `/freqtrade/` files for custom logic.
2. **Indicators**: prefer `talib.abstract` or `pandas-ta` style vectorized indicator calculations inside `populate_indicators`.
3. **Backtest first**: any new strategy logic should be paired with a suggested `freqtrade backtesting` command before recommending live/dry-run.
4. **Config awareness**: remind users that exchange credentials, pair whitelist, and risk settings live in `config.json`, not in strategy code.
5. **License note**: flag GPL-3.0 implications if the user plans commercial/closed-source distribution.
