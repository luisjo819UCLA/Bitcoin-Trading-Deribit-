# Deribit Options Data Analysis Project

This project retrieves, cleans, and analyzes options trading data from Deribit's API. It calculates options Greeks, performs implied volatility analysis, and executes synthetic delta hedging strategies to manage risk in options trading for Bitcoin. The analysis and dynamic hedging aim to evaluate market conditions and test strategy robustness over time.

## Table of Contents
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Data Filtering and Preprocessing](#data-filtering-and-preprocessing)
- [Implied Volatility and Delta Hedging](#implied-volatility-and-delta-hedging)
- [Black-Scholes Formula Overview](#black-scholes-formula-overview)
- [Deribit Fees](#deribit-fees)
- [Limitations](#limitations)
- [Contributing](#contributing)
- [License](#license)

## Features
- **Data Collection and Cleaning**: Retrieve and filter historical options data, removing outliers and specific trade types (combo trades, block trades, liquidations).
- **Options Pricing and Delta Calculation**: Compute delta values for options using the Black-Scholes-Merton model and assess moneyness and time to maturity.
- **Implied Volatility Analysis**: Analyze implied volatility patterns and apply filters for accurate market sentiment.
- **Synthetic Delta Hedging**: Execute dynamic delta hedging strategies with simulated transaction costs for different strike prices and maturities.
- **Data Visualization**: Plot index prices, implied volatility, delta values, and hedging performance.

## Installation
### Prerequisites
- Python 3.7 or higher
- Required libraries: `numpy`, `pandas`, `scipy`, `matplotlib`, `requests`, `websocket-client`, `scikit-learn`, `joblib`

Install these packages via:
```bash
pip install numpy pandas scipy matplotlib requests websocket-client scikit-learn joblib
```

## Usage
### Data Collection and Filtering
This project initially collects options trading data using both WebSocket and HTTPS API requests to Deribit. The data is then filtered for specific trade types to ensure reliable implied volatility (IV) analysis. Once filtered, data is merged into a single DataFrame for further processing.

### Implied Volatility and Dynamic Hedging
To perform dynamic hedging:
1. Calculate delta using Black-Scholes-Merton for each option.
2. Track delta adjustments to maintain delta-neutral positioning.
3. Integrate transaction costs into the synthetic delta hedging strategy.

The code executes hedging for options such as `BTC-26APR24-70000-C`. It compares hedging performance for actual and forecasted IV values, calculating resulting portfolio values to evaluate hedging effectiveness.


## Data Filtering and Preprocessing
For accurate analysis:
- **Combo, Block, and Liquidation Trades**: Exclude trades marked as `combo_id`, `block_trade_id`, or `liquidation` as they may distort IV and delta analysis.
- **Outlier Filtering**: Remove extreme outliers in contract amounts and apply IV range constraints (0 < IV < 500) to filter unrealistic values.
- **Implied Volatility Plots**: Visualize IV distributions and filter by direction (`tick_direction` values: 0 = equal, 1 = uptick, 2 = downtick, 3 = repeated).

## Black-Scholes Formula Overview
The Black-Scholes model is employed for calculating delta and IV values, with the following key parameters:
- **Call Option Price Formula**:
  \[ C = S_0 \cdot N(d_1) - K \cdot e^{-rT} \cdot N(d_2) \]
  where `d1` and `d2` are defined based on stock price `S0`, strike `K`, time to expiration `T`, risk-free rate `r`, and volatility `sigma`.

### Assumptions
1. Efficient markets, no commissions, and constant volatility.
2. Log-normal distribution of stock prices.
3. No dividend impact during option life.

### Limitations
Black-Scholes may not fully represent real markets due to changing volatility, interest rates, and log-normal distribution assumptions.

## Deribit Fees
- **Taker Fee**: 0.03%
- **Maker Fee**: 0.03%
- **Exercise Fee**: 0.015% per contract

These fees are integrated into delta hedging calculations.

## Limitations
- **API Request Limits**: High-frequency requests may yield empty results due to API rate limits.
- **Hedging Precision**: Performance may vary depending on moneyness, time to expiration, and market conditions.
- **Assumption Constraints**: The Black-Scholes model's assumptions may limit applicability in real-time scenarios with volatile changes.

## Contributing
Contributions to improve data accuracy, expand delta hedging capabilities, or add new analysis features are welcome. Please submit a pull request with a description of changes.

## License
This project is licensed under the MIT License. See `LICENSE` for details.

---

