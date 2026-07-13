# Stock Data Fetching and Train/Test Splitting

A data preparation script that fetches historical price data for selected stock tickers using the `yfinance` library, then splits them into training and test sets.

## What It Does

1. Reads a list of tickers and company names from a CSV file.
2. Randomly splits the ticker list into `80% train / 20% test`.
3. Downloads historical price data for each ticker from Yahoo Finance (default: last 60 days, 5-minute candles, including pre/post-market data).
4. Builds a table containing `Open`, `Close`, and a `Ratio` column calculated as `(Close / Open) - 1` (the return rate) for each row.
5. Saves the results to `train.csv` and `test.csv`.

## Requirements

```bash
pip install yfinance pandas numpy scikit-learn
```

## Input File

Expects a file named `tickers.csv` with the following columns:

| Ticker | Company |
|--------|---------|
| AAPL   | Apple Inc. |
| TSLA   | Tesla Inc. |
| ...    | ... |

> Note: By default the code uses the Kaggle path `/kaggle/input/randon-tickers/tickers.csv`. To run it in your own environment, update the `input_csv` variable with your own file path.

## Usage

Run the notebook cells in order. To change the settings, edit the parameters in the second cell:

```python
period = "60d"       # Lookback range (e.g. 8d, 60d, 730d)
interval = "5m"       # Candle interval (e.g. 1m, 5m, 15m, 1h, 1d)
prepost = True        # Whether to include pre/post-market data
random_state = 55     # Seed for reproducible train/test splitting
```

> Note: The `period` parameter is limited by Yahoo Finance depending on the chosen `interval` (e.g. max `8d` for `1m` interval, max `60d` for other minute-level intervals).

## Output

After running, two files are created in the project directory:

- **`train.csv`** — Price data for tickers assigned to the training set
- **`test.csv`** — Price data for tickers assigned to the test set

Both files contain the following columns:

| Column | Description |
|--------|-------------|
| Company | Company name |
| Ticker | Stock ticker symbol |
| Datetime | Timestamp of the candle |
| Open | Opening price |
| Close | Closing price |
| Ratio | Return rate calculated as `(Close / Open) - 1` |

## Notes

- If data cannot be fetched for a ticker (empty result or error), that ticker is skipped and an error message is printed to the console.
- This project currently covers only the data collection/preparation stage; it does not include a prediction model.
