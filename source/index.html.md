---
title: BitDa Futures API documentation

language_tabs: # must be one of https://git.io/vQNgJ

- shell

search: False
---

# Introduction

## API Introduction

Welcome to use BitDa API！ You can use this API get market information, trading and managing your account.

The code will display on the right side of this document. Currently we only provide "shell" code examples.

It can be accessed using the following domain names: api.bitda.com

Welcome all professional maker strategies and organizations for long term market making program.

## Public API

Verification free for below portals:

| API                                                         | Introduction            | Trading area |
| ----------------------------------------------------------- | ----------------------- | ------------ |
| [GET /open/api/v2/market/kline](#get-market-k-line)         | Get Market K-line       | Futures      |
| [GET  /open/api/v2/market/list](#get-market-list)           | Get Market List         | Futures      |
| [GET /open/api/v2/market/deals](#get-market-transactions)   | Get Market Transactions | Futures      |
| [GET /open/api/v2/market/depth](#get-market-depth)          | Get Market Depth        | Futures      |
| [GET /open/api/v2/market/state](#get-market-status)         | Get Market Status       | Futures      |
| [GET /open/api/v2/market/state/all](#get-all-market-status) | Get all market status   | Futures      |

## Private API

Eligible to access portals:

| API                                                                                              | Introduction                                            | Trading area |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------- | ------------ |
| [POST /open/api/v2/position/margin](#adjust-position-margin)                                     | Adjust position margin                                  | Futures      |
| [GET /open/api/v2/order/deals](#get-user-transaction)                                            | Get User Transaction                                    | Futures      |
| [GET /open/api/v2/order/finished](#get-completed-orders)                                         | Get Completed Orders                                    | Futures      |
| [POST /open/api/v2/order/market](#market-order)                                                  | Market Order                                            | Futures      |
| [POST /open/api/v2/order/cancel/all](#cancel-all-orders-in-a-single-market)                      | Cancel all orders in a single market                    | Futures      |
| [GET /open/api/v2/order/detail](#get-order-details)                                              | Get Order Details                                       | Futures      |
| [POST /open/api/v2/order/cancel](#cancel-order)                                                  | Cancel Order                                            | Futures      |
| [POST /open/api/v2/order/limit](#submit-limit-order)                                             | Submit Limit Order                                      | Futures      |
| [GET /open/api/v2/order/pending](#get-the-entrusted-order)                                       | Get the entrusted order                                 | Futures      |
| [POST /open/api/v2/order/stop](#submit-stop-order)                                               | Submit stop order                                       | Futures      |
| [POST /open/api/v2/order/stop/cancel](#cancel-stop-order)                                        | Cancel stop order                                       | Futures      |
| [POST /open/api/v2/order/stop/cancel/all](#cancel-all-stop-orders-for-a-single-market)           | Cancel all stop orders for a single market              | Futures      |
| [GET /open/api/v2/order/stop/pending](#get-the-stop-order-in-the-commission)                     | Get the stop order in the commission                    | Futures      |
| [GET /open/api/v2/order/stop/finished](#get-completed-stop-order)                                | Get completed stop order                                | Futures      |
| [POST /open/api/v2/setting/leverage](#adjust-market-opening-leverage-position-mode)              | Adjust market opening leverage/position mode            | Futures      |
| [GET /open/api/v2/setting/leverage](#query-the-market-opening-leverage-position-mode)            | Query the market opening leverage/position mode         | Futures      |
| [GET /open/api/v2/asset/query](#query-assets)                                                    | Query Assets                                            | Futures      |
| [GET /open/api/v2/asset/history](#query-asset-bill)                                              | Query Asset Bill                                        | Futures      |
| [GET /open/api/v2/position/pending](#user-positions)                                             | User Positions                                          | Futures      |
| [GET /open/api/v2/position/margin](#get-adjustable-margin)                                       | Get adjustable margin                                   | Futures      |
| [POST /open/api/v2/position/close/limit](#limit-close)                                           | Limit Close                                             | Futures      |
| [POST /open/api/v2/position/close/market](#market-close)                                         | Market Close                                            | Futures      |
| [POST /open/api/v2/position/close/stop](#position-take-profit-and-stop-loss-settingmodification) | Position Take Profit And Stop Loss Setting/Modification | Futures      |

# Connection Guide

## Restful Host:

    https://ws.bitda.com

## Websocket Host:

    futures
    wss://ws.bitda.com/wsf

## Verification Notice

1. All portals need to conduct verification. Parameters are "client_id","ts","Nonce","sign". "client_id" is the api
   key. "client_key" is the secret key. Please be careful.
2. Ts is the current time stamp. Query with more than 5 seconds time difference will be rejected. "Nonce" is a random
   code that should Not be the same with last time query.
3. Signature method: link "client_id","ts","Nonce" in correct order, use hmac-sha256 to sign. e.g. the unsigned code:
   client_id=abc&Nonce=xyz&ts=1571293029
4. Signature: sign = hmac.New(client_key, sign_str, sha256)
5. Content-Type: application/x-www-form-urlencoded
6. Please put the parameters in the request body for the post interface request, and carry the get interface request in
   the url link

# WebSocket Guide

1. Need verification before subscription
2. Verification method: {"method":"sign",params:{"sign":"","client_id":"","nonce":"","ts": int type},"id": int type}, e.g:  {"method":"sign",params:{"sign":"abc123","client_id":"abc123","nonce":"abc123","ts": 1702342344}, "id": 3532}
3. Client need to timely upload arbitrary code to check. Server will check status every 30 seconds. Links will be closed
   if No information received. {"op":"sub", "topic":"hb"}

## Websocket Public API

## Market KLine
### Request parameters

> Sub

```json
{
  "method": "subscribe.kline",
  "params": {
    "market": "BTCUSDT",
    "period": "15min"
  },
  "id": 6288
}
```

> UbSub

```json
{
  "method":"subscribe.kline",
  "params":{
    "market":"BTCUSDT",
    "period":"15min"
  },
  "id":6288
}
```

| Parameter | Description |
| :-------: | :---------: |
|  market   |   BTCUSDT   |
|  period   |    15min    |

> Responds:

```json
{
  "id":0,
  "method":"update.kline",
  "result":{
    "data":[
      [
        1699943820,
        "36341.59",
        "36341.59",
        "36341.59",
        "36341.59",
        "4.7680",
        "173276.70112000000000",
        "BTCUSDT"
      ]
    ],
    "period":"1min"
  },
  "error":null
}
```
### Data refresh string list
| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |   array   |             |
|   data.0   |   array   |             |
|  data.0.0  |  integer  |  Timestamp  |
|  data.0.1  |  string   | Open price  |
|  data.0.2  |  string   |    High     |
|  data.0.3  |  string   |     Low     |
|  data.0.4  |  string   |    Close    |
|  data.0.5  |  string   |  Quantity   |
|  data.0.6  |  string   |   Amount    |
|  data.0.7  |  string   | Market name |


## Market Transaction
### Request parameters

> Sub

```json
{
  "method":"subscribe.trade",
  "id":1564,
  "params":{
    "market":"BTCUSDT"
  }
}
```

> UbSub

```json
{
  "method":"unsubscribe.trade",
  "id":1564,
  "params":{
    "market":"BTCUSDT"
  }
}
```

| Parameter | Description |
| :-------: | :---------: |
|  market   |   BTCUSDT   |

> Responds:

```json
{
  "id":0,
  "method":"update.trade",
  "result":{
    "amount":"0.0050",
    "id":31931927,
    "market":"BTCUSDT",
    "price":"36341.59",
    "time":1699944005.19225,
    "type":"buy"
  },
  "error":null
}
```
### Data refresh string list
| Field name | Data Type |      Description      |
| :--------: | :-------: | :-------------------: |
|   amount   |  string   |       Quantity        |
|     id     |  integer  |       Trade id        |
|   market   |  string   |      Market name      |
|   price    |  string   |      Match price      |
|    time    |   float   |      Match time       |
|    type    |  string   | Match type: buy, sell |


## Market Depth
### Request parameters

> Sub

```json
{
  "method":"subscribe.depth",
  "id":1477,
  "params":{
    "market":"BTCUSDT",
    "merge":"0"
  }
}
```

> UbSub

```json
{
  "method":"unsubscribe.depth",
  "id":1477,
  "params":{
    "market":"BTCUSDT",
    "merge":"0"
  }
}
```

| Parameter | Description |
| :-------: | :---------: |
|  market   |   BTCUSDT   |
|   merge   |      0      |

> Responds:

```json
{
  "id":0,
  "method":"update.depth",
  "result":{
    "asks":[
      [
        "36341.6",
        "0.0444"
      ]
    ],
    "bids":[
      [
        "36341.25",
        "0.0511"
      ]
    ],
    "index_price":"36612.36",
    "last":"36341.59",
    "market":"BTCUSDT",
    "sign_price":"36589.76",
    "time":1699944061967
  },
  "error":null
}
```
### Data refresh string list
| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    asks    |   array   |             |
|   asks.0   |   array   |             |
|  asks.0.0  |  string   |             |
|  asks.0.1  |  string   |             |
|    bids    |   array   |             |
|   bids.0   |   array   |             |
|  bids.0.0  |  string   |             |
|  bids.0.1  |  string   |             |


## Market Quotes
### Request parameters

> Sub

```json
{
  "method":"subscribe.state",
  "id":1953,
  "params":{}
}

```

> UbSub

```json
{
  "method":"unsubscribe.state",
  "id":1953,
  "params":{}
}
```

| Parameter | Description |
| :-------: | :---------: |

> Responds:

```json
{
  "id":0,
  "method":"update.state",
  "result": {
    "1000SHIBUSDT": {
      "market": "1000SHIBUSDT", 
      "amount": "35226256.573504", 
      "high":"0.009001", 
      "last": "0.008607", 
      "low": "0.008324",
      "open": "0.008864", 
      "period": 86400, 
      "volume":"4036517772", 
      "change": "-0.0289936823104693", 
      "funding_time": 79,
      "position_amount": "0", 
      "funding_rate_last": "0.00092889", 
      "funding_rate_next":"0.00078062", 
      "funding_rate_predict": "0.00059084", 
      "insurance": "12920.37897885999447286856",
      "sign_price": "0.008607", 
      "index_price": "0.008606", 
      "sell_total":"46470921", 
      "buy_total": "43420303"
    }
  }, 
  "error":null
}
```
### Data refresh string list
|            Field name            | Data Type | Description |
| :------------------------------: | :-------: | :---------: |
|           market_name            |  object   |             |
|        market_name.market        |  string   |             |
|        market_name.amount        |  string   |             |
|         market_name.high         |  string   |             |
|         market_name.last         |  string   |             |
|         market_name.low          |  string   |             |
|         market_name.open         |  string   |             |
|        market_name.period        |  integer  |             |
|        market_name.volume        |  string   |             |
|        market_name.change        |  string   |             |
|     market_name.funding_time     |  integer  |             |
|   market_name.position_amount    |  string   |             |
|  market_name.funding_rate_last   |  string   |             |
|  market_name.funding_rate_next   |  string   |             |
| market_name.funding_rate_predict |  string   |             |
|      market_name.insurance       |  string   |             |
|      market_name.sign_price      |  string   |             |
|     market_name.index_price      |  string   |             |
|      market_name.sell_total      |  string   |             |
|      market_name.buy_total       |  string   |             |


## Heartbeat
### Request parameters

> Sub

```json
{"method":"ping"}
```

| Parameter | Description |
| :-------: | :---------: |

> Responds:

```json
{
  "id":0,
  "method":"ping",
  "result":"success",
  "error":null
}
```
### Data refresh string list
| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |


## Websocket Private API

## Signature
### Request parameters

> Sub

```json
{
  "method":"subscribe.sign",
  "id":8538,
  "params": {
    "client_id": "",
    "nonce": "",
    "ts": 123,
    "sign": ""
  },
}
```

| Parameter |  Description   |
| :-------: | :------------: |
| client_id | your client_id |
|   nonce   |     nonce      |
|    ts     |   timestamp    |
|   sign    |   Signature    |

> Responds:

```json
{
  "id":7681,
  "method":"sign",
  "result":"success",
  "error":null
}
```
### Data refresh string list
| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |



## User Positions

### Request parameters

> Sub

```json
{
  "method":"subscribe.position",
  "id":8538,
  "params":{}
}
```

> UbSub

```json
{
  "method":"unsubscribe.position",
  "id":8538,
  "params":{}
}
```

| Parameter | Description |
| :-------: | :---------: |

> Responds:

```json
{
  "id":0,
  "method":"update.position",
  "result":{
    "event":1,
    "position":{
      "position_id":4784242,
      "create_time":1699944061.968543,
      "update_time":1699944061.968656,
      "user_id":9108,
      "market":"BTCUSDT",
      "type":2,
      "side":2,
      "amount":"0.0444",
      "close_left":"0.0444",
      "open_price":"36341.6",
      "open_margin":"6.4063",
      "margin_amount":"16.1356",
      "leverage":"100",
      "profit_unreal":"11.0184",
      "liq_price":"0",
      "mainten_margin":"0.005",
      "mainten_margin_amount":"8.0678",
      "adl_sort":1,
      "roe":"0.6828",
      "margin_ratio":"",
      "stop_loss_price":"-",
      "take_profit_price":"-"
    }
  },
  "error":null
}
```
### Data refresh string list
|           Field name           | Data Type | Description |
| :----------------------------: | :-------: | :---------: |
|             event              |  integer  |             |
|            position            |  object   |             |
|      position.position_id      |  integer  |             |
|      position.create_time      |   float   |             |
|      position.update_time      |   float   |             |
|        position.user_id        |  integer  |             |
|        position.market         |  string   |             |
|         position.type          |  integer  |             |
|         position.side          |  integer  |             |
|        position.amount         |  string   |             |
|      position.close_left       |  string   |             |
|      position.open_price       |  string   |             |
|      position.open_margin      |  string   |             |
|     position.margin_amount     |  string   |             |
|       position.leverage        |  string   |             |
|     position.profit_unreal     |  string   |             |
|       position.liq_price       |  string   |             |
|    position.mainten_margin     |  string   |             |
| position.mainten_margin_amount |  string   |             |
|       position.adl_sort        |  integer  |             |
|          position.roe          |  string   |             |
|     position.margin_ratio      |  string   |             |
|    position.stop_loss_price    |  string   |             |
|   position.take_profit_price   |  string   |             |


## User Order
### Request parameters

> Sub

```json
{
  "method":"subscribe.order",
  "id":8538,
  "params":{}
}
```

> UbSub

```json
{
  "method":"unsubscribe.order",
  "id":8538,
  "params":{}
}
```

| Parameter | Description |
| :-------: | :---------: |

> Responds:

```json
{
  "id":0,
  "method":"update.order",
  "result":{
    "order_id":1566083571,
    "position_id":0,
    "market":"BTCUSDT",
    "type":2,
    "side":2,
    "left":"0.0000",
    "amount":"0.0444",
    "filled":"0.0444",
    "deal_fee":"0.1613",
    "price":"0",
    "avg_price":"36341.6",
    "deal_stock":"1613.567",
    "position_type":2,
    "leverage":"100",
    "update_time":1699944061.968537,
    "create_time":1699944061.968535,
    "status":3,
    "stop_loss_price":"-",
    "take_profit_price":"-",
    "client_oid":"36341ddd362363263626"
  },
  "error":null
}
```
### Data refresh string list
|    Field name     | Data Type | Description |
| :---------------: | :-------: | :---------: |
|     order_id      |  integer  |             |
|    position_id    |  integer  |             |
|      market       |  string   |             |
|       type        |  integer  |             |
|       side        |  integer  |             |
|       left        |  string   |             |
|      amount       |  string   |             |
|      filled       |  string   |             |
|     deal_fee      |  string   |             |
|       price       |  string   |             |
|     avg_price     |  string   |             |
|    deal_stock     |  string   |             |
|   position_type   |  integer  |             |
|     leverage      |  string   |             |
|    update_time    |   float   |             |
|    create_time    |   float   |             |
|      status       |  integer  |             |
|  stop_loss_price  |  string   |             |
| take_profit_price |  string   |             |


## User Assets

### Request parameters

> Sub

```json
{
  "method":"subscribe.asset",
  "id":8538,
  "params":{}
}
```

> UbSub

```json
{
  "method":"unsubscribe.asset",
  "id":8538,
  "params":{}
}
```

| Parameter | Description |
| :-------: | :---------: |

> Responds:

```json
{
  "id":0,
  "method":"update.asset",
  "result":{
    "USDT":{
      "available":"10320.9887",
      "frozen":"0",
      "margin":"16.1356",
      "balance_total":"10320.9887",
      "profit_unreal":"11.0315",
      "transfer":"10097.1501",
      "bonus":"223.8386"
    }
  },
  "error":null
}
```
### Data refresh string list
|        Field name        | Data Type | Description |
| :----------------------: | :-------: | :---------: |
|        marke_name        |  object   |             |
|   marke_name.available   |  string   |             |
|    marke_name.frozen     |  string   |             |
|    marke_name.margin     |  string   |             |
| marke_name.balance_total |  string   |             |
| marke_name.profit_unreal |  string   |             |
|   marke_name.transfer    |  string   |             |
|     marke_name.bonus     |  string   |             |


# Open v2 Public API

## Get Market K-line

```shell
curl "https://api.bitda.com/open/api/v2/market/kline"
```

### HTTP query

- GET `/open/api/v2/market/kline`

<aside class="Notice">Rate limit 1r/s</aside>

### Request parameters

| Parameter Name | Required | Example    | Remark                                                                  |
| -------------- | -------- | ---------- | ----------------------------------------------------------------------- |
| market         | Yes      | ETHUSDT    | Market name                                                             |
| type           | Yes      | 1min       | K-line period,1min,5min,15min,30min,1hour,4hour,6hour,12hour,1day,1week |
| start_time     | No       | 1697617535 | Start time                                                              |
| end_time       | No       | 1697627535 | End time                                                                |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    [
      {
        "open": "1571.23",
        "high": "1573.89",
        "low": "1571.23",
        "close": "1573.89",
        "volume": "3.02",
        "time": 1697620709569
      }
    ]
  ]
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    time    |  integer  |             |
|    open    |  string   |             |
|    high    |  string   |             |
|    low     |  string   |             |
|   close    |  string   |             |
|   volume   |  string   |             |

## Get Market List

```shell
curl "https://api.bitda.com/open/api/v2/market/list"
```

### HTTP query

- GET `/open/api/v2/market/list`

### Request parameters

None

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "type": 1,
      "leverages": [
        "3",
        "5",
        "8",
        "10",
        "15",
        "20",
        "30",
        "50",
        "100"
      ],
      "name": "BTCUSTT",
      "stock": "BTC",
      "money": "USTT",
      "fee_prec": 5,
      "tick_size": "0.01",
      "stock_prec": 8,
      "money_prec": 2,
      "amount_prec": 4,
      "amount_min": "0.0001",
      "available": true,
      "limits": [
        [
          "2500.0001",
          "3",
          "0.15"
        ],
        [
          "2000.0001",
          "5",
          "0.1"
        ],
        [
          "1500.0001",
          "8",
          "0.063"
        ],
        [
          "1000.0001",
          "10",
          "0.03"
        ],
        [
          "500.0001",
          "15",
          "0.025"
        ],
        [
          "250.0001",
          "20",
          "0.02"
        ],
        [
          "100.0001",
          "30",
          "0.015"
        ],
        [
          "50.0001",
          "50",
          "0.01"
        ],
        [
          "20.0001",
          "100",
          "0.005"
        ]
      ],
      "sort": 1
    }
  ]
}
```

### Response Content

| Field name  | Data Type | Description |
| :---------: | :-------: | :---------: |
|    type     |  integer  |             |
|  leverages  |   array   |             |
|    name     |  string   |    Pair     |
|    stock    |  string   |             |
|    money    |  string   |             |
|  fee_prec   |  integer  |             |
|  tick_size  |  string   |             |
| stock_prec  |  integer  |             |
| money_prec  |  integer  |             |
| amount_prec |  integer  |             |
| amount_min  |  string   |             |
|  available  |   bool    |             |
|   limits    |   array   |             |
|  limits.0   |   array   |             |
| limits.0.0  |  string   |             |
| limits.0.1  |  string   |             |
| limits.0.2  |  string   |             |
|    sort     |  integer  |             |

## Get Market Transactions

```shell
curl "https://api.bitda.com/open/api/v2/market/deals"
```

### HTTP query

Spot

- GET `/open/api/v2/market/deals`

### Request parameters

| Parameter Name | Required | Example | Remark      |
| -------------- | -------- | ------- | ----------- |
| market         | Yes      | ETHUSDT | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "id": 27699216,
      "price": "1573.89",
      "amount": "0.922",
      "type": "buy",
      "time": 1697619536.256684
    }
  ]
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|     id     |  integer  |             |
|   price    |  string   |             |
|   amount   |  string   |             |
|    type    |  string   |             |
|    time    |  integer  |             |

## Get Market Depth

```shell
curl "https://api.bitda.com/open/api/v2/market/depth"

```

### HTTP query

- GET `/open/api/v2/market/depth`

<aside class="Notice">Rate limit 0.1r/s</aside>

### Request parameters

| Field name | Data Type | Required |        Description         |
| :--------: | :-------: | :------: | :------------------------: |
|   market   |  string   |   Yes    | Market name e.g: BTC-USDT  |
|   merge    |  string   |    No    | Merge depth，0，0.01，0.02 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "index_price": "1577.63",
    "sign_price": "1581.5",
    "time": 1697620709569,
    "last": "1573.89",
    "asks": [
      [
        "1621.22",
        "30.613"
      ]
    ],
    "bids": [
      [
        "1573.84",
        "0.819"
      ]
    ]
  }
}
```

### Response Content

| Field name  | Data Type |     Description     |
| :---------: | :-------: | :-----------------: |
| index_price |  string   |                     |
| sign_price  |  string   |                     |
|    time     |  integer  |         ms          |
|    last     |  string   |     last price      |
|    asks     |   array   |                     |
|   asks.0    |   array   | [{price, quantity}] |
|  asks.0.0   |  string   |        price        |
|  asks.0.1   |  string   |      quantity       |
|    bids     |   array   |                     |
|   array.0   |   array   | [{price, quantity}] |
|  array.0.0  |  string   |        price        |
|  array.0.1  |  string   |      quantity       |

## Get Market Status

```shell
curl "https://api.bitda.com/open/api/v2/market/state"

```

### HTTP query
- GET `/open/api/v2/market/state`

<aside class="Notice">Rate limit 10r/s</aside>

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "market": "ETHUSDT",
    "amount": "4753.05",
    "high": "1573.89",
    "last": "1573.89",
    "low": "1571.23",
    "open": "1571.23",
    "change": "0.0016929411989333",
    "period": 86400,
    "volume": "3.02",
    "funding_time": 400,
    "position_amount": "2.100",
    "funding_rate_last": "0.00375",
    "funding_rate_next": "0.00293873",
    "funding_rate_predict": "-0.00088999",
    "insurance": "10500.45426906585552617850",
    "sign_price": "1581.98",
    "index_price": "1578.12",
    "sell_total": "112.974",
    "buy_total": "170.914"
  }
}
```

### Response Content

|      Field name      | Data Type | Description |
| :------------------: | :-------: | :---------: |
|        market        |  string   |             |
|        amount        |  string   |             |
|         high         |  string   |             |
|         last         |  string   |             |
|         low          |  string   |             |
|         open         |  string   |             |
|        change        |  string   |             |
|        period        |  integer  |             |
|        volume        |  string   |             |
|     funding_time     |  integer  |             |
|   position_amount    |  string   |             |
|  funding_rate_last   |  string   |             |
|  funding_rate_next   |  string   |             |
| funding_rate_predict |  string   |             |
|      insurance       |  string   |             |
|      sign_price      |  string   |             |
|     index_price      |  string   |             |
|      sell_total      |  string   |             |
|      buy_total       |  string   |             |

## Get All Market Status

```shell
cuel "https://api.bitda.com/open/api/v2/market/state/all"

```

### HTTP query
- GET `/open/api/v2/market/state/all`

<aside class="Notice">Rate limit 10r/s</aside>

### Request parameters
None

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "1000SHIBUSDT": {
      "market": "1000SHIBUSDT",
      "amount": "0",
      "high": "0.007072",
      "last": "0.007072",
      "low": "0.007072",
      "open": "0.007072",
      "change": "0",
      "period": 86400,
      "volume": "0",
      "funding_time": 400,
      "position_amount": "0",
      "funding_rate_last": "0.00375",
      "funding_rate_next": "0.00375",
      "funding_rate_predict": "0.00375",
      "insurance": "10500.45426906585552617850",
      "sign_price": "0.006919",
      "index_price": "0.006898",
      "sell_total": "6079974",
      "buy_total": "8153875"
    }
  }
}
```

### Response Content

|             Field name             | Data Type | Description |
| :--------------------------------: | :-------: | :---------: |
|          data.market_name          |  object   | market name |
|      data.market_name.market       |  string   |    Price    |
|      data.market_name.amount       |  string   |             |
|       data.market_name.high        |  string   |             |
|       data.market_name.last        |  string   |             |
|        data.market_name.low        |  string   |             |
|       data.market_name.open        |  string   |             |
|      data.market_name.change       |  string   |             |
|      data.market_name.period       |  integer  |             |
|      data.market_name.volume       |  string   |             |
|   data.market_name.funding_time    |  integer  |             |
|  data.market_name.position_amount  |  string   |             |
| data.market_name.funding_rate_last |  string   |             |
| data.market_name.funding_rate_next |  string   |             |
|     data.market_name.insurance     |  string   |             |
|    data.market_name.sign_price     |  string   |             |
|    data.market_name.index_price    |  string   |             |
|    data.market_name.sell_total     |  string   |             |
|     data.market_name.buy_total     |  string   |             |

# Open v2 Private API

## Adjust Position Margin

```shell
curl "https://api.bitda.com/open/api/v2/position/margin"

```

### HTTP query
- POST `/open/api/v2/position/margin`


### Request parameters(form-data parameters)
**Headers**

|  Field name  |      Value       | Required | Example | Remark |
| :----------: | :--------------: | :------: | :-----: | :----: |
| Content-Type | application/json |   Yes    |         |        |

**Body**

| Field name | Data Type | Required |                            Description                            |
| :--------: | :-------: | :------: | :---------------------------------------------------------------: |
|   market   |  string   |   Yes    |                            Market name                            |
|    type    |  integer  |   Yes    | Adjustment type: 1 means increase margin, 2 means decrease margin |
|  quantity  |  string   |   Yes    |                             quantity                              |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "position_id": 4750887, 
    "profit_unreal": "9.0806004045", 
    "finish_type": 1, 
    "liq_time": 0, 
    "create_time": 1697619536.255882, 
    "update_time": 1697620824.557015, 
    "mainten_margin": "0.02", 
    "user_id": 9108, 
    "market": "ETHUSDT", 
    "adl_sort_val": "0.01254794", 
    "type": 1, 
    "liq_price": "1525.67027360000000000644", 
    "side": 2, 
    "open_val_max": "1573.88688000000000000000", 
    "sys": 0, 
    "margin_amount": "79.69434400000000000000", 
    "amount": "1.000", 
    "open_margin": "0.05063536967790213741", 
    "deal_asset_fee": "0", 
    "amount_max": "1.000", 
    "close_left": "1.000", 
    "open_price": "1573.88688000000000000000", 
    "open_val": "1573.88688000000000000000", 
    "open_margin_imply": "0", 
    "mainten_margin_amount": "31.47773760000000000000", 
    "leverage": "20", 
    "profit_real": "-0.78694344000000000000", 
    "profit_clearing": "-0.78694344000000000000", 
    "liq_order_time": 0, 
    "liq_amount": "0", 
    "liq_profit": "0", 
    "liq_order_price": "0", 
    "fee_asset": "", 
    "bkr_price": "1494.19253600000000000644", 
    "liq_price_imply": "0", 
    "bkr_price_imply": "0", 
    "adl_sort": 1, 
    "total": 5
  }
}

```

### Response Content

|      Field name       | Data Type | Description |
| :-------------------: | :-------: | :---------: |
|      position_id      |  integer  |             |
|     profit_unreal     |  string   |             |
|      finish_type      |  integer  |             |
|       liq_time        |  integer  |             |
|      create_time      |   float   |             |
|      update_time      |   float   |             |
|    mainten_margin     |  string   |             |
|        user_id        |  integer  |             |
|        market         |  string   |             |
|     adl_sort_val      |  string   |             |
|         type          |  integer  |             |
|       liq_price       |  string   |             |
|         side          |  integer  |             |
|     open_val_max      |  string   |             |
|          sys          |  integer  |             |
|     margin_amount     |  string   |             |
|        amount         |  string   |             |
|      open_margin      |  string   |             |
|    deal_asset_fee     |  string   |             |
|      amount_max       |  string   |             |
|      close_left       |  string   |             |
|      open_price       |  string   |             |
|       open_val        |  string   |             |
|   open_margin_imply   |  string   |             |
| mainten_margin_amount |  string   |             |
|       leverage        |  string   |             |
|      profit_real      |  string   |             |
|    profit_clearing    |  string   |             |
|    liq_order_time     |  integer  |             |
|      liq_amount       |  string   |             |
|      liq_profit       |  string   |             |
|    liq_order_price    |  string   |             |
|       fee_asset       |  string   |             |
|       bkr_price       |  string   |             |
|    liq_price_imply    |  string   |             |
|    bkr_price_imply    |  string   |             |
|       adl_sort        |  integer  |             |
|         total         |  integer  |             |

## Get User Transaction

```shell
curl "https://api.bitda.com/open/api/v2/order/deals"

```

### HTTP query
- GET `/open/api/v2/order/deals`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
| start_time |  integer  |    No    | Start time  |
|  end_time  |  integer  |    No    |  End time   |
|    page    |  integer  |   Yes    |    Page     |
| page_size  |  integer  |   Yes    |  Page size  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "records": 
    [
      {
        "position_type": 1, 
        "market": "ETHUSDT", 
        "time": 1697619536.256684, 
        "side": 2, 
        "leverage": "20", 
        "user_id": 9108, 
        "order_id": 1470801730, 
        "role": 2, 
        "amount": "0.922", 
        "price": "1573.89", 
        "deal_fee": "0.7255", 
        "deal_stock": "1451.1265", 
        "filled_type": 1, 
        "trade_type": 1, 
        "deal_profit": "--"
      }
    ], 
    "page": 1, 
    "page_size": 10, 
    "count": 0
  }
}

```

### Response Content

|       Field name        | Data Type | Description |
| :---------------------: | :-------: | :---------: |
|         records         |   array   |             |
|        records.0        |  object   |             |
| records.0.position_type |  integer  |             |
|    records.0.market     |  string   |             |
|     records.0.time      |   float   |             |
|     records.0.side      |  integer  |             |
|   records.0.leverage    |  string   |             |
|    records.0.user_id    |  integer  |             |
|   records.0.order_id    |  integer  |             |
|     records.0.role      |  integer  |             |
|    records.0.amount     |  string   |             |
|     records.0.price     |  string   |             |
|   records.0.deal_fee    |  string   |             |
|  records.0.deal_stock   |  string   |             |
|  records.0.filled_type  |  integer  |             |
|  records.0.trade_type   |  integer  |             |
|  records.0.deal_profit  |  string   |             |
|          page           |  integer  |             |
|        page_size        |  integer  |             |
|          count          |  integer  |             |

## Get Completed Orders

```shell
curl "https://api.bitda.com/open/api/v2/order/finished"

```

### HTTP query
- GET `/open/api/v2/order/finished`

<aside class="Notice">Rate limit 1r/s</aside>

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
| start_time |  integer  |    No    | Start time  |
|  end_time  |  integer  |    No    |  End time   |
|    page    |  integer  |   Yes    |    Page     |
| page_size  |  integer  |   Yes    |  Page size  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "records": [
      {
        "order_id": 1470801730, 
        "position_id": 0, 
        "market": "ETHUSDT", 
        "type": 2, 
        "side": 2, 
        "left": "0", 
        "amount": "1", 
        "filled": "1", 
        "deal_fee": "0.7869", 
        "price": "0", 
        "avg_price": "1573.88", 
        "deal_stock": "1573.8868", 
        "position_type": 1, 
        "leverage": "20", 
        "update_time": 1697619536.256684, 
        "create_time": 1697619536.255875, 
        "status": 3, 
        "stop_loss_price": "-", 
        "take_profit_price": "-"
      }
    ], 
    "page": 1, 
    "page_size": 10, 
    "count": 0
  }
}

```

### Response Content

|         Field name          | Data Type | Description |
| :-------------------------: | :-------: | :---------: |
|           records           |   array   |             |
|          records.0          |  object   |             |
|     records.0.order_id      |  integer  |             |
|    records.0.position_id    |  integer  |             |
|      records.0.market       |  string   |             |
|       records.0.type        |  integer  |             |
|       records.0.side        |  integer  |             |
|       records.0.left        |  string   |             |
|      records.0.amount       |  string   |             |
|      records.0.filled       |  string   |             |
|     records.0.deal_fee      |  string   |             |
|       records.0.price       |  string   |             |
|     records.0.avg_price     |  string   |             |
|    records.0.deal_stock     |  string   |             |
|   records.0.position_type   |  integer  |             |
|     records.0.leverage      |  string   |             |
|    records.0.update_time    |   float   |             |
|    records.0.create_time    |   float   |             |
|      records.0.status       |  integer  |             |
|  records.0.stop_loss_price  |  string   |             |
| records.0.take_profit_price |  string   |             |
|            page             |  integer  |             |
|          page_size          |  integer  |             |
|            count            |  integer  |             |


## Market Order

```shell
curl "https://api.bitda.com/open/api/v2/order/market"

```

### HTTP query
- POST `/open/api/v2/order/market`

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |

**Body**

|       Field name       | Data Type | Required |                            Description                            |
| :--------------------: | :-------: | :------: | :---------------------------------------------------------------: |
|         market         |  string   |   Yes    |                            Market name                            |
|          side          |  integer  |   Yes    |                        Side，1 sell，2 buy                        |
|        quantity        |  string   |   Yes    |                             Quantity                              |
|       client_oid       |  string   |    No    |                       User-defined order id                       |
|        is_stop         |  boolean  |    No    |          Is it a take profit order or a stop loss order?          |
|    stop_loss_price     |  string   |    No    |                          Stop loss price                          |
|  stop_loss_price_type  |  integer  |    No    |    Stop price type，1 last price，2 index price，3 sign price     |
|   take_profit_price    |  string   |    No    |                         Take profit price                         |
| take_profit_price_type |  integer  |    No    | Take profit price type，1 last price，2 index price，3 sign price |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": 1471038436
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Cancel all orders in a single market

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel/all"

```

### HTTP query
- POST `/open/api/v2/order/cancel/all`

<aside class="Notice">Rate limit 0.5r/s</aside>

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |

**Body**

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": null
}

```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  object   |             |

## Get Order Details

```shell
curl "https://api.bitda.com/open/api/v2/order/detail"
```

### HTTP query
- GET `/open/api/v2/order/detail`

<aside class="Notice">Rate limit 6r/s</aside>

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
|  order_id  |  integer  |   Yes    |  Order id   |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "order_id": 1470445037, 
    "position_id": 0, 
    "market": "ETHUSDT", 
    "type": 2, 
    "side": 1, 
    "left": "0", 
    "amount": "1", 
    "filled": "1", 
    "deal_fee": "0.7869", 
    "price": "0", 
    "avg_price": "1573.84", 
    "deal_stock": "1573.84", 
    "position_type": 1, 
    "leverage": "20", 
    "update_time": 1697616547.90107, 
    "create_time": 1697616547.901067, 
    "status": 3, 
    "stop_loss_price": "-", 
    "take_profit_price": "-",
    "client_oid":"36341ddd362363263626"
  }
}

```

### Response Content

|    Field name     | Data Type | Description |
| :---------------: | :-------: | :---------: |
|     order_id      |  integer  |  Order id   |
|    position_id    |  integer  |             |
|      market       |  string   |             |
|       type        |  integer  |             |
|       side        |  integer  |             |
|       left        |  string   |             |
|      amount       |  string   |             |
|      filled       |  string   |             |
|     deal_fee      |  string   |             |
|       price       |  string   |             |
|     avg_price     |  string   |             |
|    deal_stock     |  string   |             |
|   position_type   |  integer  |             |
|     leverage      |  string   |             |
|    update_time    |   float   |             |
|    create_time    |   float   |             |
|      status       |  integer  |             |
|  stop_loss_price  |  string   |             |
| take_profit_price |  string   |             |

## Cancel Order Batch

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel/batch"
```

### HTTP query
- POST `/open/api/v2/order/cancel/batch`

<aside class="Notice">Rate limit 6r/s</aside>

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
| order_ids  |  string   |   Yes    |  Order ids  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "order_ids": [
      1470445037,
      1470445038
    ]
  }
}

```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Cancel Order

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel"
```

### HTTP query
- POST `/open/api/v2/order/cancel`

<aside class="Notice">Rate limit 6r/s</aside>

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
|  order_id  |  integer  |   Yes    |  Order id   |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": 1471202902
}

```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Submit Limit Order

```shell
"https://api.bitda.com/open/api/v2/order/limit"
```

### HTTP query
- POST `/open/api/v2/order/limit`

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

|       Field name       | Data Type | Required |                               Description                               |
| :--------------------: | :-------: | :------: | :---------------------------------------------------------------------: |
|         market         |  string   |   Yes    |                               Market name                               |
|          side          |  integer  |   Yes    |                           Side，1 sell，2 buy                           |
|         price          |  string   |   Yes    |                                  Price                                  |
|        quantity        |  string   |   Yes    |                                Quantity                                 |
|       client_oid       |  string   |    No    |                        User-defined order number                        |
|        is_stop         |  boolean  |    No    |             Is it a take profit order or a stop loss order              |
|    stop_loss_price     |  string   |    No    |                             Stop loss price                             |
|  stop_loss_price_type  |  string   |    No    |  Stop loss order price type，1 last price，2 index price，3 sign price  |
|   take_profit_price    |  string   |    No    |                         Take profit order price                         |
| take_profit_price_type |  string   |    No    | Take profit order price type，1 last price，2 index price，3 sign price |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": 1471038436
}

```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Get the entrusted order

```shell
"https://api.bitda.com/open/api/v2/order/pending"

```

### HTTP query
- GET `/open/api/v2/order/pending`

<aside class="Notice">Rate limit 6r/s</aside>

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |    No    | Market name |
|    page    |  integer  |    No    |    Page     |
| page_size  |  integer  |    No    |  Page size  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success",
  "data": {
    "records": [
      {
        "order_id": 1471038436, 
        "position_id": 0, 
        "market": "ETHUSDT", 
        "type": 1, 
        "side": 1, 
        "left": "1.000", 
        "amount": "1.000", 
        "filled": "0", 
        "deal_fee": "0", 
        "price": "1700", 
        "avg_price": "", 
        "deal_stock": "0", 
        "position_type": 1, 
        "leverage": "20", 
        "update_time": 1697621580.978632, 
        "create_time": 1697621580.978632, 
        "status": 1, 
        "stop_loss_price": "-", 
        "take_profit_price": "-",
        "client_oid":"36341ddd362363263626"
      }
    ], 
    "page": 1, 
    "page_size": 10,
    "count": 1
  }
}

```

### Response Content

|         Field name          | Data Type | Description |
| :-------------------------: | :-------: | :---------: |
|           records           |   array   |             |
|          records.0          |  object   |             |
|     records.0.order_id      |  integer  |             |
|    records.0.position_id    |  integer  |             |
|      records.0.market       |  string   |             |
|       records.0.type        |  integer  |             |
|       records.0.side        |  integer  |             |
|       records.0.left        |  string   |             |
|      records.0.amount       |  string   |             |
|      records.0.filled       |  string   |             |
|     records.0.deal_fee      |  string   |             |
|       records.0.price       |  string   |             |
|     records.0.avg_price     |  string   |             |
|    records.0.deal_stock     |  string   |             |
|   records.0.position_type   |  string   |             |
|     records.0.leverage      |  string   |             |
|    records.0.update_time    |   float   |             |
|    records.0.create_time    |   float   |             |
|      records.0.status       |  integer  |             |
|  records.0.stop_loss_price  |  string   |             |
| records.0.take_profit_price |  string   |             |
|            page             |  integer  |             |
|          page_size          |  integer  |             |
|            count            |  integer  |             |

## Submit stop order

```shell
"https://api.bitda.com/open/api/v2/order/stop"

```

### HTTP query
- POST `/open/api/v2/order/stop`

<aside class="Notice">Rate limit 5r/s</aside>

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

|   Field name    | Data Type | Required |                                   Description                                   |
| :-------------: | :-------: | :------: | :-----------------------------------------------------------------------------: |
|     market      |  string   |   Yes    |                                   Market name                                   |
|      side       |  integer  |   Yes    |                               Side，1 sell，2 buy                               |
|   order_price   |  string   |    No    | Order price，If left blank, the order will be placed based on the market price. |
|   stop_price    |  string   |   Yes    |                                   Stop price                                    |
|    cut_price    |  string   |   Yes    |                                Last market price                                |
| stop_price_type |  integer  |   Yes    |              Price type，1 last price，2 index price，3 sign price              |
|    quantity     |  string   |   Yes    |                                    Quantity                                     |

> Responds:

```json
{"code": 0, "msg": "success", "data": null}

```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  object   |    null     |

## Cancel stop order

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/cancel"
```

### HTTP query
- POST `/open/api/v2/order/stop/cancel`

<aside class="Notice">Rate limit 10r/s</aside>

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
|  order_id  |  string   |   Yes    |  Order id   |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": 1471202902
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

<aside class="Notice">Rate limit 1r/s</aside>

## Cancel all stop orders for a single market

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/cancel/all"

```

### HTTP query
- POST `/open/api/v2/order/stop/cancel/all`

### Request parameters
**Headers**

| Parameter Name | Value            | Required | Example | Remark |
| -------------- | ---------------- | -------- | ------- | ------ |
| Content-Type   | application/json | Yes      |         |        |
**Body**

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  object   |             |


## Get the stop order in the commission

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/pending"

```

### HTTP query
- GET `/open/api/v2/order/stop/pending`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
|    page    |  integer  |   Yes    |    Page     |
| page_size  |  integer  |   Yes    |  Page size  |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "contract_order_id": "1693809549574678795",
        "order_id": 0,
        "position_id": 0,
        "market": "BTCUSDT",
        "contract_order_type": 0,
        "trigger_type": 3,
        "trigger_price": "30000",
        "order_price": "0",
        "size": "1",
        "order_time": 1693809549,
        "update_time": 1693809549,
        "cut_price": "",
        "status": 2,
        "side": 1,
        "position_type": 1,
        "leverage": "100",
        "entrust_id": "gyWTY4NgLg"
      }
    ],
    "page": 1,
    "page_size": 1,
    "count": 2
  }
}
```

### Response Content

|          Field name           | Data Type | Description |
| :---------------------------: | :-------: | :---------: |
|            records            |   array   |             |
|           records.0           |  object   |             |
|  records.0.contract_order_id  |  string   |             |
|      records.0.order_id       |  integer  |             |
|     records.0.position_id     |  integer  |             |
|       records.0.market        |  string   |             |
| records.0.contract_order_type |  integer  |             |
|    records.0.trigger_type     |  integer  |             |
|    records.0.trigger_price    |  string   |             |
|     records.0.order_price     |  string   |             |
|        records.0.size         |  string   |             |
|     records.0.order_time      |  integer  |             |
|     records.0.update_time     |  integer  |             |
|      records.0.cut_price      |  string   |             |
|       records.0.status        |  integer  |             |
|        records.0.side         |  integer  |             |
|    records.0.position_type    |  integer  |             |
|      records.0.leverage       |  string   |             |
|     records.0.entrust_id      |  string   |             |
|             page              |  integer  |             |
|           page_size           |  integer  |             |
|             count             |  integer  |             |


## Get completed stop order

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/finished"
```

### HTTP query
- GET `/open/api/v2/order/stop/finished`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |
|    page    |  integer  |   Yes    |    Page     |
| page_size  |  integer  |   Yes    |  Page size  |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "contract_order_id": "1693479864228767250",
        "order_id": 0,
        "position_id": 0,
        "market": "BTCUSDT",
        "contract_order_type": 0,
        "trigger_type": 3,
        "trigger_price": "38000",
        "order_price": "0",
        "size": "1",
        "order_time": 1693479864,
        "update_time": 1693479966,
        "cut_price": "",
        "status": 4,
        "side": 1,
        "position_type": 1,
        "leverage": "100",
        "entrust_id": "gxf0Qpu1WE"
      }
    ],
    "page": 1,
    "page_size": 10,
    "count": 6
  }
}
```

### Response Content

|          Field name           | Data Type | Description |
| :---------------------------: | :-------: | :---------: |
|            records            |   array   |             |
|           records.0           |  object   |             |
|  records.0.contract_order_id  |  string   |             |
|      records.0.order_id       |  integer  |             |
|     records.0.position_id     |  integer  |             |
|       records.0.market        |  string   |             |
| records.0.contract_order_type |  integer  |             |
|    records.0.trigger_type     |  integer  |             |
|    records.0.trigger_price    |  string   |             |
|     records.0.order_price     |  string   |             |
|        records.0.size         |  string   |             |
|     records.0.order_time      |  integer  |             |
|     records.0.update_time     |  integer  |             |
|      records.0.cut_price      |  string   |             |
|       records.0.status        |  integer  |             |
|        records.0.side         |  integer  |             |
|    records.0.position_type    |  integer  |             |
|      records.0.leverage       |  string   |             |
|     records.0.entrust_id      |  string   |             |
|             page              |  integer  |             |
|           page_size           |  integer  |             |
|             count             |  integer  |             |

## Adjust market opening leverage/position mode

```shell
curl "https://api.bitda.com/open/api/v2/setting/leverage"
```

### HTTP query
- POST `/open/api/v2/setting/leverage`

### Request parameters
**Headers**

| Field Name   | Value            | Required | Example | Remark |
| ------------ | ---------------- | -------- | ------- | ------ |
| Content-Type | application/json | Yes      |         |        |
**Body**

|  Field name   | Data Type | Required |                     Description                     |
| :-----------: | :-------: | :------: | :-------------------------------------------------: |
|    market     |  string   |   Yes    |                     Market name                     |
|   leverage    |  string   |   Yes    |                      Leverage                       |
| position_type |  string   |   Yes    | Position type，1 Isolated position 2 Cross position |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  object   |             |

## Query the market opening leverage/position mode

```shell
curl "https://api.bitda.com/open/api/v2/setting/leverage"
```

### HTTP query
- GET `/open/api/v2/setting/leverage`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |   Yes    | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "leverage": "100",
    "position_type": 1
  }
}
```

### Response Content

|  Field name   | Data Type | Description |
| :-----------: | :-------: | :---------: |
|   leverage    |  string   |             |
| position_type |  integer  |             |


## Query Assets

```shell
curl "https://api.bitda.com/open/api/v2/asset/query"
```

### HTTP query
- GET `/open/api/v2/asset/query`

### Request parameters
None

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "USDT": {
      "available": "8300.569",
      "frozen": "0",
      "margin": "0",
      "balance_total": "8300.569",
      "profit_unreal": "0",
      "transfer": "8300.569",
      "bonus": "0"
    },
  }
}
```

### Response Content

|           Field name           | Data Type | Description |
| :----------------------------: | :-------: | :---------: |
|        data.market_name        |  object   |             |
|   data.market_name.available   |  string   |             |
|    data.market_name.frozen     |  string   |             |
|    data.market_name.margin     |  string   |             |
| data.market_name.balance_total |  string   |             |
| data.market_name.profit_unreal |  string   |             |
|   data.market_name.transfer    |  string   |             |
|     data.market_name.bonus     |  string   |             |

## Query Asset Bill

```shell
curl "https://api.bitda.com/open/api/v2/asset/history"
```

### HTTP query
- GET `/open/api/v2/asset/history`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   asset    |  string   |   Yes    | Asset name  |
|  business  |  string   |    No    |  Business   |
| start_time |  integer  |    No    | Start time  |
|  end_time  |  integer  |    No    |  End time   |
|    page    |  integer  |   Yes    |    Page     |
| page_size  |  integer  |   Yes    |  Page size  |


> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "id": 1562,
        "time": 1693496496.157132,
        "user_id": 9108,
        "asset": "USDT",
        "business": "Close pnl",
        "change": "-482.541",
        "balance": "8300.569"
      }
    ],
    "page": 1,
    "page_size": 1,
    "count": 406
  }
}
```

### Response Content

|     Field name     | Data Type | Description |
| :----------------: | :-------: | :---------: |
|      records       |   array   |             |
|     records.0      |  object   |             |
|    records.0.id    |  integer  |             |
|   records.0.time   |   float   |             |
| records.0.user_id  |  integer  |             |
|  records.0.asset   |  string   |             |
| records.0.business |  string   |             |
|  records.0.change  |  string   |             |
| records.0.balance  |  string   |             |

## User Positions

```shell
curl "https://api.bitda.com/open/api/v2/position/pending"
```

### HTTP query
- GET `/open/api/v2/position/pending`

### Request parameters

| Field name | Data Type | Required | Description |
| :--------: | :-------: | :------: | :---------: |
|   market   |  string   |    No    | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "position_id": 2497913,
      "create_time": 1693809270.599202,
      "update_time": 1693809270.599314,
      "user_id": 9108,
      "market": "BTCUSDT",
      "type": 1,
      "side": 2,
      "amount": "1.0000",
      "close_left": "1.0000",
      "open_price": "25992.1",
      "open_margin": "0.01",
      "margin_amount": "259.921",
      "leverage": "100",
      "profit_unreal": "-18.5706",
      "liq_price": "25862.13",
      "mainten_margin": "0.005",
      "mainten_margin_amount": "129.9605",
      "adl_sort": 5,
      "roe": "-0.0714",
      "stop_loss_price": "-",
      "take_profit_price": "-"
    }
  ]
}
```

### Response Content

|          Field name          | Data Type | Description |
| :--------------------------: | :-------: | :---------: |
|             data             |   array   |             |
|            data.0            |  object   |             |
|      data.0.position_id      |  integer  |             |
|      data.0.create_time      |   float   |             |
|      data.0.update_time      |   float   |             |
|        data.0.user_id        |  integer  |             |
|        data.0.market         |  string   |             |
|         data.0.type          |  integer  |             |
|         data.0.side          |  integer  |             |
|        data.0.amount         |  string   |             |
|      data.0.close_left       |  string   |             |
|      data.0.open_price       |  string   |             |
|      data.0.open_margin      |  string   |             |
|     data.0.margin_amount     |  string   |             |
|       data.0.leverage        |  string   |             |
|     data.0.profit_unreal     |  string   |             |
|       data.0.liq_price       |  string   |             |
|    data.0.mainten_margin     |  string   |             |
| data.0.mainten_margin_amount |  string   |             |
|       data.0.adl_sort        |  integer  |             |
|          data.0.roe          |  string   |             |
|    data.0.stop_loss_price    |  string   |             |
|   data.0.take_profit_price   |  string   |             |

## Get adjustable margin

```shell
curl "https://api.bitda.com/open/api/v2/position/margin"
```

### HTTP query
- GET `/open/api/v2/position/margin`

### Request parameters

| Field name  | Data Type | Required | Description |
| :---------: | :-------: | :------: | :---------: |
| position_id |  integer  |    No    | Position id |
|   market    |  string   |    No    | Market name |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "amount": "1.0000",
    "available": "8410.56909378778272067016",
    "leverage": "100",
    "margin_amount": "372.541",
    "max_removable_margin": "64.7714395551"
  }
}
```

### Response Content

|      Field name      | Data Type | Description |
| :------------------: | :-------: | :---------: |
|        amount        |  string   |             |
|      available       |  string   |             |
|       leverage       |  string   |             |
|    margin_amount     |  string   |             |
| max_removable_margin |  string   |             |

## Limit Close

```shell
curl "https://api.bitda.com/open/api/v2/position/close/limit"
```

### HTTP query
- POST `/open/api/v2/position/close/limit`

### Request parameters
**Headers**

| Parameter Name | Value            | Required | Example | Remark |
| -------------- | ---------------- | -------- | ------- | ------ |
| Content-Type   | application/json | Yes      |         |        |
**Body**

| Field name  | Data Type | Required |      Description      |
| :---------: | :-------: | :------: | :-------------------: |
|   market    |  string   |   Yes    |      Market name      |
| position_id |  integer  |   Yes    |      Position id      |
|  quantity   |  string   |   Yes    |   Closing quantity    |
|    price    |  string   |   Yes    |     Closing price     |
| client_oid  |  string   |    No    | User-defined order id |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": 325235235
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Market Close

```shell
curl "https://api.bitda.com/open/api/v2/position/close/market"
```

### HTTP query
- POST `/open/api/v2/position/close/market`

### Request parameters
**Headers**

| Parameter Name | Value            | Required | Example | Remark |
| -------------- | ---------------- | -------- | ------- | ------ |
| Content-Type   | application/json | Yes      |         |        |
**Body**

| Field name  | Data Type | Required |      Description      |
| :---------: | :-------: | :------: | :-------------------: |
|   market    |  string   |   Yes    |      Market name      |
| position_id |  integer  |   Yes    |      Position id      |
| client_oid  |  string   |    No    | User-defined order id |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": 235235325
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  integer  |             |

## Position Take Profit And Stop Loss Setting/Modification

```shell
curl "https://api.bitda.com/open/api/v2/position/close/stop"
```

### HTTP query
- POST `/open/api/v2/position/close/stop`

### Request parameters
**Headers**

| Parameter Name | Value            | Required | Example | Remark |
| -------------- | ---------------- | -------- | ------- | ------ |
| Content-Type   | application/json | Yes      |         |        |
**Body**

|       Field name       | Data Type | Required |      Description       |
| :--------------------: | :-------: | :------: | :--------------------: |
|         market         |  string   |   Yes    |      Market name       |
|      position_id       |  integer  |   Yes    |      Position id       |
|    stop_loss_price     |  string   |    No    |    Stop loss price     |
|  stop_loss_price_type  |  integer  |    No    |  Stop loss price type  |
|   take_profit_price    |  string   |    No    |   Take profit price    |
| take_profit_price_type |  integer  |    No    | Take profit price type |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### Response Content

| Field name | Data Type | Description |
| :--------: | :-------: | :---------: |
|    data    |  object   |             |


# API Example

> Python:

```python
# -*- coding:utf-8 -*-
import json
import requests
import time
import hmac
import hashlib

host = "baseurl"
client_id = ""
client_key = ""


def gen_sign(client_id, client_key):
    ts = int(time.time())
    nonce = "abcdefg"
    obj = {"ts": ts, "nonce": nonce, "sign": "", "client_id": client_id}
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts)
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj["sign"] = v.hexdigest()
    return obj

# market api
def market_list():
    print("> market list")
    path = "/market/list"
    res = requests.get(host + path)
    print(json.dumps(res.json()))

# setting api
def adjust_leverage(market: str, leverage: int, position_type: int):
    print("> adjust leverage")
    path = "/setting/leverage"
    obj = gen_sign(client_id, client_key)
    obj.update({"market": market, "leverage": leverage, "position_type": position_type})
    res = requests.post(host + path, data=obj)
    print(json.dumps(res.json()))
    
# asset api
def get_asset():
    print(">get asset")
    path = "/asset/query"
    obj = gen_sign(client_id, client_key)
    res = requests.get(host + path, params=obj)
    print(json.dumps(res.json()))

# order api
def put_limit_order(
        market: str,
        side: int,
        price: str,
        quantity: str,
        client_oid: str,
        is_stop: bool,
        stop_loss_price: str,
        stop_loss_price_type: int,
        take_profit_price: str,
        take_profit_price_type: int
):
    print("> put limit order")
    path = "/order/limit"
    obj = gen_sign(client_id, client_key)
    obj.update(
        {
            "market": market,
            "side": side,
            "price": price,
            "quantity": quantity,
            "client_oid": client_oid,
            "is_stop": is_stop,
            "stop_loss_price": stop_loss_price,
            "stop_loss_price_type": stop_loss_price_type,
            "take_profit_price": take_profit_price,
            "take_profit_price_type": take_profit_price_type,
        }
    )
    res = requests.post(host + path, data=obj)
    print(json.dumps(res.json()))
```

# Websocket Example

> Python:

```python
# -*- coding:utf-8 -*-
import datetime
import time
import hmac
import hashlib
import websockets
import json
import asyncio
import uvloop

asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())

host = "baseurl"
client_id = ""
client_key = ""


async def ping(ws):
    obj = {"method": "ping"}
    op = json.dumps(obj)
    await ws.send(op)


def sign():
    ts = str(int(time.time()))
    nonce = "abcdefg"
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts)
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj = {
        "method": "sign",
        "params": {
            "client_id": client_id,
            "nonce": nonce,
            "ts": str(ts),
            "sign": v.hexdigest()
        },
        "id": 0,
    }
    return obj


async def depth(ws):
    obj = {
        "method": "subscribe.depth",
        "params": {
            "market": "BTCUSDT",
            "merge": "0",
        },
        "id": 0,
    }
    print(obj)
    await ws.send(json.dumps(obj))

async def user_asset(ws):
    obj = {
        "method": "subscribe.asset",
        "id": 0,
    }
    print(obj)
    await ws.send(json.dumps(obj))


async def startup():
    print("start to connect %s..." % host)
    ws = await websockets.connect(host)

    await ws.send(json.dumps(sign()))
    await ping(ws)
    await depth(ws)
    await user_asset(ws)

    while 1:
        try:
            data = await ws.recv()
            print(datetime.datetime.now(), data)
        except websockets.exceptions.ConnectionClosed as e:
            print("connect closed...", e)
            return
        except:
            pass


if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(startup())

```


