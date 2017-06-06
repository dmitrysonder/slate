---
title: Knife API Reference

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Knife API documentation. Knife is open RESTful API which provides historical and real-time data about cryptocurrencies markets.

## Authentification

The proccess of authentification

```GET```

# Markets

There are a set of methods related to markets.
Market - is a traded pair (e.g. "BTCUSD"). Knife API provides aggregated data for specified "market" from all exchanges

## Get all markets

Returns a list of all existing markets. 

## Get Market

Returns a object with detailed data for specified market

## Get ticker

If no query parameters are sent, then returns ticker data for every supported symbol. If crypto(s) and/or fiat(s) are sent as parameters, then only the ticker for those values is sent

### Per Symbol

Returns ticker data for specified symbol

### Short

Returns basic ticker denoting last and daily average price for the specified crypto/fiat values.

### Ticker Changes Per Symbol

Returns ticker values and price changes for specified market and symbol.

### Custom

This endpoint can be used to generate a custom index in a certain currency. The “inex” path parameter represents “include” or “exclude”, you can choose to generate an index removing specified exchanges, or only including the few that you require.

Note that the exchange must have an order book in the specified currency.

#Exchanges

Endpoints to communicate with existing exchanges and retreive different kinds of data

## Get Exchanges

Returns list of all existing exchanges. Parameters: type, markets

Response:
name (markets, pairs)
health
ping

## Exchange Data

### Get data for Exchange

Returns data about specified exchange:
name
url
traded pairs (symbol, volumes)
markets
timestamp

### Get Order Book by pair

Return order book for specified exchange

### Get Trades by pair

Return trades for specified exchange. 

### Get Chart Data by pair

Return OHLCV charts for pair from specified exchange

### Get ticker

Returns ticker: ask, bid, last, volume

# Meta data

## Get Meta Data

Returns a meta data of all available coins

# Conversion

Endpoint to convert any currency to any currency

## Perform crypto-fiat conversion

Returns conversion from start currency to resulting currency. Only conversion from fiat to crypto, or vice verca

## Perfrom crypto-crypto conversion

Return conversion from start cryptocurrency to resulting cryptocurrency.

# General market data

## Get general Data

Returns general data about crypto-economics state


