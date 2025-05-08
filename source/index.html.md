---
title: BitDa Futures API 文档

language_tabs: # must be one of https://git.io/vQNgJ

- shell

search: False
---

# 简介

## API 简介

欢迎使用BitDa API！ 你可以使用此 API 获得市场行情数据，进行交易，并且管理你的账户。

在文档的右侧是代码，目前我们仅提供针对 `shell` 的代码示例。

可以使用以下域名访问：api.bitda.com

欢迎有优秀 maker 策略且交易量大的机构参与长期做市商项目。

## 公共接口
不需要鉴权可访问接口如下：

| 接口                                                        | 简介             | 市场 |
| ----------------------------------------------------------- | ---------------- | ---- |
| [GET /open/api/v2/market/kline](#get-market-k-line)         | 获取市场K线      | 合约 |
| [GET  /open/api/v2/market/list](#get-market-list)           | 获取市场列表     | 合约 |
| [GET /open/api/v2/market/deals](#get-market-transactions)   | 获取市场成交     | 合约 |
| [GET /open/api/v2/market/depth](#get-market-depth)          | 获取市场深度     | 合约 |
| [GET /open/api/v2/market/state](#get-market-status)         | 获取市场状态     | 合约 |
| [GET /open/api/v2/market/state/all](#get-all-market-status) | 获取所有市场状态 | 合约 |

## 鉴权接口

可以访问的接口如下：

| 接口                                                                                             | 简介                   | 市场 |
| ------------------------------------------------------------------------------------------------ | ---------------------- | ---- |
| [POST /open/api/v2/position/margin](#adjust-position-margin)                                     | 调整持仓保证金         | 合约 |
| [GET /open/api/v2/order/deals](#get-user-transaction)                                            | 获取成交记录           | 合约 |
| [GET /open/api/v2/order/finished](#get-completed-orders)                                         | 获取历史委托           | 合约 |
| [POST /open/api/v2/order/market](#market-order)                                                  | 市价下单               | 合约 |
| [POST /open/api/v2/order/cancel/all](#cancel-all-orders-in-a-single-market)                      | 取消所有委托           | 合约 |
| [GET /open/api/v2/order/detail](#get-order-details)                                              | 获取委托详情           | 合约 |
| [POST /open/api/v2/order/cancel/batch](#batch-cancel-orders-in-a-single-market)                  | 批量取消委托           | 合约 |
| [POST /open/api/v2/order/cancel](#cancel-order)                                                  | 取消委托               | 合约 |
| [POST /open/api/v2/order/limit](#submit-limit-order)                                             | 限价下单               | 合约 |
| [GET /open/api/v2/order/pending](#get-the-entrusted-order)                                       | 获取当前委托           | 合约 |
| [POST /open/api/v2/order/stop](#submit-stop-order)                                               | 条件单下单             | 合约 |
| [POST /open/api/v2/order/stop/cancel](#cancel-stop-order)                                        | 取消条件单             | 合约 |
| [POST /open/api/v2/order/stop/cancel/all](#cancel-all-stop-orders-for-a-single-market)           | 取消所有条件单         | 合约 |
| [GET /open/api/v2/order/stop/pending](#get-the-stop-order-in-the-commission)                     | 获取当前条件单         | 合约 |
| [GET /open/api/v2/order/stop/finished](#get-completed-stop-order)                                | 获取历史条件单         | 合约 |
| [POST /open/api/v2/setting/leverage](#adjust-market-opening-leverage-position-mode)              | 调整持仓杠杆和持仓模式 | 合约 |
| [GET /open/api/v2/setting/leverage](#query-the-market-opening-leverage-position-mode)            | 查询持仓杠杆和持仓模式 | 合约 |
| [GET /open/api/v2/asset/query](#query-assets)                                                    | 获取资产详情           | 合约 |
| [GET /open/api/v2/asset/history](#query-asset-bill)                                              | 获取资产历史           | 合约 |
| [GET /open/api/v2/position/pending](#user-positions)                                             | 获取当前持仓           | 合约 |
| [GET /open/api/v2/position/margin](#get-adjustable-margin)                                       | 获取持仓可调整保证金   | 合约 |
| [POST /open/api/v2/position/close/limit](#limit-close)                                           | 限价平仓               | 合约 |
| [POST /open/api/v2/position/close/market](#market-close)                                         | 市价平仓               | 合约 |
| [POST /open/api/v2/position/close/stop](#position-take-profit-and-stop-loss-settingmodification) | 持仓止盈止损           | 合约 |

# 接入说明

## Restful Host:

    https://api.bitda.com

## Websocket Host:

    合约交易 
    wss://ws.bitda.com/wsf

## 鉴权说明

1. 所有鉴权接口都需要进行鉴权，参数为client_id, ts, nonce, sign。client_id是api key, client_key为密钥，请妥善保管。

2. client_id为api key，ts为当前时间戳，与服务器时间差正负5秒会被拒绝，nonce为随机字符串，不能与上次请求所使用相同。

3. 签名方法, 将client_id, ts, nonce进行排序连接，使用hmac-sha256方法进行签名，例如待签名字符串为: client_id=abc&nonce=xyz&ts=1571293029

4. 签名: sign = hmac.New(client_key, sign_str, sha256)

5. Content-Type: application/x-www-form-urlencoded

6. 合约接口，post接口请求请将参数放在请求体里面，get接口请求携带在url链接中。
   
# WebSocket说明

1. 需要先进行鉴权，才可进行订阅。
   
2. 鉴权格式: {"method": "sign", "params": {"ts":"", "nonce":"", "client_id":"", "sign": ""}, "id": 1},如:{"method": "sign", "params": {"ts":"1738944827", "nonce":"abcdef", "client_id":"xxxx", "sign": "xxxxx"}, "id": 1}
   
3. 心跳处理，客户端需定时上发心跳信息，任意字符串，服务端每30秒会检查心跳，超时没有收到自动关闭连接。{"method": "ping", "params": {}, "id": 1}


## Websocket 公共接口

## 市场KLine
### 请求参数

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
  "method":"unsubscribe.kline",
  "params":{
    "market":"BTCUSDT",
    "period":"15min"
  },
  "id":6288
}
```

| 参数名 |  描述   |
| :----: | :-----: |
| market | BTCUSDT |
| period |  15min  |

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
### 数据更新字段列表
|  参数名  | 参数类型 |    描述    |
| :------: | :------: | :--------: |
|   data   |  array   |            |
|  data.0  |  array   |            |
| data.0.0 | integer  | 时间戳 秒  |
| data.0.1 |  string  |   开盘价   |
| data.0.2 |  string  |   最高价   |
| data.0.3 |  string  |   最低价   |
| data.0.4 |  string  |   收盘价   |
| data.0.5 |  string  |  成交数量  |
| data.0.6 |  string  |  成交金额  |
| data.0.7 |  string  | 市场交易对 |


## 市场成交
### 请求参数

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

> UnSub

```json
{
  "method":"unsubscribe.trade",
  "id":1564,
  "params":{
    "market":"BTCUSDT"
  }
}
```

| 参数名 |  描述   |
| :----: | :-----: |
| market | BTCUSDT |

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
### 数据更新字段列表
| 参数名 | 参数类型 |          描述           |
| :----: | :------: | :---------------------: |
| amount |  string  |          数量           |
|   id   | integer  |         订单ID          |
| market |  string  |       市场交易对        |
| price  |  string  |        成交价格         |
|  time  |  float   |        成交时间         |
|  type  |  string  | 成交方向: buy买, sell卖 |


## 市场深度
### 请求参数

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

> UnSub

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

| 参数名 |  描述   |
| :----: | :-----: |
| market | BTCUSDT |
| merge  |    0    |

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
### 数据更新字段列表
|  参数名  | 参数类型 | 描述  |
| :------: | :------: | :---: |
|   asks   |  array   |       |
|  asks.0  |  array   |       |
| asks.0.0 |  string  |       |
| asks.0.1 |  string  |       |
|   bids   |  array   |       |
|  bids.0  |  array   |       |
| bids.0.0 |  string  |       |
| bids.0.1 |  string  |       |


## 市场行情
### 请求参数

> Sub

```json
{
  "method":"subscribe.state",
  "id":1953,
  "params":{}
}

```

> UnSub

```json
{
  "method":"unsubscribe.state",
  "id":1953,
  "params":{}
}
```

| 参数名 | 描述  |
| :----: | :---: |

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
### 数据更新字段列表
|              参数名              | 参数类型 | 描述  |
| :------------------------------: | :------: | :---: |
|           market_name            |  object  |       |
|        market_name.market        |  string  |       |
|        market_name.amount        |  string  |       |
|         market_name.high         |  string  |       |
|         market_name.last         |  string  |       |
|         market_name.low          |  string  |       |
|         market_name.open         |  string  |       |
|        market_name.period        | integer  |       |
|        market_name.volume        |  string  |       |
|        market_name.change        |  string  |       |
|     market_name.funding_time     | integer  |       |
|   market_name.position_amount    |  string  |       |
|  market_name.funding_rate_last   |  string  |       |
|  market_name.funding_rate_next   |  string  |       |
| market_name.funding_rate_predict |  string  |       |
|      market_name.insurance       |  string  |       |
|      market_name.sign_price      |  string  |       |
|     market_name.index_price      |  string  |       |
|      market_name.sell_total      |  string  |       |
|      market_name.buy_total       |  string  |       |


## 心跳
### 请求参数

> Sub

```json
{"method":"ping"}
```

| 参数名 | 描述  |
| :----: | :---: |

> Responds:

```json
{
  "id":0,
  "method":"ping",
  "result":"success",
  "error":null
}
```
### 数据更新字段列表
| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |


## Websocket鉴权接口

## 登陆
### 请求参数

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

|  参数名   |     描述      |
| :-------: | :-----------: |
| client_id | 你的client ID |
|   nonce   |     nonce     |
|    ts     |   timestamp   |
|   sign    |     签名      |

> Responds:

```json
{
  "id":7681,
  "method":"sign",
  "result":"success",
  "error":null
}
```
### 数据更新字段列表
| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |



## 用户持仓

### 请求参数

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

| 参数名 | 描述  |
| :----: | :---: |

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
### 数据更新字段列表
|             参数名             | 参数类型 | 描述  |
| :----------------------------: | :------: | :---: |
|             event              | integer  |       |
|            position            |  object  |       |
|      position.position_id      | integer  |       |
|      position.create_time      |  float   |       |
|      position.update_time      |  float   |       |
|        position.user_id        | integer  |       |
|        position.market         |  string  |       |
|         position.type          | integer  |       |
|         position.side          | integer  |       |
|        position.amount         |  string  |       |
|      position.close_left       |  string  |       |
|      position.open_price       |  string  |       |
|      position.open_margin      |  string  |       |
|     position.margin_amount     |  string  |       |
|       position.leverage        |  string  |       |
|     position.profit_unreal     |  string  |       |
|       position.liq_price       |  string  |       |
|    position.mainten_margin     |  string  |       |
| position.mainten_margin_amount |  string  |       |
|       position.adl_sort        | integer  |       |
|          position.roe          |  string  |       |
|     position.margin_ratio      |  string  |       |
|    position.stop_loss_price    |  string  |       |
|   position.take_profit_price   |  string  |       |


## 用户订单
### 请求参数

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

| 参数名 | 描述  |
| :----: | :---: |

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
### 数据更新字段列表
|      参数名       | 参数类型 | 描述  |
| :---------------: | :------: | :---: |
|     order_id      | integer  |       |
|    position_id    | integer  |       |
|      market       |  string  |       |
|       type        | integer  |       |
|       side        | integer  |       |
|       left        |  string  |       |
|      amount       |  string  |       |
|      filled       |  string  |       |
|     deal_fee      |  string  |       |
|       price       |  string  |       |
|     avg_price     |  string  |       |
|    deal_stock     |  string  |       |
|   position_type   | integer  |       |
|     leverage      |  string  |       |
|    update_time    |  float   |       |
|    create_time    |  float   |       |
|      status       | integer  |       |
|  stop_loss_price  |  string  |       |
| take_profit_price |  string  |       |


## 用户资产

### 请求参数

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

| 参数名 | 描述  |
| :----: | :---: |

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
### 数据更新字段列表
|          参数名          | 参数类型 | 描述  |
| :----------------------: | :------: | :---: |
|        marke_name        |  object  |       |
|   marke_name.available   |  string  |       |
|    marke_name.frozen     |  string  |       |
|    marke_name.margin     |  string  |       |
| marke_name.balance_total |  string  |       |
| marke_name.profit_unreal |  string  |       |
|   marke_name.transfer    |  string  |       |
|     marke_name.bonus     |  string  |       |


# Open v2公共接口

<h2 id="get-market-k-line">获取市场K线</h2>


```shell
curl "https://api.bitda.com/open/api/v2/market/kline"
```

### HTTP请求

- GET `/open/api/v2/market/kline`

<aside class="Notice">限速1r/s</aside>

### 请求参数

| 参数名     | 参数类型 | 是否必须 | 描述                                                                 |
| ---------- | -------- | -------- | -------------------------------------------------------------------- |
| market     | string   | 是       | ETHUSDT, 市场交易对                                                  |
| type       | string   | 是       | K-line周期,1min,5min,15min,30min,1hour,4hour,6hour,12hour,1day,1week |
| start_time | integer  | 否       | 开始时间, 秒                                                         |
| end_time   | integer  | 否       | 结束时间, 秒                                                         |

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

### 返回字段

| 参数名 | 参数类型 |  描述  |
| :----: | :------: | :----: |
|  time  | integer  |  时间  |
|  open  |  string  | 开盘价 |
|  high  |  string  | 最高价 |
|  low   |  string  | 最低价 |
| close  |  string  | 收盘价 |
| volume |  string  | 成交量 |

<h2 id="get-market-list">获取市场列表</h2>

```shell
curl "https://api.bitda.com/open/api/v2/market/list"
```

### HTTP请求

- GET `/open/api/v2/market/list`

### 请求参数

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
      "name": "BTCUSDT",
      "stock": "BTC",
      "money": "USDT",
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
      "sort": 1,
      "maker_fee": "0.0005",
      "taker_fee": "0.0006"
    }
  ]
}
```

### 返回字段

|   参数名    | 参数类型 |                   描述                   |
| :---------: | :------: | :--------------------------------------: |
|    type     | integer  |         类型，正向合约/反向合约          |
|  leverages  |  array   |                 杠杆列表                 |
|    name     |  string  |                市场交易对                |
|    stock    |  string  |             标的资产，如BTC              |
|    money    |  string  |             计价资产，如USDT             |
|  fee_prec   | integer  |                手续费精度                |
|  tick_size  |  string  |               价格最小变动               |
| stock_prec  | integer  |               标的资产精度               |
| money_prec  | integer  |               计价资产精度               |
| amount_prec | integer  |                 数量精度                 |
| amount_min  |  string  |               最小下单数量               |
|  available  |   bool   |                 是否可用                 |
|   limits    |  array   | 持仓限制, [最大持仓数量，杠杆，保证金率] |
|  limits.0   |  array   |                 持仓限制                 |
| limits.0.0  |  string  |               最大持仓数量               |
| limits.0.1  |  string  |                   杠杆                   |
| limits.0.2  |  string  |                 保证金率                 |
|    sort     | integer  |                   排序                   |
|  maker_fee  |  string  |                挂单手续费                |
|  taker_fee  |  string  |                吃单手续费                |

<h2 id="get-market-transactions">获取市场成交</h2>

```shell
curl "https://api.bitda.com/open/api/v2/market/deals"
```

### HTTP请求

- GET `/open/api/v2/market/deals`

### 请求参数

| 参数名 | 参数类型 | 是否必须 |        描述         |
| :----: | :------: | :------: | :-----------------: |
| market |  string  |    是    | ETHUSDT, 市场交易对 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "id": 27699216,
      "price": "1573.89",
      "volume": "0.922",
      "type": "buy",
      "time": 1697619536123
    }
  ]
}
```

### 返回字段

| 参数名 | 参数类型 |      描述      |
| :----: | :------: | :------------: |
|   id   | integer  |     成交ID     |
| price  |  string  |      价格      |
| volume |  string  |      数量      |
|  type  |  string  | 类型，buy/sell |
|  time  | integer  |   时间，毫秒   |

<h2 id="get-market-depth">获取市场深度</h2>

```shell
curl "https://api.bitda.com/open/api/v2/market/depth"

```

### HTTP请求

- GET `/open/api/v2/market/depth`

<aside class="Notice">限速0.1r/s</aside>

### 请求参数

| 参数名 | 参数类型 | 是否必须 |          描述           |
| :----: | :------: | :------: | :---------------------: |
| market |  string  |    是    |  市场交易对: BTC-USDT   |
| merge  |  string  |    否    | 合并深度，0，0.01，0.02 |

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
      {
        "price": "1621.22",
        "volume": "30.613"
      }
    ],
    "bids": [
      {
        "price": "1573.84",
        "volume": "0.819"
      }
    ]
  }
}
```

### 返回字段

|     参数名     | 参数类型 |             描述              |
| :------------: | :------: | :---------------------------: |
|  index_price   |  string  |           指数价格            |
|   sign_price   |  string  |           标记价格            |
|      time      | integer  |             毫秒              |
|      last      |  string  |         最新成交价格          |
|      asks      |  array   |             卖盘              |
|     asks.0     |  object  | [{price 价格, quantity 数量}] |
|  asks.0.price  |  string  |             价格              |
| asks.0.volume  |  string  |             数量              |
|      bids      |  array   |             买盘              |
|    array.0     |  object  | [{price 价格, quantity 数量}] |
| array.0.price  |  string  |             价格              |
| array.0.volume |  string  |             数量              |

<h2 id="get-market-status">获取市场状态</h2>

```shell
curl "https://api.bitda.com/open/api/v2/market/state"

```

### HTTP请求
- GET `/open/api/v2/market/state`

<aside class="Notice">限速10r/s</aside>

### 请求参数

| 参数名 | 参数类型 | 是否必须 |    描述    |
| :----: | :------: | :------: | :--------: |
| market |  string  |    是    | 交易对名称 |

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
    "position_volume": "2.100",
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

### 返回字段

|        参数名        | 参数类型 |      描述      |
| :------------------: | :------: | :------------: |
|        market        |  string  |      市场      |
|        amount        |  string  | 数量，单位USDT |
|         high         |  string  |     最高价     |
|         last         |  string  |   最新成交价   |
|         low          |  string  |     最低价     |
|         open         |  string  |     开盘价     |
|        change        |  string  |      涨跌      |
|        period        | integer  |      周期      |
|        volume        |  string  |     成交量     |
|     funding_time     | integer  |  资金费率时间  |
|   position_volume    |  string  |     持仓量     |
|  funding_rate_last   |  string  | 上一次资金费率 |
|  funding_rate_next   |  string  | 下一次资金费率 |
| funding_rate_predict |  string  |  预测资金费率  |
|      insurance       |  string  |    保险基金    |
|      sign_price      |  string  |    标记价格    |
|     index_price      |  string  |    指数价格    |
|      sell_total      |  string  |    卖盘总量    |
|      buy_total       |  string  |    买盘总量    |

<h2 id="get-all-market-status">获取所有市场状态</h2>

```shell
cuel "https://api.bitda.com/open/api/v2/market/state/all"

```

### HTTP请求
- GET `/open/api/v2/market/state/all`

<aside class="Notice">限速10r/s</aside>

### 请求参数
无

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
      "position_volume": "0",
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

### 返回字段

|                参数名                 | 参数类型 |      描述      |
| :-----------------------------------: | :------: | :------------: |
|           data.market_name            |  object  |   市场交易对   |
|        data.market_name.market        |  string  |   市场交易对   |
|        data.market_name.amount        |  string  | 数量，单位USDT |
|         data.market_name.high         |  string  |     最高价     |
|         data.market_name.last         |  string  |   最新成交价   |
|         data.market_name.low          |  string  |     最低价     |
|         data.market_name.open         |  string  |     开盘价     |
|        data.market_name.change        |  string  |      涨跌      |
|        data.market_name.period        | integer  |      周期      |
|        data.market_name.volume        |  string  |     成交量     |
|     data.market_name.funding_time     | integer  |  资金费率时间  |
|   data.market_name.position_volume    |  string  |     持仓量     |
|  data.market_name.funding_rate_last   |  string  | 上一次资金费率 |
|  data.market_name.funding_rate_next   |  string  | 下一次资金费率 |
| data.market_name.funding_rate_predict |  string  |  预测资金费率  |
|      data.market_name.insurance       |  string  |    保险基金    |
|      data.market_name.sign_price      |  string  |    标记价格    |
|     data.market_name.index_price      |  string  |    指数价格    |
|      data.market_name.sell_total      |  string  |    卖盘总量    |
|      data.market_name.buy_total       |  string  |    买盘总量    |

# Open v2鉴权接口

<h2 id="adjust-position-margin">调整持仓保证金</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/margin"

```

### HTTP请求
- POST `/open/api/v2/position/margin`


### 请求参数

|  参数名  | 参数类型 | 是否必须 |                  描述                  |
| :------: | :------: | :------: | :------------------------------------: |
|  market  |  string  |    是    |               市场交易对               |
|   type   | integer  |    是    | 保证金类型: 1 增加保证金, 2 减少保证金 |
| quantity |  string  |    是    |                  数量                  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "position_id": 4750887, 
    "profit_unreal": "9.0806004045", 
    "create_time": 1697619536, 
    "update_time": 1697620824, 
    "mainten_margin": "0.02", 
    "market": "ETHUSDT", 
    "type": 1, 
    "liq_price": "1525.67027360000000000644", 
    "side": 2, 
    "margin_amount": "79.69434400000000000000", 
    "volume": "1.000", 
    "open_margin": "0.05063536967790213741", 
    "close_left": "1.000", 
    "open_price": "1573.88688000000000000000", 
    "mainten_margin_amount": "31.47773760000000000000", 
    "leverage": "20", 
    "adl_sort": 1, 
    "roe": "0.05",
    "margin_ratio": "0.75",
    "stop_loss_price": "1544.000",
    "take_profit_price": "1655.55",
    "stop_loss_price_type": 1,
    "take_profit_price_type": 1
  }
}

```

### 返回字段

|         参数名         | 参数类型 |      描述      |
| :--------------------: | :------: | :------------: |
|      position_id       | integer  |     持仓ID     |
|      create_time       | integer  |    创建时间    |
|      update_time       | integer  |    更新时间    |
|         market         |  string  |      市场      |
|          type          | integer  |      类型      |
|          side          | integer  |      方向      |
|         volume         |  string  |      数量      |
|       close_left       |  string  |  剩余平仓数量  |
|       open_price       |  string  |    开仓价格    |
|      open_margin       |  string  |   开仓保证金   |
|     margin_amount      |  string  |     保证金     |
|        leverage        |  string  |      杠杆      |
|     profit_unreal      |  string  |   未实现盈亏   |
|       liq_price        |  string  |    强平价格    |
|     mainten_margin     |  string  |   维持保证金   |
| mainten_margin_amount  |  string  | 维持保证金金额 |
|        adl_sort        | integer  |      排序      |
|          roe           |  string  |     盈亏比     |
|      margin_ratio      |  string  |    保证金率    |
|    stop_loss_price     |  string  |    止损价格    |
|   take_profit_price    |  string  |    止盈价格    |
|  stop_loss_price_type  | integer  |  止损价格类型  |
| take_profit_price_type | integer  |  止盈价格类型  |

<h2 id="get-user-transaction">获取成交记录</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/deals"

```

### HTTP请求
- GET `/open/api/v2/order/deals`

### 请求参数

|   参数名   | 参数类型 | 是否必须 |    描述    |
| :--------: | :------: | :------: | :--------: |
|   market   |  string  |    是    | 市场交易对 |
| start_time | integer  |    否    |  开始时间  |
|  end_time  | integer  |    否    |  结束时间  |
|    page    | integer  |    是    |    页码    |
| page_size  | integer  |    是    |  每页数量  |

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
        "time": 1697619536, 
        "side": 2, 
        "leverage": "20", 
        "order_id": 1470801730, 
        "role": 2, 
        "volume": "0.922", 
        "price": "1573.89", 
        "deal_fee": "0.7255", 
        "deal_stock": "1451.1265", 
        "filled_type": 1, 
        "trade_type": 1, 
        "deal_profit": "--"
      }
    ], 
    "page": 1, 
    "page_size": 10
  }
}

```

### 返回字段

|         参数名          | 参数类型 |    描述    |
| :---------------------: | :------: | :--------: |
|         records         |  array   |    记录    |
|        records.0        |  object  |            |
| records.0.position_type | integer  |  持仓类型  |
|    records.0.market     |  string  |    市场    |
|     records.0.time      | integer  |    时间    |
|     records.0.side      | integer  |    方向    |
|   records.0.leverage    |  string  |    杠杆    |
|   records.0.order_id    | integer  |   订单ID   |
|     records.0.role      | integer  |    角色    |
|    records.0.volume     |  string  |    数量    |
|     records.0.price     |  string  |    价格    |
|   records.0.deal_fee    |  string  | 成交手续费 |
|  records.0.deal_stock   |  string  |  成交数量  |
|  records.0.filled_type  | integer  |  成交类型  |
|  records.0.trade_type   | integer  |  交易类型  |
|  records.0.deal_profit  |  string  |  成交盈亏  |
|          page           | integer  |    页码    |
|        page_size        | integer  |   页大小   |

<h2 id="get-completed-orders">获取历史委托</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/finished"

```

### HTTP请求
- GET `/open/api/v2/order/finished`

<aside class="Notice">限速1r/s</aside>

### 请求参数

|   参数名   | 参数类型 | 是否必须 |    描述    |
| :--------: | :------: | :------: | :--------: |
|   market   |  string  |    是    | 市场交易对 |
| start_time | integer  |    否    |  开始时间  |
|  end_time  | integer  |    否    |  结束时间  |
|    page    | integer  |    是    |    页码    |
| page_size  | integer  |    是    |  每页数量  |

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
        "volume": "1", 
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
        "take_profit_price": "-",
        "client_oid": "112233",
        "target": 1
      }
    ], 
    "page": 1, 
    "page_size": 10, 
  }
}

```

### 返回字段

|           参数名            | 参数类型 |       描述       |
| :-------------------------: | :------: | :--------------: |
|           records           |  array   |       记录       |
|          records.0          |  object  |                  |
|     records.0.order_id      | integer  |      订单ID      |
|    records.0.position_id    | integer  |      持仓ID      |
|      records.0.market       |  string  |       市场       |
|       records.0.type        | integer  |       类型       |
|       records.0.side        | integer  | 方向，1:卖，2:买 |
|       records.0.left        |  string  |     剩余数量     |
|      records.0.volume       |  string  |       数量       |
|      records.0.filled       |  string  |    已成交数量    |
|     records.0.deal_fee      |  string  |    成交手续费    |
|       records.0.price       |  string  |       价格       |
|     records.0.avg_price     |  string  |     平均价格     |
|    records.0.deal_stock     |  string  |     成交数量     |
|   records.0.position_type   | integer  |     持仓类型     |
|     records.0.leverage      |  string  |       杠杆       |
|    records.0.update_time    |  float   |     更新时间     |
|    records.0.create_time    |  float   |     创建时间     |
|      records.0.status       | integer  |       状态       |
|  records.0.stop_loss_price  |  string  |     止损价格     |
| records.0.take_profit_price |  string  |     止盈价格     |
|    records.0.client_oid     |  string  |   客户端订单ID   |
|      records.0.target       | integer  |       目标       |
|            page             | integer  |       页码       |
|          page_size          | integer  |      页大小      |


<h2 id="market-order">市价下单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/market"

```

### HTTP请求
- POST `/open/api/v2/order/market`

### 请求参数

|         参数名         | 参数类型 | 是否必须 |                      描述                      |
| :--------------------: | :------: | :------: | :--------------------------------------------: |
|         market         |  string  |    是    |                   市场交易对                   |
|          side          | integer  |    是    |                方向，1 卖，2 买                |
|        quantity        |  string  |    是    |                      数量                      |
|       client_oid       |  string  |    否    |              用户自定义 order id               |
|        is_stop         | boolean  |    否    |              是否是止盈单止损单?               |
|    stop_loss_price     |  string  |    否    |                    止损价格                    |
|  stop_loss_price_type  | integer  |    否    |  止损价类型，1 最新价，2 指数价格，3 标记价格  |
|   take_profit_price    |  string  |    否    |                    止盈价格                    |
| take_profit_price_type | integer  |    否    | 止盈价格类型，1 最新价，2 指数价格，3 标记价格 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "order_id": 112233
  }
}
```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<h2 id="cancel-all-orders-in-a-single-market">取消所有委托</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel/all"

```

### HTTP请求
- POST `/open/api/v2/order/cancel/all`

<aside class="Notice">限速0.5r/s</aside>

### 请求参数

| 参数名 | 参数类型 | 是否必须 |    描述    |
| :----: | :------: | :------: | :--------: |
| market |  string  |    是    | 市场交易对 |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": null
}

```

### 返回字段

| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |
|  data  |  object  |       |

<h2 id="get-order-details">获取委托详情</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/detail"
```

### HTTP请求
- GET `/open/api/v2/order/detail`

<aside class="Notice">限速6r/s</aside>

### 请求参数

|  参数名  | 参数类型 | 是否必须 |    描述    |
| :------: | :------: | :------: | :--------: |
|  market  |  string  |    是    | 市场交易对 |
| order_id |  string  |    是    |  委托单ID  |

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
    "volume": "1", 
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
    "client_oid":"36341ddd362363263626",
    "target": 1
  }
}

```

### 返回字段

|      参数名       | 参数类型 |       描述       |
| :---------------: | :------: | :--------------: |
|     order_id      | integer  |      订单ID      |
|    position_id    | integer  |      持仓ID      |
|      market       |  string  |       市场       |
|       type        | integer  |       类型       |
|       side        | integer  | 方向，1:卖，2:买 |
|       left        |  string  |     剩余数量     |
|      volume       |  string  |       数量       |
|      filled       |  string  |    已成交数量    |
|     deal_fee      |  string  |    成交手续费    |
|       price       |  string  |       价格       |
|     avg_price     |  string  |     平均价格     |
|    deal_stock     |  string  |     成交数量     |
|   position_type   | integer  |     持仓类型     |
|     leverage      |  string  |       杠杆       |
|    update_time    |  float   |     更新时间     |
|    create_time    |  float   |     创建时间     |
|      status       | integer  |       状态       |
|  stop_loss_price  |  string  |     止损价格     |
| take_profit_price |  string  |     止盈价格     |
|    client_oid     |  string  |   客户端订单ID   |
|      target       | integer  |       目标       |

<h2 id="batch-cancel-orders-in-a-single-market">批量取消委托</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel/batch"
```

### HTTP请求
- POST `/open/api/v2/order/cancel/batch`

<aside class="Notice">限速6r/s</aside>

### 请求参数

|  参数名   | 参数类型 | 是否必须 |    描述    |
| :-------: | :------: | :------: | :--------: |
|  market   |  string  |    是    | 市场交易对 |
| order_ids |  string  |    是    |  委托单ID  |

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

### 返回字段

|  参数名   | 参数类型 |     描述     |
| :-------: | :------: | :----------: |
| order_ids |  array   | 成功的订单ID |

<h2 id="cancel-order">取消委托</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/cancel"
```

### HTTP请求
- POST `/open/api/v2/order/cancel`

<aside class="Notice">限速6r/s</aside>

### 请求参数

|  参数名  | 参数类型 | 是否必须 |    描述    |
| :------: | :------: | :------: | :--------: |
|  market  |  string  |    是    | 市场交易对 |
| order_id | integer  |    是    |  委托单ID  |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "order_id": 123456
  }
}

```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<h2 id="submit-limit-order">限价下单</h2>

```shell
"https://api.bitda.com/open/api/v2/order/limit"
```

### HTTP请求
- POST `/open/api/v2/order/limit`

### 请求参数

|         参数名         | 参数类型 | 是否必须 |                     描述                     |
| :--------------------: | :------: | :------: | :------------------------------------------: |
|         market         |  string  |    是    |                  市场交易对                  |
|          side          | integer  |    是    |               方向，1 卖，2 买               |
|         price          |  string  |    是    |                     价格                     |
|        quantity        |  string  |    是    |                     数量                     |
|       client_oid       |  string  |    否    |             用户自定义 order id              |
|        is_stop         | boolean  |    否    |             是否是止盈单止损单?              |
|    stop_loss_price     |  string  |    否    |                   止损价格                   |
|  stop_loss_price_type  |  string  |    否    | 止损价类型，1 最新价，2 指数价格，3 标记价格 |
|   take_profit_price    |  string  |    否    |                   止盈价格                   |
| take_profit_price_type |  string  |    否    | 止盈价类型，1 最新价，2 指数价格，3 标记价格 |

> Responds:

```json
{
  "code": 0, 
  "msg": "success", 
  "data": {
    "order_id": 1471038436
  }
}

```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<h2 id="get-the-entrusted-order">获取当前委托</h2>

```shell
"https://api.bitda.com/open/api/v2/order/pending"

```

### HTTP请求
- GET `/open/api/v2/order/pending`

<aside class="Notice">限速6r/s</aside>

### 请求参数

|  参数名   | 参数类型 | 是否必须 |    描述    |
| :-------: | :------: | :------: | :--------: |
|  market   |  string  |    否    | 市场交易对 |
|   page    | integer  |    否    |    页码    |
| page_size | integer  |    否    |  每页数量  |

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
        "volume": "1.000", 
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
        "client_oid":"36341ddd362363263626",
        "target": 0
      }
    ], 
    "page": 1, 
    "page_size": 10,
    "count": 1
  }
}

```

### 返回字段

|           参数名            | 参数类型 |       描述       |
| :-------------------------: | :------: | :--------------: |
|           records           |  array   |       记录       |
|          records.0          |  object  |                  |
|     records.0.order_id      | integer  |      订单ID      |
|    records.0.position_id    | integer  |      持仓ID      |
|      records.0.market       |  string  |       市场       |
|       records.0.type        | integer  |       类型       |
|       records.0.side        | integer  | 方向，1:卖，2:买 |
|       records.0.left        |  string  |     剩余数量     |
|      records.0.volume       |  string  |       数量       |
|      records.0.filled       |  string  |    已成交数量    |
|     records.0.deal_fee      |  string  |    成交手续费    |
|       records.0.price       |  string  |       价格       |
|     records.0.avg_price     |  string  |     平均价格     |
|    records.0.deal_stock     |  string  |     成交数量     |
|   records.0.position_type   |  string  |     持仓类型     |
|     records.0.leverage      |  string  |       杠杆       |
|    records.0.update_time    |  float   |     更新时间     |
|    records.0.create_time    |  float   |     创建时间     |
|      records.0.status       | integer  |       状态       |
|  records.0.stop_loss_price  |  string  |     止损价格     |
| records.0.take_profit_price |  string  |     止盈价格     |
|    records.0.client_oid     |  string  |   客户端订单ID   |
|      records.0.target       | integer  |       目标       |
|            page             | integer  |       页码       |
|          page_size          | integer  |      页大小      |
|            count            | integer  |       总数       |

<h2 id="submit-stop-order">条件单下单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/stop"

```

### HTTP请求
- POST `/open/api/v2/order/stop`

<aside class="Notice">限速5r/s</aside>

### 请求参数

|     参数名      | 参数类型 | 是否必须 |                      描述                      |
| :-------------: | :------: | :------: | :--------------------------------------------: |
|     market      |  string  |    是    |                   市场交易对                   |
|      side       | integer  |    是    |                方向，1 卖，2 买                |
|   order_price   |  string  |    否    |       订单价格，若留空，则按市场价格下单       |
|   stop_price    |  string  |    是    |                 条件单触发价格                 |
|    cut_price    |  string  |    是    |                    止损价格                    |
| stop_price_type | integer  |    是    | 价格触发类型，1 最新价，2 指数价格，3 标记价格 |
|    quantity     |  string  |    是    |                      数量                      |

> Responds:

```json
{"code": 0, "msg": "success", "data": null}

```

### 返回字段

| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |
|  data  |  object  | null  |

<h2 id="cancel-stop-order">取消条件单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/cancel"
```

### HTTP请求
- POST `/open/api/v2/order/stop/cancel`

<aside class="Notice">限速10r/s</aside>

### 请求参数

|  参数名  | 参数类型 | 是否必须 |    描述    |
| :------: | :------: | :------: | :--------: |
|  market  |  string  |    是    | 市场交易对 |
| order_id |  string  |    是    |   订单ID   |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "order_id": 1471202902
  }
}
```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<aside class="Notice">限速1r/s</aside>

<h2 id="cancel-all-stop-orders-for-a-single-market">取消所有条件单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/cancel/all"

```

### HTTP请求
- POST `/open/api/v2/order/stop/cancel/all`

### 请求参数

| 参数名 | 参数类型 | 是否必须 |    描述    |
| :----: | :------: | :------: | :--------: |
| market |  string  |    是    | 市场交易对 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### 返回字段

| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |
|  data  |  object  |       |


<h2 id="get-the-stop-order-in-the-commission">获取当前条件单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/pending"

```

### HTTP请求
- GET `/open/api/v2/order/stop/pending`

### 请求参数

|  参数名   | 参数类型 | 是否必须 |    描述    |
| :-------: | :------: | :------: | :--------: |
|  market   |  string  |    是    | 市场交易对 |
|   page    | integer  |    是    |    页码    |
| page_size | integer  |    是    |  每页数量  |

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

### 返回字段

|            参数名             | 参数类型 |     描述     |
| :---------------------------: | :------: | :----------: |
|            records            |  array   |     记录     |
|           records.0           |  object  |              |
|  records.0.contract_order_id  |  string  |  合约订单ID  |
|      records.0.order_id       | integer  |    订单ID    |
|     records.0.position_id     | integer  |    持仓ID    |
|       records.0.market        |  string  |     市场     |
| records.0.contract_order_type | integer  | 合约订单类型 |
|    records.0.trigger_type     | integer  |   触发类型   |
|    records.0.trigger_price    |  string  |   触发价格   |
|     records.0.order_price     |  string  |   订单价格   |
|        records.0.size         |  string  |     数量     |
|     records.0.order_time      | integer  |   创建时间   |
|     records.0.update_time     | integer  |   更新时间   |
|      records.0.cut_price      |  string  |   止损价格   |
|       records.0.status        | integer  |     状态     |
|        records.0.side         | integer  |     方向     |
|    records.0.position_type    | integer  |   持仓类型   |
|      records.0.leverage       |  string  |     杠杆     |
|     records.0.entrust_id      |  string  |    委托ID    |
|             page              | integer  |     页码     |
|           page_size           | integer  |    页大小    |
|             count             | integer  |     总数     |


<h2 id="get-completed-stop-order">获取历史条件单</h2>

```shell
curl "https://api.bitda.com/open/api/v2/order/stop/finished"
```

### HTTP请求
- GET `/open/api/v2/order/stop/finished`

### 请求参数

|   参数名   | 参数类型 | 是否必须 |    描述    |
| :--------: | :------: | :------: | :--------: |
|   market   |  string  |    是    | 市场交易对 |
| start_time | integer  |    是    |  开始时间  |
| start_time | integer  |    是    |  结束时间  |
|    page    | integer  |    是    |    页码    |
| page_size  | integer  |    是    |  每页数量  |

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

### 返回字段

|            参数名             | 参数类型 |     描述     |
| :---------------------------: | :------: | :----------: |
|            records            |  array   |     记录     |
|           records.0           |  object  |              |
|  records.0.contract_order_id  |  string  |  合约订单ID  |
|      records.0.order_id       | integer  |    订单ID    |
|     records.0.position_id     | integer  |    持仓ID    |
|       records.0.market        |  string  |     市场     |
| records.0.contract_order_type | integer  | 合约订单类型 |
|    records.0.trigger_type     | integer  |   触发类型   |
|    records.0.trigger_price    |  string  |   触发价格   |
|     records.0.order_price     |  string  |   订单价格   |
|        records.0.size         |  string  |     数量     |
|     records.0.order_time      | integer  |   创建时间   |
|     records.0.update_time     | integer  |   更新时间   |
|      records.0.cut_price      |  string  |   止损价格   |
|       records.0.status        | integer  |     状态     |
|        records.0.side         | integer  |     方向     |
|    records.0.position_type    | integer  |   持仓类型   |
|      records.0.leverage       |  string  |     杠杆     |
|     records.0.entrust_id      |  string  |    委托ID    |
|             page              | integer  |     页码     |
|           page_size           | integer  |    页大小    |
|             count             | integer  |     总数     |

<h2 id="adjust-market-opening-leverage-position-mode">调整持仓杠杆和持仓模式</h2>

```shell
curl "https://api.bitda.com/open/api/v2/setting/leverage"
```

### HTTP请求
- POST `/open/api/v2/setting/leverage`

### 请求参数

|    参数名     | 参数类型 | 是否必须 |          描述           |
| :-----------: | :------: | :------: | :---------------------: |
|    market     |  string  |    是    |       市场交易对        |
|   leverage    |  string  |    是    |        杠杆倍数         |
| position_type |  string  |    是    | 持仓类型，1 逐仓 2 全仓 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### 返回字段

| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |
|  data  |  object  |       |

<h2 id="query-the-market-opening-leverage-position-mode">查询持仓杠杆和持仓模式</h2>

```shell
curl "https://api.bitda.com/open/api/v2/setting/leverage"
```

### HTTP请求
- GET `/open/api/v2/setting/leverage`

### 请求参数

| 参数名 | 参数类型 | 是否必须 |    描述    |
| :----: | :------: | :------: | :--------: |
| market |  string  |    是    | 市场交易对 |

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

### 返回字段

|    参数名     | 参数类型 | 描述  |
| :-----------: | :------: | :---: |
|   leverage    |  string  |       |
| position_type | integer  |       |


<h2 id="query-assets">获取资产详情</h2>

```shell
curl "https://api.bitda.com/open/api/v2/asset/query"
```

### HTTP请求
- GET `/open/api/v2/asset/query`

### 请求参数
无

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

### 返回字段

|             参数名             | 参数类型 |    描述    |
| :----------------------------: | :------: | :--------: |
|        data.market_name        |  object  |            |
|   data.market_name.available   |  string  |  可用资产  |
|    data.market_name.frozen     |  string  |  订单冻结  |
|    data.market_name.margin     |  string  | 持仓保证金 |
| data.market_name.balance_total |  string  |   总余额   |
| data.market_name.profit_unreal |  string  | 未实现盈亏 |
|   data.market_name.transfer    |  string  | 可划转金额 |
|     data.market_name.bonus     |  string  |   保证金   |

<h2 id="query-asset-bill">获取资产历史</h2>

```shell
curl "https://api.bitda.com/open/api/v2/asset/history"
```

### HTTP请求
- GET `/open/api/v2/asset/history`

### 请求参数

|   参数名   | 参数类型 | 是否必须 |   描述   |
| :--------: | :------: | :------: | :------: |
|   asset    |  string  |    是    | 资产名称 |
|  business  |  string  |    否    | 业务类型 |
| start_time | integer  |    否    | 开始时间 |
|  end_time  | integer  |    否    | 结束时间 |
|    page    | integer  |    是    |   页码   |
| page_size  | integer  |    是    | 每页数量 |


> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "time": 1693496496.157132,
        "asset": "USDT",
        "business": "Close pnl",
        "change": "-482.541",
      }
    ],
    "page": 1,
    "page_size": 1,
  }
}
```

### 返回字段

|       参数名       | 参数类型 |     描述     |
| :----------------: | :------: | :----------: |
|      records       |  array   |     记录     |
|     records.0      |  object  |              |
|   records.0.time   | integer  | 时间. 单位ms |
|  records.0.asset   |  string  |     资产     |
| records.0.business |  string  |   业务类型   |
|  records.0.change  |  string  |   资产变化   |

<h2 id="user-positions">获取当前持仓</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/pending"
```

### HTTP请求
- GET `/open/api/v2/position/pending`

### 请求参数

| 参数名 | 参数类型 | 是否必须 |    描述    |
| :----: | :------: | :------: | :--------: |
| market |  string  |    否    | 市场交易对 |

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
      "market": "BTCUSDT",
      "type": 1,
      "side": 2,
      "volume": "1.0000",
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
      "margin_ratio": "0.7",
      "stop_loss_price": "-",
      "take_profit_price": "-",
      "stop_loss_price_type": 1,
      "take_profit_price_type": 1
    }
  ]
}
```

### 返回字段

|            参数名             | 参数类型 |      描述      |
| :---------------------------: | :------: | :------------: |
|             data              |  array   |                |
|            data.0             |  object  |                |
|      data.0.position_id       | integer  |     持仓ID     |
|      data.0.create_time       | integer  |    创建时间    |
|      data.0.update_time       | integer  |    更新时间    |
|         data.0.market         |  string  |      市场      |
|          data.0.type          | integer  |      类型      |
|          data.0.side          | integer  |      方向      |
|         data.0.volume         |  string  |      数量      |
|       data.0.close_left       |  string  |  剩余平仓数量  |
|       data.0.open_price       |  string  |    开仓价格    |
|      data.0.open_margin       |  string  |   开仓保证金   |
|     data.0.margin_amount      |  string  |     保证金     |
|        data.0.leverage        |  string  |      杠杆      |
|     data.0.profit_unreal      |  string  |   未实现盈亏   |
|       data.0.liq_price        |  string  |    强平价格    |
|     data.0.mainten_margin     |  string  |   维持保证金   |
| data.0.mainten_margin_amount  |  string  | 维持保证金金额 |
|        data.0.adl_sort        | integer  |      排序      |
|          data.0.roe           |  string  |     盈亏比     |
|      data.0.margin_ratio      |  string  |    保证金率    |
|    data.0.stop_loss_price     |  string  |    止损价格    |
|   data.0.take_profit_price    |  string  |    止盈价格    |
|  data.0.stop_loss_price_type  | integer  |  止损价格类型  |
| data.0.take_profit_price_type | integer  |  止盈价格类型  |

<h2 id="get-adjustable-margin">获取持仓可调整保证金</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/margin"
```

### HTTP请求
- GET `/open/api/v2/position/margin`

### 请求参数

|   参数名    | 参数类型 | 是否必须 |    描述    |
| :---------: | :------: | :------: | :--------: |
| position_id | integer  |    否    |   持仓ID   |
|   market    |  string  |    否    | 市场交易对 |

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

### 返回字段

|        参数名        | 参数类型 |       描述       |
| :------------------: | :------: | :--------------: |
|        amount        |  string  |       数量       |
|      available       |  string  |    可用保证金    |
|       leverage       |  string  |       杠杆       |
|    margin_amount     |  string  |      保证金      |
| max_removable_margin |  string  | 最大可移除保证金 |

<h2 id="limit-close">限价平仓</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/close/limit"
```

### HTTP请求
- POST `/open/api/v2/position/close/limit`

### 请求参数

|   参数名    | 参数类型 | 是否必须 |     描述     |
| :---------: | :------: | :------: | :----------: |
|   market    |  string  |    是    |  市场交易对  |
| position_id | integer  |    是    |    持仓ID    |
|  quantity   |  string  |    是    |   平仓数量   |
|    price    |  string  |    是    |   平仓价格   |
| client_oid  |  string  |    否    | 客户端订单ID |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "order_id": 325235235
  }
}
```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<h2 id="market-close">市价平仓</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/close/market"
```

### HTTP请求
- POST `/open/api/v2/position/close/market`

### 请求参数

|   参数名    | 参数类型 | 是否必须 |     描述     |
| :---------: | :------: | :------: | :----------: |
|   market    |  string  |    是    |  市场交易对  |
| position_id | integer  |    是    |    持仓ID    |
|  quantity   |  string  |    否    |              |
| client_oid  |  string  |    否    | 用户自定义ID |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "order_id": 235235325
  }
}
```

### 返回字段

|  参数名  | 参数类型 |  描述  |
| :------: | :------: | :----: |
| order_id | integer  | 订单ID |

<h2 id="position-take-profit-and-stop-loss-settingmodification">持仓止盈止损</h2>

```shell
curl "https://api.bitda.com/open/api/v2/position/close/stop"
```

### HTTP请求
- POST `/open/api/v2/position/close/stop`

### 请求参数

|         参数名         | 参数类型 | 是否必须 |     描述     |
| :--------------------: | :------: | :------: | :----------: |
|         market         |  string  |    是    |  市场交易对  |
|      position_id       | integer  |    是    |    持仓ID    |
|    stop_loss_price     |  string  |    否    |   止损价格   |
|  stop_loss_price_type  | integer  |    否    | 止损价格类型 |
|   take_profit_price    |  string  |    否    |   止盈价格   |
| take_profit_price_type | integer  |    否    | 止盈价格类型 |

> Responds:

```json
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

### 返回字段

| 参数名 | 参数类型 | 描述  |
| :----: | :------: | :---: |
|  data  |  object  |       |


# 接口示例

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

# Websocket示例

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


