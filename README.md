# Binance Volitility Trading Bot

## Description
This Binance trading bot analyses the changes in price across all coins on Binance and place trades on the most volatile ones. 
In addition to that, this Binance trading algorithm will also keep track of all the coins bought and sell them according to your specified Stop Loss and Take Profit.



The bot will listen to changes in price accross all coins on Binance. By default, we're only picking USDT pairs. We're excluding Margin (like BTCDOWNUSDT) and Fiat pairs

> Information below is an example and is all configurable

- The bot checks if the any coin has gone up by more than 3% in the last 5 minutes
- The bot will buy 100 USDT of the most volatile coins on Binance
- The bot will sell at 6% profit or 3% stop loss


You can follow the [Binance volatility bot guide](https://www.cryptomaton.org/2021/05/08/how-to-code-a-binance-trading-bot-that-detects-the-most-volatile-coins-on-binance/) for a step-by-step walkthrough of the bot development.

## READ BEFORE USE
1. If you use the `TEST_MODE: False` in your config, you will be using REAL money.
2. To ensure you do not do this, ALWAYS check the `TEST_MODE` configuration item in the config.yml file..
3. This is a framework for users to modify and adapt to their overall strategy and needs, and in no way a turn-key solution.
4. Depending on the current market, the default config might not do much, so you will have to adapt it to your own strategy.

## Usage
Please checkout our wiki pages:

- [Setup Guide](https://github.com/CyberPunkMetalHead/Binance-volatility-trading-bot/wiki/Setup-Guide)
- [Bot Strategy Guide](https://github.com/CyberPunkMetalHead/Binance-volatility-trading-bot/wiki/Bot-Strategy-Guide)
- [Configuration Guide](https://github.com/CyberPunkMetalHead/Binance-volatility-trading-bot/wiki/Configuration)

## Troubleshooting

1. Read the [FAQ](FAQ.md)
2. Open an issue / check us out on `#troubleshooting` at [Discord](https://discord.gg/buD27Dmvu3) 🚀 
    - Do not spam, do not berate, we are all humans like you, this is an open source project, not a full time job. 

## 💥 Disclaimer

All investment strategies and investments involve risk of loss. 
**Nothing contained in this program, scripts, code or repository should be construed as investment advice.**
Any reference to an investment's past or potential performance is not, 
and should not be construed as, a recommendation or as a guarantee of 
any specific outcome or profit.
By using this program you accept all liabilities, and that no claims can be made against the developers or others connected with the program.

# Docker
The Docker addition has some modifications in order to safely work.
For starters, it uses ENV variables instead of a creds.config file. You can add the ENV variables to the global .env file from your host machine and reference them in your docker-compose for e.g. ENV ACCESS_KEY would be: `${ACCESS_KEY}`.
There are 3 ENV variables:
- ENV='production' (script will know to use the API keys, otherwise use the debug environment ones)
- ACCESS_KEY (your API access key)
- ACCESS_SECRET (your API secret)

You have to put your the following files in a separate directory (for Docker-compose to mount):
- config.yml
- coins_bought.json (so you'll retain your portfolio if docker shutdown)
- tickers.txt
- trades.txt


Pull `latest` Docker container:
`docker pull hidde43/binance-trading-bot:latest`

Run Docker container locally:
`docker run -it --platform linux/amd64 --env-file .env hidde43/binance-trading-bot:latest`

Example Docker-compose:
```yaml
# Binance Volatility Trading Bot
  binance_bot:
    container_name: binance_bot
    restart: always
    image: hidde43/binance-trading-bot:latest
    volumes:
      - <path to your data folder>:/config # config is what Docker expects as mount
    environment:
      ENV: production
      ACCESS_KEY: ${BINANCE_ACCESS_KEY} # parsed from global ENV
      SECRET_KEY: ${BINANCE_SECRET_KEY}
```

Build new version:
`docker buildx build --platform linux/amd64  -f build/Dockerfile . -t hidde43/binance-trading-bot:latest -t hidde43/binance-trading-bot:v0.1`