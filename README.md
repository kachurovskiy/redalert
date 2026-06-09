# Opportunity Radar

http://kachurovskiy.com/redalert/ - single-file, browser-only monitor for contrarian opportunity signals based on aggregate Fear & Greed history.

## What It Does

- Fetches long historical Fear & Greed coverage from `whit3rabbit/fear-greed-data`.
- Fetches the selected market ticker's daily close history from Yahoo Finance's chart endpoint through a browser-readable text proxy.
- Fetches VIX close history from the `datasets/finance-vix` daily CSV on GitHub.
- Merges in the live CNN Fear & Greed graph data on page load.
- Uses only the aggregate `fear_and_greed.score` and `fear_and_greed_historical.data` fields from CNN.
- Lets the user edit Fear & Greed and VIX criteria from the Settings tab.
- Defaults to an optimizer-informed criteria set: score <= 5 for 3 saved rows, score <= 20 for 5 saved rows, or VIX >= 35 for 1 saved row.
- Shows the full-width series chart on the main tab with every year labeled, vertical new-year boundary lines, transparent green signal-period bands, and signal-start markers on the selected ticker line.
- Starts the Overview chart at 10 years by default on wider screens and 1 year by default on narrow screens, with controls for all data or the last 10, 5, 3, or 1 years.
- Keeps the current signal summary on a separate tab whose label reports whether a signal is active.
- Shows 1, 3, 6, and 12 month historical forward-return averages on the Signal tab, using prior signal starts under the current criteria. These are backward-looking samples, not forecasts.
- Groups signal criteria, market ticker selection, return checkpoints, optimizer cadence, and full local-storage reset controls in the Settings tab.
- Provides an optimizer tab that ranks candidate criteria primarily by expanding walk-forward out-of-sample validation, while still showing full-history context, loss rate, loss size, and a configurable signal cadence target. Rows include train average return, validation average return, validation win rate, worst validation return, validation signal count, and a stability score; applying a rule with strong in-sample results but weak validation evidence requires confirmation. It also estimates the best percent of remaining cash to deploy on each active signal day, resetting capital for each contiguous signal period rather than holding all buys to the latest date. Optimizer work runs in a Web Worker pool when multiple CPU cores are available, with progress reporting so the page remains responsive.
- Shows selected-ticker forward returns after configurable month checkpoints, defaulting to 3, 6, and 12 months, with per-horizon stats for win rate, median return, median gain/loss, and best/worst outcomes.
- Shows data-source status in the Sources tab and highlights when automatic refresh action is needed.
- Keeps the detailed row table on a secondary tab.
- Stores normalized history and settings only in the browser's `localStorage`, with a Settings reset button that clears local storage and refreshes the page.
- Runs as static HTML with no build step, backend, analytics, API keys, or bundled market data.

## Run Locally

Open http://kachurovskiy.com/redalert/ in a browser.

If browser CORS policy blocks the direct CNN request, the page retries through the configured AllOrigins proxy. For a public deployment, replace that proxy with infrastructure you control if you do not want visitors' browsers to contact a third-party proxy.

## Publishing Notes

- This project is unaffiliated with CNN.
- CNN and Fear & Greed Index names belong to their respective owners.
- This repository does not include or redistribute CNN data; the browser fetches data at runtime.
- The long-history source is the canonical `fear-greed.csv` documented by `whit3rabbit/fear-greed-data`: https://github.com/whit3rabbit/fear-greed-data
- The selected ticker overlay and forward-return calculations use Yahoo Finance chart data, read through `r.jina.ai` because Yahoo does not expose browser-readable CORS headers for the chart JSON. SPY is the default, with QQQ, TQQQ, and custom Yahoo symbols available from Settings.
- VIX values use the `datasets/finance-vix` daily CSV: https://raw.githubusercontent.com/datasets/finance-vix/main/data/vix-daily.csv
- Forward returns use the selected ticker price at the signal and the first available selected-ticker trading day on or after each target date.
- Review CNN's current Terms of Use before relying on this in a public or production context: https://www.cnn.com/terms
- This is not financial advice and should not be used as the sole basis for trading or investment decisions.

## Privacy And Security

The app has no user accounts, no server, and no telemetry. It saves fetched rows, selected ticker prices, criteria settings, return checkpoints, and optimizer cadence settings in local browser storage and restricts network access with a Content Security Policy to the CNN data host, Yahoo Finance, `r.jina.ai`, the historical CSV hosts, and the configured CORS proxy.

## License

MIT. See [LICENSE](LICENSE).
