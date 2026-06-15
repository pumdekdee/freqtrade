# Claude Project Context: Freqtrade Custom Bot

This file provides comprehensive context regarding this specific Freqtrade cryptocurrency trading bot repository for Claude AI.

## Project Overview
* **Name**: Freqtrade (Customized Fork)
* **Language**: Python 3.11+ (accounts for 98.5% of the codebase)
* **Type**: Free and open-source crypto trading bot
* **Database**: SQLite (for persistence and trade tracking)
* **Interfaces**: Telegram RPC and Built-in WebUI (FreqUI)

## System & Software Requirements
* **Python Version**: `>= 3.11`
* **Core Dependencies**: `pip`, `git`, `TA-Lib`, `virtualenv`
* **Infrastructure**: Cloud instance with minimum 2GB RAM, 1GB disk space, 2vCPU. 
* **Critical Rule**: System clock must be synchronized to an NTP server frequently to avoid exchange communication lag.

## Supported Exchanges
* **Spot**: Binance, BingX, Bitget, Bitmart, Bybit, Gate.io, HTX, Hyperliquid (DEX), Kraken, OKX, MyOKX, Bitvavo, Kucoin.
* **Futures (Leverage)**: Binance, Bitget, Gate.io, Hyperliquid (DEX), OKX, Bybit, Kraken.

## Repository Structure & Key Directories
* `/freqtrade/` - Core bot logic, modules, and orchestration.
* `/user_data/` - User-specific data including strategies, hyperopt configurations, and local backtest data.
* `/config_examples/` - Reference configuration templates.
* `/docker/` & `docker-compose.yml` - Docker deployment setups.
* `/docs/` - Project documentation files.
* `/scripts/` - Utility and helper scripts.
* `/tests/` - Test suites for verifying system components.

## Development & Git Workflow
* **Main Branches**:
  * `develop` - Default branch. Contains latest features. **All Pull Requests must target this branch.**
  * `stable` - Production-ready, well-tested stable releases.
* **Contribution Rule**: For major feature changes, an issue must be opened or discussed in the `#dev` Discord channel before implementing.

## Freqtrade CLI Cheat Sheet
When generating or troubleshooting code/scripts, use these primary modules:
* `freqtrade trade` - Run the live/dry-run trading bot.
* `freqtrade create-userdir` - Initialize user data directory.
* `freqtrade new-config` / `new-strategy` - Generate scaffolding templates.
* `freqtrade download-data` - Fetch historical OHLCV data.
* `freqtrade backtesting` / `backtesting-analysis` - Run and evaluate simulations.
* `freqtrade hyperopt` - Optimize strategy parameters using machine learning.
* `freqtrade edge` - Calculate optimal position sizes based on historical win rates.
* `freqtrade webserver` - Host the FreqUI web interface.

## Telegram Interface Reference
The bot responds to the following Remote Procedure Call (RPC) commands:
* `/start` / `/stop` / `/stopentry` - Control overall trading states.
* `/status [trade_id]` - View active positions.
* `/profit` / `/profit_long` / `/profit_short` - View performance metrics over `n` days.
* `/forceexit <trade_id>|all` - Instantly close positions (ignores minimum ROI).
* `/balance` - Check exchange asset distribution.
* `/daily <n>` - Show daily PnL breakdown.

## AI Assistant Guidelines for this Project
1. **Safety First**: Always prioritize safety. Remind the user to run new strategies in **Dry-Run** mode first.
2. **Strategy Code**: When writing custom trading strategies, strictly adhere to Python 3.11 standards, use TA-Lib for indicators, and format the output according to the Freqtrade strategy class specifications (`IStrategy`).
3. **Target Branch**: Ensure all codebase adjustment recommendations are compliant with the `develop` branch ecosystem.
