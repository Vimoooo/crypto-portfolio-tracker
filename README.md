import requests

API_URL = "https://api.coingecko.com/api/v3/simple/price"

portfolio = {
    "bitcoin": 0.05,
    "ethereum": 0.8,
    "solana": 5,
}

def get_prices(coins):
    params = {
        "ids": ",".join(coins),
        "vs_currencies": "usd"
    }

    response = requests.get(API_URL, params=params, timeout=10)
    response.raise_for_status()

    return response.json()


def calculate_portfolio(prices):
    total = 0

    print("\nCrypto Portfolio")
    print("-" * 35)

    for coin, amount in portfolio.items():
        price = prices.get(coin, {}).get("usd", 0)
        value = price * amount
        total += value

        print(f"{coin.title():<12} ${value:,.2f}")

    print("-" * 35)
    print(f"Total Value: ${total:,.2f}")


def main():
    try:
        prices = get_prices(portfolio.keys())
        calculate_portfolio(prices)

    except requests.RequestException as error:
        print(f"API Error: {error}")

    except Exception as error:
        print(f"Unexpected Error: {error}")


if name == "main":
    main()
