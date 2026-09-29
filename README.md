# ✈️ Flight Deal Finder

🇺🇸 **English** | 🇧🇷 [Português](README.pt-BR.md)

Python bot that searches, compares and monitors airfare, storing price history in a database and sending alerts through Telegram.

**Current version:** V1
**Data source:** [Travelpayouts / Aviasales Data API](https://travelpayouts.github.io/slate/)

## What it does

- **Flexible date search:** queries the API month by month and merges every result within a date range.
- **Alternative airports:** compares several destinations at once (e.g. Milan via MXP, LIN and BGY).
- **Cost-benefit ranking:** scores each flight by combining criteria with adjustable weights:

  | Criterion | Default weight |
  |---|---|
  | Price | 50% |
  | Duration | 25% |
  | Stops | 15% |
  | Departure time (8am–8pm is ideal) | 10% |

  The weights can be changed per profile, e.g. a "comfort" profile that prioritizes duration and fewer stops.
- **Price history in SQLite:** every search is saved to a local database (`voos` table), building a historical base per route. Price alerts are stored in their own table (`alertas`).
- **Price classification:** compares the current price with the route's historical average:
  - 🟢 **Excellent deal:** 15% or more below average
  - 🟡 **Good price:** 5% to 15% below average
  - ⚪ **Normal price:** between 5% below and 10% above average
  - 🔴 **High price:** more than 10% above average
- **Deal detection:** only triggers once a route has at least 3 records, so it never relies on averages without enough data.

## Telegram bot (RECON-1)

| Command | What it does |
|---|---|
| `/start` | Introduces the bot and shows how to use it |
| `/missao ORIGIN, DESTINATION, DD/MM/YYYY, DD/MM/YYYY` | Searches flights in the period, sends a "radar" GIF of the scan and returns the top 3 ranked flights with links |
| `/alerta ORIGIN, DESTINATION, max=VALUE` | Creates a maximum-price alert, checked automatically every 30 minutes |

Origin and destination accept a city name or an IATA code (e.g. `São Paulo` or `GRU`). The bot's messages are in Portuguese.

Example:
```
/missao São Paulo, Milão, 15/09/2026, 30/09/2026
```

## Technologies

- **Python**
- **Requests** to consume the REST API
- **Pandas** for data processing and analysis
- **SQLite** for price history (SQL queries)
- **Matplotlib** for the radar animation
- **python-telegram-bot** for the bot and scheduled alerts
- **Google Colab** as the runtime environment

## How to run

1. Open the notebook in Google Colab.
2. Get two tokens:
   - **Travelpayouts:** free sign-up at travelpayouts.com
   - **Telegram:** create a bot with [@BotFather](https://t.me/BotFather)
3. In Colab, add the tokens under **Secrets** (key icon in the sidebar) with these names:
   - `TRAVELPAYOUTS_TOKEN`
   - `TELEGRAM_TOKEN`
4. Run the cells in order. The `flight_deal_finder.db` database is created automatically in the `FlightDealFinder` folder of your Google Drive.

> Tokens never live in the code: they are read from Colab Secrets (`userdata.get`).

## Next steps

- Run the bot outside Colab, on a server, so it stays online
- Price evolution charts per route over time
- Combined round trips and multiple passengers
- Price trend forecasting based on the stored history

## Disclaimer

Personal project for study and portfolio purposes. Prices come from the Travelpayouts/Aviasales API and may change before purchase.
