---
title: Knife API Reference

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

language_tabs:
  - Examples

includes:

search: true
---

# Introduction

Welcome to the Knife API documentation. 

**Knife** - is most powerfull open RESTful API which provides historical and real-time data about all existing cryptocurrencies markets.
Our API is integrated with about 70 differrent exchanges:

+ Poloniex
+ Localbitcoins 
+ Bitshares
+ etc

**Disclaimer:**
*bla bla bla <br> to read more CLICK [HERE](https://www.google.ru/)*

## Rate Limiting

Knife API is free to use. However, certain endpoints require authentication. Please [Sign Up](https://knife.io/signup) and create a [API Key](https://knife.io/profile/apikeys).

The number of requests to our API are limited on the free plan. Full details of each plan’s access rights can be found [here](https://knife.io/plans)

# Authentication

API Key authentication should be used for endpoints that are restricted to a certain plan. These endpoints are marked throughout our documentation where required.

Authenticating your API requests is recommended for all users, as the rate limiting for registered users are more generous.
Any request that is made using an API Key must be signed, you can create an API Key pair in your [account](https://knife.io/profile/apikeys).

## Requests

All authenticated requests must contain this HTTP header:

+ `Sign:{value}`

`value` is composed from `timestamp`, `public_key` .... // описать процесс аутентификации и получения `Sign`

Example of `Sign` header:

+ `1234.YzQxNGYyMGI1YzJjNDg3YThkOGU1MTgwZWNhYjY4ODI=.a963aa66dbe4871ca17bbd64d5c1043d2f9204f19538b6e0f69c78f04adab9c8`

# Markets

The "Markets" section contains endpoints related to aggreagated market data.
Market - is a traded pair (e.g. "btc-usd" or "eth-btc"). 

Knife API provides aggregated data for specified "market" from 70 exchanges.

## List Markets

> GET https://knife.io/api/markets?filter=btc

```json
200 (application/json)

[
    {
      "market": "btc-usd",
      "exchanges": ["bitfinex","bitstamp", ...]
    },
    {
      "market": "btc-usdt",
      "exchanges": ["poloniex","bittrex", ...]
    },
    {
      "market": "btc-cny",
      "exchanges": ["OKcoin","Huobi", ...]
    },
    ...
]

```

Get a list of all existing markets. We calls "Market" Can be filtered by currency

**HTTP Request**

`GET https://knife.io/api/markets`

**URI parameters**

Parameter | Default | Description
--------- | ------- | -----------
filter | none | Filter by asset/currency. For example "XMR" will return all markets for XMR currency. If not specified - full list will be returned

## Market

> GET https://knife.io/api/markets/btc-usd?exclude=poloniex

```json
200 (application/json)

{
    {
      "market": "eth-btc",
      "exchanges": ["bittrex","bitstamp", ...],
      "price":"0.1",
      "volume": "5234440", //volume in basis currency
      "volume_usd": "52344400011" //volume in USD
    }
}
```

Get a detailed data for specified market.
You can `include` or `exclude` exchanges by specifying desired parameter in URI

**HTTP Request**

`GET https://knife.io/api/markets/{market}`

`market` - it can be any existing pair. For example: eth-btc

**URI parameters**

Parameter | Default | Description
--------- | ------- | -----------
include or exclude | none | Include or exclude specific exchanges for resulting data

<aside class="warning">You can't use two include and exclude parameters at the same time.</aside>

## Ticker

Endpoints for retreiving ticker data for any pair (market)

### Short

> GET https://knife.io/api/ticker/btc-usd

```json
200 (application/json)

{
  "ask": 2418.79,
  "bid": 2418.35,
  "last": 2418.66,
  "averages": {
    "daily": 2418.98,
    "weekly": 2418.39,
    "monthly": 2419.76
  },
  "volume": 256542.49, //in basis currency
  "changes": {
    "price": {
      "weekly": 9.92,
      "monthly": -20.62,
      "daily": 0.93
    },
    "percent": {
      "weekly": 2.43,
      "monthly": -4.69,
      "daily": 0.22
    }
  },
  "volume_percent": 66.42, //from all volumes for basis currency
  "timestamp": 1458754392,
  "display_timestamp": "Wed, 23 Mar 2016 17:33:12 +0000"
}
```
Get actual volume-weighted data for specified `market` from all exchanges.

**HTTP Request**

`GET https://knife.io/api/ticker/{market}`

### Full 

> GET https://knife.io/api/ticker/full/btc-usd

```json
200 (application/json)

[
{ 
  "exchange":"poloniex",
  "ask": 2418.79,
  "bid": 2418.35,
  "last": 2418.66,
  "averages": {
    "daily": 2418.98,
    "weekly": 2418.39,
    "monthly": 2419.76
  },
  "volume": 256542.49, //in basis currency
  "changes": {
    "price": {
      "weekly": 9.92,
      "monthly": -20.62,
      "daily": 0.93
    },
    "percent": {
      "weekly": 2.43,
      "monthly": -4.69,
      "daily": 0.22
    }
  },
  "volume_percent": 66.42, //from all volumes for basis currency
  "timestamp": 1458754392,
  "display_timestamp": "Wed, 23 Mar 2016 17:33:12 +0000"
},
{ 
  "exchange":"bittrex",
  "ask": 2418.79,
  ...
]

```

Get full ticker data for `market` divided into exchanges

**HTTP Request**

`GET https://knife.io/api/ticker/full/{market}`

**URI parameters**

Parameter | Default | Description
--------- | ------- | -----------
include or exclude | none | Include or exclude specific exchanges for resulting data

#Exchanges

Endpoints to communicate with existing exchanges and retreive different kinds of data.

## List Exchanges

> GET https://knife.io/api/exchanges/

```json
200 (application/json)

[
{ 
  "exchange":"poloniex",
  "ex_id":1,
  "taker_fee":0.003,
  "maker_fee":0.001,
  "ex_url":"https://poloniex.com"
},
{ 
  "exchange":"bittrex",
  "ex_id":2,
  "taker_fee":0.003,
  "maker_fee":0.001,
  "ex_url":"https://bittrex.com"
},
 ...
],

```

Get list of all existing exchanges where you can get Order Book, Trades History and Charts Data using the endpoints below.

**HTTP Request**

`GET https://knife.io/api/exchanges/`

## Order Book

> POST https://knife.io/api/exchanges

```json

BODY:
{
  "exchange": "poloniex",
  "market": "btc-usdt",
  "data": "order_book",
  "count":50
}

200 (application/json)
{
      "exchange": "poloniex",
      "market": "btc-usdt"
      "data": "order_book",
      "asks": [
        {
          "price": 696.8600000000,
          "quantity": 0.0171218000,
          "total": 11.9314975480
        },
        {
          "price": 696.8600000000,
          "quantity": 0.1290000000,
          "total": 89.8949400000
        }
      ],
      "bids": [
        {
          "price": 2696.8500000000,
          "quantity": 0.2145000000,
          "total": 249.4743250000
        },
        {
          "price": 2696.8500000000,
          "quantity": 0.1799000000,
          "total": 225.3633150000
        }
      ]
}

```

Get Order Book for specified `market` from specified `exchange`. 

**HTTP Request**

`POST https://knife.io/api/exchanges/`

**POST body parameters**

Parameter | Description
--------- | -----------
market | Market (e.g. "xmr-btc")
exchange | Convention name of exchange
data | The type of data. Can be `order_book` or 'trades'
count | Limits number of resulting order. Set to `0` to get full order book. The `count` is limited to `10` by default


## Trades History

> POST https://knife.io/api/exchanges

```json

BODY:
{
  "exchange": "poloniex",
  "market": "btc-usdt",
  "data": "trades_history",
  "count":50
}

200 (application/json)
{
      "exchange": "poloniex",
      "market": "btc-usdt",
      "data": "history",
      "history": [
        {
          "price": 2675.9900000000,
          "quantity": 0.2851600000,
          "timestamp": 1458754392,
          "type": "BUY"
        },
        {
          "price": 2676.0000000000,
          "quantity": 0.5000000000,
          "timestamp": 1458754392,
          "type": "SELL"
        },
        {
          "price": 2676.0000000000,
          "quantity": 1.0307844000,
          "timestamp": 1458754392,
          "type": "SELL"
        },
      ]
    },

```

Get recent trades history for specified `market` from specified `exchange`. This endpoint can provide only 1000 last trades

**HTTP Request**

`POST https://knife.io/api/exchanges/`

**POST body parameters**

Parameter | Description
--------- | -----------
market | Market (e.g. "xmr-btc")
exchange | Convention name of exchange
data | The type of data. Can be `order_book` or `trades` 
count | Limits number of resulting order. Set to `0` to get full order book. The `count` is limited to `10` by default

# Advanced data

Knife API provides special endpoints to get different advanced data about crypto-economics.

## Macro-Economics data

> GET https://knife.io/api/advanced/macro?convert=usd

```json
200 (application/json)

{
  "total_cap":100000000000,
  "bitcoin_dominance":45.5,
  "total_24h_volume":1349949494,
  "active_exchanges":79,
  "active_markets":345,
  "active_assets":800,
}

```
Returns general macro-economics data

**HTTP Request**

`GET https://knife.io/api/advanced/macro`

**URI parameters**

Parameter | Description
--------- | -----------
convert | fiat symbol for converting `total_cap` and `total_24_volume`

## Meta Data Per Symbol

> GET https://knife.io/api/advanced/meta?symbol=21Mcoin

```json
200 (application/json)

[
    {
        "genesis_id": "",
        "system": "21Million",
        "dependencies": [
            "Ethereum"
        ],
        "icon": "21million",
        "token": {
            "name": "21MCoin token",
            "symbol": "21MCoin"
        },
        "consensus": {
            "consensus_type": "ERC20 Token",
            "consensus_name": "Ethereum"
        },
        "descriptions": {
            "state": "Project",
            "system_type": "cryptoasset",
            "headline": "Open, transparent media finance platform"
        },
        "crowdsales": {
            "start_date": "2017-06-12T00:00:00",
            "end_date": "2017-07-12T00:00:00",
            "end_date_plan": "2017-07-12T00:00:00",
            "genesis_address": [
                ""
            ],
            "funding_terms": "https://www.21million.co.uk/",
            "funding_url": "https://www.21million.co.uk/",
            "tokens_sold": "",
            "tokens_issued": "",
            "btc_raised": "",
            "cap_limit": [
                "usd = 7500000"
            ],
            "raised": [
                ""
            ]
        },
        "links": [
            {
                "type": "website",
                "name": "21million.co.uk",
                "url": "https://www.21million.co.uk/",
                "tags": [
                    "Main"
                ]
            },
            {
                "type": "paper",
                "name": "21Million Whitepaper",
                "url": "https://media.wix.com/ugd/1478f4_1328b8d0432b41a5a24b40f746e2c031.pdf",
                "tags": [
                    "Main",
                    "Science"
                ]
            },
            {
                "type": "twitter",
                "name": "Twitter",
                "url": "https://twitter.com/Real21Million",
                "icon": "twitter.png",
                "tags": [
                    "News"
                ]
            },
            {
                "type": "slack",
                "name": "21Million Slack",
                "url": "https://21millioncoins.slack.com/signup",
                "icon": "slack.png",
                "tags": [
                    "News"
                ]
            },
            {
                "type": "custom",
                "name": "Facebook",
                "url": "https://www.facebook.com/Real21Million/",
                "icon": "facebook.png",
                "tags": [
                    "News"
                ]
            },
            {
                "type": "custom",
                "name": "YouTube",
                "url": "https://www.youtube.com/channel/UCgoT8hDTuUaS-ssjACHI-PA",
                "icon": "youtube.png",
                "tags": [
                    "News"
                ]
            }
        ]
    },
  ...
]
```
Returns a meta data for specified `symbol` or `name` of blockchain system

**HTTP Request**

`GET https://knife.io/api/advanced/meta`

**URI parameters**

Parameter | Description
--------- | -----------
symbol | symbol of blockchain asset

## Social Stats

> GET https://knife.io/api/advanced/social?symbol=BTC

```json
{
  "Response": "Success",
  "Message": "Social data successfully returned",
  "Type": 100,
  "Data": {
    "General": {
      "Name": "BTC",
      "CoinName": "Bitcoin",
      "Type": "Webpagecoinp",
      "Points": 344868
    },
    "Social":[ 
    "Twitter": {
      "followers": 98788,
      "following": "98",
      "lists": 1891,
      "favourites": "71",
      "statuses": 12616,
      "account_creation": "1313643968",
      "name": "Bitcoin",
      "link": "https://twitter.com/bitcoin",
      "Points": 108255
    },
    "Reddit": {
      "subscribers": 176594,
      "active_users": 381,
      "community_creation": "1284042626",
      "posts_per_hour": "5.06",
      "posts_per_day": "121.49",
      "comments_per_hour": "44.82",
      "comments_per_day": 1075.7,
      "link": "https://www.reddit.com/r/bitcoin/",
      "name": "Bitcoin",
      "Points": 179888
    },
    "Facebook": {
      "likes": 25816,
      "is_closed": "false",
      "talking_about": 93,
      "name": "Bitcoin P2P Cryptocurrency",
      "link": "https://www.facebook.com/bitcoins/",
      "Points": 26746
    },
    "CodeRepository": {
      "List": [{
        "stars": 7918,
        "language": "C++",
        "forks": 5439,
        "open_total_issues": "402",
        "subscribers": 1013,
        "size": "100332",
        "url": "https://github.com/bitcoin/bitcoin",
        "last_update": "1449226680",
        "last_push": "1449225545",
        "created_at": "1292771803",
        "fork": "false",
        "source": {
          "Name": "",
          "Url": "",
          "InternalId": -1
        },
        "parent": {
          "Name": "",
          "Url": "",
          "InternalId": -1
        },
        "open_pull_issues": "79",
        "closed_pull_issues": "4820",
        "closed_total_issues": "6753",
        "open_issues": "323",
        "closed_issues": "1933",
        "Points": 21835
      }],
    }
  }
}
```
Get social stats data per specified `symbol`

**HTTP Request**

`GET https://knife.io/api/advanced/meta`

**URI parameters**

Parameter | Description
--------- | -----------
symbol | symbol of blockchain asset

# Blockchain Data

See here: https://chainz.cryptoid.info/api.dws

# Conversion

Endpoint to convert any currency to any currency

## Perform conversion

> GET https://knife.io/api/convert?from=btc&to=eth&amount=1

```json
{
  "success": true,
  "time": "4-06-2017 13:55:32",
  "price": 10.93
}
```

Returns conversion from any (crypto or fiat) currency to any other (crypto or fiat) currency

**HTTP Request**

`GET https://knife.io/api/convert`

**URI parameters**

Parameter | Description
--------- | -----------
from | Source currency code
to | Target currency code
amount | Amount of source currency

# Historical Data

Get a historical market data for specified `market` from specified `exchange`. This data available only for "Business" subscription plan. 

Each request should be authenticated.

## Trades History

> GET https://knife.io/api/historical/trades_history?exchange=poloniex&market=btc-usdt&start=18999400404&end=18999400904

```json

200 (application/json)
{
      "exchange": "poloniex",
      "market": "btc-usdt",
      "data": "trades_history",
      "history": [
        {
          "price": 2675.9900000000,
          "quantity": 0.2851600000,
          "timestamp": 1458754392,
          "type": "BUY"
        },
        {
          "price": 2676.0000000000,
          "quantity": 0.5000000000,
          "timestamp": 1458754392,
          "type": "SELL"
        },
        {
          "price": 2676.0000000000,
          "quantity": 1.0307844000,
          "timestamp": 1458754392,
          "type": "SELL"
        },
        ...
      ]
    },

```

Get historical trades for specified `market` from specified `exchange`. 
Returns up to 50 000 trades between `start` and `end` unix timestamp.

**HTTP Request**

`GET https://knife.io/api/historical/trades_history`

**URI parameters**

Parameter | Description
--------- | -----------
exchange | Convention name of exchange
market | Name of market (e.g. "btc-usdt")
start | Timestamp for start 
end | Timestamp for finish

## Historical Order Book

> GET https://knife.io/api/historical/order_book?market=btc-usdt&exchange=poloniex&start=18999400404&end=18999400904&type=all

```json

200 (application/json)
{
      "exchange": "poloniex",
      "market": "btc-usdt"
      "asks": [
        {
          "timestamp":18999400404,
          "price": 696.8600000000,
          "quantity": 0.0171218000,
          "total": 11.9314975480
        },
        {
          "timestamp":18999400405,
          "price": 696.8600000000,
          "quantity": 0.1290000000,
          "total": 89.8949400000
        }
      ],
      "bids": [
        {
          "timestamp":18999400404,
          "price": 2696.8500000000,
          "quantity": 0.2145000000,
          "total": 249.4743250000
        },
        {
          "timestamp":18999400405,
          "price": 2696.8500000000,
          "quantity": 0.1799000000,
          "total": 225.3633150000
        }
      ]
}

```

Returns historical order book data for specified `market` from specified `exchange`.
`Start` and `end` are given in UNIX timestamp format and used to specify the date range for the data returned.
All data is sorted by timestamp.

**HTTP Request**

`GET https://knife.io/api/historical/order_book`

**URI parameters**

Parameter | Description
--------- | -----------
exchange | Convention name of exchange
market | Name of market (e.g. "btc-usdt")
start | Timestamp for start 
end | Timestamp for finish
type | `ask` or `bid`. By default `all`

## Charts Data

> GET https://knife.io/api/historical/charts?exchange=poloniex&market=btc-usdt&period=300&start=18999400404&end=18999400904

```json

200 (application/json)
[{"date":1405699200,"high":0.0045388,"low":0.00403001,"open":0.00404545,"close":0.00435873,"volume":44.34555992},
{"date":1405713600,"high":0.00435,"low":0.00412,"open":0.00428012,"close":0.00412,"volume":19.12271662},
{"date":1405728000,"high":0.00435161,"low":0.00406,"open":0.00411473,"close":0.00435161,"volume":35.18169499},

```

Returns candlestick chart data. Required GET parameter `period` (candlestick period in seconds; valid values are 300, 900, 1800, 7200, 14400, and 86400). `Start` and `end` are given in UNIX timestamp format and used to specify the date range for the data returned.

**HTTP Request**

`GET https://knife.io/api/historical/charts`

**URI parameters**

Parameter | Description
--------- | -----------
exchange | Convention name of exchange
market | Name of market (e.g. "btc-usdt")
period | Candlestick period in seconds; valid values are 300, 900, 1800, 7200, 14400, and 86400.
start | Timestamp for start 
end | Timestamp for finish
