# Kalshi 15-Minute Commodity & FX Analyzer

Mobile-first, read-only Streamlit dashboard for Android Chrome. It monitors six underlying markets, discovers open Kalshi markets, estimates short-horizon UP/DOWN probabilities, stores predictions locally, and supports paper-trade tracking and a basic proxy backtest.

## Included markets
- Gold: `GC=F`
- Silver: `SI=F`
- Crude Oil / WTI: `CL=F`
- EUR/USD: `EURUSD=X`
- GBP/USD: `GBPUSD=X`
- USD/JPY: `JPY=X` (Yahoo convention: yen per US dollar)

## Important limitations
1. **No live Kalshi settlement feed is presumed.** Yahoo Finance quotes are indicative and may be delayed. The app requires you to manually enter the contract's opening and current settlement-reference values and confirm that you independently verified they match the official contract rules.
2. Kalshi contract discovery scans currently open public markets and filters text for asset names and 15-minute terms. It may miss contracts or surface a nonmatching market. Verify every ticker, title, close time, and settlement rule.
3. The model is an educational baseline, not a proven profitable predictor. It is not calibrated until you build a sufficiently large, clean, out-of-sample dataset.
4. The backtest is an underlying-price proxy test, not historical Kalshi execution/P&L. It does not reconstruct old order books, fees, spreads, or exact settlement references.
5. The fee value shown in the workbench is a conservative placeholder. Verify the current fee schedule and exact series/order fee before evaluating a trade.
6. This application does not place orders, access account balances, or request Kalshi credentials.

## Run locally on Windows, Mac, or Linux
Install Python 3.10 or newer.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# Mac/Linux:
source .venv/bin/activate

pip install -r requirements.txt
streamlit run app.py
```

Streamlit prints a local URL, usually `http://localhost:8501`.

## Open it on Android
### Easiest: run on a computer on the same Wi-Fi
1. Start Streamlit with:
   ```bash
   streamlit run app.py --server.address 0.0.0.0
   ```
2. Find the computer's local IPv4 address (for example `192.168.1.25`).
3. On Android Chrome, open `http://192.168.1.25:8501`.
4. Keep the computer running and both devices on the same Wi-Fi.

### Cloud hosting
You can deploy the folder to a Python hosting service that supports Streamlit. Upload `app.py` and `requirements.txt`, set the app entry point to `app.py`, and keep the app private if you do not want your locally stored trading notes exposed. Do not add Kalshi private API keys; this app does not need them.

## Refresh and data
- The dashboard refreshes every 15/30/60 seconds.
- Kalshi public market discovery is cached for 30–45 seconds.
- Yahoo one-minute history is cached for 45 seconds. This is not a promise of real-time delivery.
- Local prediction and paper-trade history is stored in `kalshi_analyzer.db` in the working directory.

## Risk settings
Default bankroll is $75, max risk per paper trade is $1, and daily loss stop is $4. Adjust the bankroll input to your actual available amount. Maximum loss for a YES/NO purchase is calculated as entry price times contracts plus estimated fees. The app blocks paper trades above the configured per-trade risk.

## Files
- `app.py` — dashboard and model
- `requirements.txt` — Python dependencies
- `README.md` — setup and safety notes
