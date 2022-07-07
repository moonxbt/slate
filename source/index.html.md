---
atitle: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://www.moonxbt.com'>MoonXBT</a>

includes:
  - errors

search: true
---

# Change Log


## 2022-5-30

1st release of API document.

Modify the interface adapted to moonxbt.

# General Info


## General API Information

- Some endpoints will require an API Key. 
- The base endpoint is: https://v2api.moonxbt.com
- All endpoints return either a JSON object or array.
- Data is returned in ascending order. Oldest first, newest last.
- All time and timestamp related fields are in milliseconds.


## HTTP Return Codes

- HTTP 200 OK
- HTTP 4XX return codes are used for malformed requests; the issue is on the sender's side.
- HTTP 5XX return codes are used for internal errors; the issue is on MoonXBT's side.

## General Information on Endpoints
- For POST endpoints, the parameters may be sent as a query string or in the request body. You may mix parameters between both the query string and request body if you wish to do so.
- Parameters may be sent in any order.
- If a parameter sent in both the query string and request body, the query string parameter will be used.

## Authentication

**Overview**

The API request may be tampered during internet, therefore all private API must be signed by your API Key (Secrete Key).

Each API Key has permission property, please check the API permission, and make sure your API key has proper permission.

A valid request consists of below parts:

- API Path: for example https://v2api.moonxbt.com
- API Access Key: The 'Access Key' in your API Key
- Signature Method: The Hash method that is used to sign, it uses HmacSHA256
- Signature Version: The version for the signature protocol, it uses 1
- Timestamp: The UTC time when the request is sent, e.g. 2017-05-11T16:22:06. It is useful to prevent the request to be intercepted by third-party.
- For POST request, the parameters needn't be signed and they should be put in request body.
- Signature: The value after signed, it is guarantee the signature is valid and the request is not be tampered.

**Signature Method**

The signature may be different if the request text is different, therefore the request should be normalized before signing. Below signing steps take the order query as an example:

This is a full URL to query one order:

`https://v2api.moonxbt.com/api/endpoint?`

`AccessKey=73915b6e-e41d-43ff-ae18-0c29df541b43`

`&Signature=fN5ViyRfXY%2FlheM5rGmChIIsvzRD93hXYiUO6BFG8AY%3D`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=10`

`&Timestamp=2022-05-28T08:30:50`

**Http Request Body**:

```json
{ "id": 3, "method": "spotsKline:meta", "jsonrpc": "2.0", "params": { } }
```

# Rest Api

 The endpoint is: https://v2api.moonxbt.com/api/endpoint

API Key Permission：Need, and please refer to the Authentication chapter.

The signature info can be get by Query

## Spots

### K-line Spots Endpoint

####   Meta

Get detailed market trading info about the trading symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{

    }
}
```



| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

## 

**Response Body**:

```json
{
    "result":{
        "spotsSymbols":[
            {
                "supportMarginTrade":true,
                "hidden":false,
                "displayOrder":0,
                "derivative":false,
                "baseMinimumIncrement":0.00001,
                "quoteScale":2,
                "orderBookAccuracy":"0,0.1,0.01",
                "alwaysChargeQuote":true,
                "baseScale":5,
                "zone":"MAIN",
                "quoteMinimumIncrement":0.01,
                "name":"BTC_USDT",
                "baseMaximumQuantity":10000,
                "baseMinimumQuantity":0.00001,
                "id":100105,
                "endTime":4083840000000,
                "openTime":1648818928000,
                "baseName":"BTC",
                "quoteName":"USDT"
            }
        ],
        "spotsCurrencies":[
            "BTC",
            "ETH",
            "USDT"
        ],
        "currencies":[
            {
                "hidden":false,
                "depositOpenTime":0,
                "name":"BNB",
                "displayOrder":0,
                "derivative":false,
                "id":115,
                "iconUrl":"https://moole-verify-img-bucket-dev.s3.ap-southeast-1.amazonaws.com/operation/upload/admin/299924555940761600.png",
                "withdrawOpenTime":0,
                "displayScale":8
            },
            {
                "hidden":false,
                "depositOpenTime":0,
                "name":"USDT",
                "displayOrder":3,
                "derivative":false,
                "id":105,
                "iconUrl":"https://moole-verify-img-bucket-dev.s3.ap-southeast-1.amazonaws.com/operation/upload/jinhaiyun/295202182851203072.png",
                "withdrawOpenTime":0,
                "displayScale":8
            }
        ]
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

#### SpotsList

Get spots trading pair info about price, volume, symbol name.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

## 

**Response Body**:

```json
{
    "result":[
        {
            "volume":"0",
            "symbolId":100105,
            "price":0,
            "name":"BTC_USDT",
            "changes":0
        },
        {
            "volume":"0",
            "symbolId":101105,
            "price":0,
            "name":"ETH_USDT",
            "changes":0
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```
| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### Ticker

Get kline info around recent 24h on the fixed symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```



| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

## 

**Response Body**:

```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
            1653557820000,
            null,
            null,
            null,
            null,
            0,
            0
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

About data: 

[timestamp, open, high, low, close, amount, change]

#### AllTicker

The summary of the K-line info, and the frequency is less than 10op/s

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:allTicker",
    "jsonrpc": "2.0",
    "params": {
    }
}
```



**Response Body**:

```json
{
    "result": [
        {
            "symbol": "BTC_USDT",
            "data": [
                1.65355782E+12,
                null,
                null,
                null,
                null,
                0,
                0
            ],
            "type": "TICKER",
            "sequenceId": "920557",
            "ts": "1652163323227"
        }
    ],
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### OrderBook

Get the order book

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:bars",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
    }
}
```



**Response Body**:

```json
{
    "result": {
        "price": 40207.67,
        "sellOrders": [],
        "buyOrders": [],
        "sequenceId": "157"
    },
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### Bars

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:bars",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "type":"MIN15",
        "start":0,
        "end":0,
        "limit":5
    }
}
```

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| symbol | name of the symbol                       | String    |
| type   | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |



**Response Body**:

```json
{
    "result": [

        [
            1.6520895E+12,
            36476.46,
            36476.46,
            36476.42,
            36476.42,
            0.00002
        ],
        [
            1.6520913E+12,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ],
        [
            1.6520967E+12,
            39999.99,
            39999.99,
            39999.99,
            39999.99,
            0.01
        ],
        [
            1.6521516E+12,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00001
        ],
        [
            1.6521633E+12,
            4E+4,
            4E+4,
            4E+4,
            4E+4,
            0.0005
        ]
    ],
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

| Field | Description        | Data Type |
| ----- | ------------------ | --------- |
| start | Timestamp of start | long      |
| End   | Timestamp of end   | long      |
| limit | The bar amount     | long      |



#### Ticks

Get the recent ticks info

**Request Path**: `POST /api/endpoint`

**Request Body**:

| Field  | Description         | Data Type |
| ------ | ------------------- | --------- |
| Symbol | The symbol name     | String    |
| limit  | The amount of ticks | Limit     |



```json
{
    "id": 3,
    "method": "spotsKline:ticks",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "limit": 1,
        "sequenceId": 0
    }
}
```



**Response Body**:

```json
{
    "result": [
        {
            "data": [
                1651195258029,
                0,
                39885.68,
                0.00032,
                0
            ],
            "sequenceId": 3169352
        }
    ],
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### OrderChanges



**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:orderChanges",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```



**Response Body**:

```json
{
    "result": [],
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

#### OrderMatches



**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```



**Response Body**:

```json
{
    "result": [],
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### Ping

Send ping to check  the service whether available

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "ts": 1653537731930
    }
}
```



**Response Body**:

```json
{
    "result": {
        "gap": 293585010,
        "type": "PONG",
        "ts": 1653831316940
    },
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



### Spots Trade Endpoint

####  Batch Open

Batch to open orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 5,
    "method": "spots:batch",
    "jsonrpc": "2.0",
    "params": {
        "orders": [
            {
                "direction": "LONG",
                "type": "LIMIT",
                "source": "WEB",
                "symbol": "BTC_USDT",
                "quantity": 0.01,
                "postOnly": false,
                "hidden": false,
                "price": 40000,
                "fillOrKill": false,
                "immediateOrCancel": false
            }
        ]
    }
}
```

**Response Body**:

```json
{
    "result": {
        "total": 1,
        "success": 1,
        "failure": 0
    },
    "id": 5,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

####  Batch Cancel

Batch to cancel orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":5,
    "method":"spots:batchCancel",
    "jsonrpc":"2.0",
    "params":{
        "orderIds":[
            283132203,
            283142203
        ]
    }
}
```

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":3,
    "result":{
        "code":0
    }
}
```

#### Open



Get the open state orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 6,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false
    }
}
```

**Response Body**:

```json
{
    "result": {
        "account": {
            "BTC": {
                "usdtPrice": null,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "MATIC": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "BNB": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "XRP": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "ETH": {
                "usdtPrice": null,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "DOGE": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "USDT": {
                "usdtPrice": 0,
                "available": 999199.2,
                "frozen": 800.8,
                "debt": 0
            },
            "USDC": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "TRX": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "LUNA": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            }
        },
        "order": [
            {
                "symbol": "BTC_USDT",
                "quantity": 0.01,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "trailingDistance": 0,
                "clientOrderId": "319884411512426496",
                "fee": 0,
                "marginTrade": false,
                "chargeQuote": true,
                "trailingBasePrice": 0,
                "type": "LIMIT",
                "fillPrice": 0.0,
                "triggerDirection": "LONG",
                "features": 0,
                "createdAt": 1654074386869,
                "trailing": false,
                "unfilledQuantity": 0.01,
                "price": 4E+4,
                "takerFeeRate": 0.002,
                "id": "9870812206",
                "status": "PENDING",
                "direction": "LONG",
                "updatedAt": 1654074386869
            },
            {
                "symbol": "BTC_USDT",
                "quantity": 0.01,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "trailingDistance": 0,
                "clientOrderId": "319866698551398400",
                "fee": 0,
                "marginTrade": false,
                "chargeQuote": true,
                "trailingBasePrice": 0,
                "type": "LIMIT",
                "fillPrice": 0.0,
                "triggerDirection": "LONG",
                "features": 0,
                "createdAt": 1654070163770,
                "trailing": false,
                "unfilledQuantity": 0.01,
                "price": 4E+4,
                "takerFeeRate": 0.002,
                "id": "9412312206",
                "status": "PENDING",
                "direction": "LONG",
                "updatedAt": 1654070163770
            }
        ]
    },
    "id": 6,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

#### Open Symbol 

Get the trade order by symbol

**Request Path**: `POST /api/endpoint`

Get the tradable symbols

**Request Body**:

```json
{
    "id": 6,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false,
        "symbolName": "BTC_USDT"
    }
}
```

**Response Body**:

```json
{
    "result": {
        "account": {
            "BTC": {
                "usdtPrice": null,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "MATIC": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "BNB": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "XRP": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "ETH": {
                "usdtPrice": null,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "DOGE": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "USDT": {
                "usdtPrice": 0,
                "available": 999199.2,
                "frozen": 800.8,
                "debt": 0
            },
            "USDC": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "TRX": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            },
            "LUNA": {
                "usdtPrice": 0,
                "available": 1E+6,
                "frozen": 0,
                "debt": 0
            }
        },
        "order": [
            {
                "symbol": "BTC_USDT",
                "quantity": 0.01,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "trailingDistance": 0,
                "clientOrderId": "319884411512426496",
                "fee": 0,
                "marginTrade": false,
                "chargeQuote": true,
                "trailingBasePrice": 0,
                "type": "LIMIT",
                "fillPrice": 0.0,
                "triggerDirection": "LONG",
                "features": 0,
                "createdAt": 1654074386869,
                "trailing": false,
                "unfilledQuantity": 0.01,
                "price": 4E+4,
                "takerFeeRate": 0.002,
                "id": "9870812206",
                "status": "PENDING",
                "direction": "LONG",
                "updatedAt": 1654074386869
            },
            {
                "symbol": "BTC_USDT",
                "quantity": 0.01,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "trailingDistance": 0,
                "clientOrderId": "319866698551398400",
                "fee": 0,
                "marginTrade": false,
                "chargeQuote": true,
                "trailingBasePrice": 0,
                "type": "LIMIT",
                "fillPrice": 0.0,
                "triggerDirection": "LONG",
                "features": 0,
                "createdAt": 1654070163770,
                "trailing": false,
                "unfilledQuantity": 0.01,
                "price": 4E+4,
                "takerFeeRate": 0.002,
                "id": "9412312206",
                "status": "PENDING",
                "direction": "LONG",
                "updatedAt": 1654070163770
            }
        ]
    },
    "id": 6,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

#### Account

Get account info

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 6,
    "method": "spots:accounts",
    "jsonrpc": "2.0",
    "params": {
       
    }
}
```

**Response Body**:

```json
{
    "result": {
        "BTC": {
            "usdtPrice": null,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "MATIC": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "BNB": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "XRP": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "ETH": {
            "usdtPrice": null,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "DOGE": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "USDT": {
            "usdtPrice": 0,
            "available": 999199.2,
            "frozen": 800.8,
            "debt": 0
        },
        "USDC": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "TRX": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        },
        "LUNA": {
            "usdtPrice": 0,
            "available": 1E+6,
            "frozen": 0,
            "debt": 0
        }
    },
    "id": 6,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### Closed



**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 6,
    "method": "spots:closed",
    "jsonrpc": "2.0",
    "params": {
        "range": "",
        "symbolName": "",
        "offsetId": 0,
        "limit": 100
    }
}
```

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":3,
    "result":{
        "range":"202207",
        "hasMore":true,
        "nextOffsetId":"125559832207",
        "results":[
            {
                "id":"125564892207",
                "clientOrderId":null,
                "features":0,
                "price":0.0000006722,
                "fee":0.0238631,
                "fillPrice":0.0000006722,
                "marginTrade":false,
                "chargeQuote":true,
                "quantity":20000000,
                "unfilledQuantity":0,
                "makerFeeRate":0.001,
                "takerFeeRate":0.002,
                "type":"LIMIT",
                "status":"FULLY_FILLED",
                "direction":"SHORT",
                "triggerDirection":"LONG",
                "triggerOn":0,
                "trailingBasePrice":0,
                "trailingDistance":0,
                "createdAt":1656669215978,
                "updatedAt":1656669772039,
                "symbol":"CTHAI_USDT",
                "trailing":false
            }
        ]
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

#### GetOrder



 Get the order by order id

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 6,
    "method": "spots:getOrder",
    "jsonrpc": "2.0",
    "params": {
        "orderId": 9870812206
    }
}
```

**Response Body**:

```json
{
    "result": {
        "symbol": "BTC_USDT",
        "quantity": 0.01,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": "9870812206",
        "fee": 0,
        "marginTrade": false,
        "chargeQuote": true,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 0.0,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1654074386869,
        "trailing": false,
        "unfilledQuantity": 0.01,
        "price": 4E+4,
        "takerFeeRate": 0.002,
        "id": "9870812206",
        "status": "PENDING",
        "direction": "LONG",
        "updatedAt": 1654074386869
    },
    "id": 6,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



## Liquid Contract

### K-line Liquid Contract Endpoint

#### 

####  History

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

**Reqeust Parameters**：

| Parameter    | Description                      |  Mandatory      |  Data Type  |  Value Range
| ------------ | -------------------------------- |------------------|--------------|---------------|
|kType         | Data range                       | true              |integer  | 1: 1 day, 2: 1 min, 3: 5 mins, 4: 15 mins, 5: 1 hour, 7: 4 hours |
|size          | Fetch size                       | true              |integer  | 1-200 |
|symbol        | Trading symbol (wildcard inacceptable)  | true |string  | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |

**Response Body**:

```json
{
    "result": {
        "code": 0,
        "data": [
            {
                "volume": 1.84740664,
                "amount": 16307.12321434,
                "high": 31861.83,
                "low": 31435.82941177,
                "time": 1653955200,
                "close": 31728.01882353,
                "open": 31734.24235294
            }
        ],
        "time": "2022-05-31 19:05:30",
        "message": "Success",
        "tid": null
    },
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Kline Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |
| tid     | tracer id usd for open tracing           | string    |

**Kline Entity**

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | number                                   |
| high   | High price       | number                                   |
| low    | Low price        | number                                   |
| open   | Open price       | number                                   |
| time   | Market Timestamp | long                                     |



### Liquid Contract Endpoint

#### 

#### Position History



API Key Permission：Read

This endpoint returns a list of historical orders owned by this API user.

**Request Path**: POST /api/endpoint

```json
{
    "id": 5,
    "method": "cfd:historyList",
    "jsonrpc": "2.0",
    "params": {
        "tradeVO": {
            "code": "",
            "direction": "",
            "deviceType": "",
            "currencyName": "",
            "begin": "2020-04-20 18:28:41",
            "end": "2022-05-20 18:28:41"

        }
    }
}
```



**Reqeust Parameters**：

| Parameter    | Description       | Mandatory | Data Type | Value Range                              |
| ------------ | ----------------- | --------- | --------- | ---------------------------------------- |
| code         | Order Code        | false     | string    | -                                        |
| direction    | Order Type        | false     | integer   | 1: up, 2: down                           |
| deviceType   | Device Type       | false     | string    | iOS, Android, Web                        |
| currencyName | Currency Name     | false     | string    | btcusd, eosusd, ltcusd, trxusd, adausd, xrpusd, ethusd, etcusd, bchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |

**Response Body**:

```json
{
    "result": {
        "code": 0,
        "data": [
            {
                "orderType": 0,
                "code": "8241670ea9d64ec7916a2f2535943711",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31788.49230451,
                "holding": true,
                "orderTime": 1653997696203,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 75,
                "simulated": 0,
                "passNightFee": 112.5,
                "currentPrice": null,
                "positions": 2.359344,
                "lever": 1.5E+2,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 31597.76135151,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31788.49230451,
                "settleCharge": 0,
                "targetProfit": 32848.10871451,
                "account": "10023780"
            },
            {
                "orderType": 0,
                "code": "661b4b14d28641fcbb060ad4db266f23",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31792.56705565,
                "holding": true,
                "orderTime": 1653997685192,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 1E+1,
                "simulated": 0,
                "passNightFee": 15,
                "currentPrice": null,
                "positions": 0.314538,
                "lever": 2E+1,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 30361.90153865,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31792.56705565,
                "settleCharge": 0,
                "targetProfit": 39740.70881865,
                "account": "10023780"
            }
        ],
        "time": "2022-05-31 19:55:28",
        "message": "Success",
        "tid": null
    },
    "id": 5,
    "jsonrpc": "2.0"
}
```



#### Position Holdings

API Key Permission：Read

This endpoint returns a list of holding orders owned by this API user.

**Request Path**: POST /api/endpoint

**Request Body**:

```json
{
    "id": 5,
    "method": "cfd:holdingList",
    "jsonrpc": "2.0",
    "params": {
        "tradeVO": {
            "code": "",
            "direction": "",
            "deviceType": "",
            "currencyName": "",
            "begin": "2020-04-20 18:28:41",
            "end": "2022-06-20 18:28:41"
        }
    }
}
```



**Reqeust Parameters**：

| Parameter    | Description       | Mandatory | Data Type | Value Range                              |
| ------------ | ----------------- | --------- | --------- | ---------------------------------------- |
| code         | Order Code        | false     | string    | -                                        |
| direction    | Order Type        | false     | integer   | 1: up, 2: down                           |
| deviceType   | Device Type       | false     | string    | iOS, Android, Web                        |
| currencyName | Currency Name     | false     | string    | btcusd, eosusd, ltcusd, trxusd, adausd, xrpusd, ethusd, etcusd, bchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |

**Response Body**:

```json
{
    "result": {
        "code": 0,
        "data": [
            {
                "orderType": 0,
                "code": "8241670ea9d64ec7916a2f2535943711",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31788.49230451,
                "holding": true,
                "orderTime": 1653997696203,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 75,
                "simulated": 0,
                "passNightFee": 112.5,
                "currentPrice": null,
                "positions": 2.359344,
                "lever": 1.5E+2,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 31597.76135151,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31788.49230451,
                "settleCharge": 0,
                "targetProfit": 32848.10871451,
                "account": "10023780"
            },
            {
                "orderType": 0,
                "code": "661b4b14d28641fcbb060ad4db266f23",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31792.56705565,
                "holding": true,
                "orderTime": 1653997685192,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 1E+1,
                "simulated": 0,
                "passNightFee": 15,
                "currentPrice": null,
                "positions": 0.314538,
                "lever": 2E+1,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 30361.90153865,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31792.56705565,
                "settleCharge": 0,
                "targetProfit": 39740.70881865,
                "account": "10023780"
            }
        ],
        "time": "2022-05-31 19:55:28",
        "message": "Success",
        "tid": null
    },
    "id": 5,
    "jsonrpc": "2.0"
}
```



#### Position Pendings



API Key Permission：Read

This endpoint returns a list of pending orders owned by this API user.

**Request Path**: ` Post /api/endpoint`

**Request Body**: 

```json
{
    "id": 5,
    "method": "cfd:pendingList",
    "jsonrpc": "2.0",
    "params": {
        "tradeVO": {
            "code": "",
            "direction": "",
            "deviceType": "",
            "currencyName": "",
            "begin": "2020-04-20 18:28:41",
            "end": "2022-05-20 18:28:41"
        }
    }
}
```



**Reqeust Parameters**：

| Parameter    | Description       | Mandatory | Data Type | Value Range                              |
| ------------ | ----------------- | --------- | --------- | ---------------------------------------- |
| code         | Order Code        | false     | string    | -                                        |
| direction    | Order Type        | false     | integer   | 1: up, 2: down                           |
| deviceType   | Device Type       | false     | string    | iOS, Android, Web                        |
| currencyName | Currency Name     | false     | string    | btcusd, eosusd, ltcusd, trxusd, adausd, xrpusd, ethusd, etcusd, bchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |

**Response Body**:

```json
{
    "result": {
        "code": 0,
        "data": [
            {
                "orderType": 0,
                "code": "8241670ea9d64ec7916a2f2535943711",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31788.49230451,
                "holding": true,
                "orderTime": 1653997696203,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 75,
                "simulated": 0,
                "passNightFee": 112.5,
                "currentPrice": null,
                "positions": 2.359344,
                "lever": 1.5E+2,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 31597.76135151,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31788.49230451,
                "settleCharge": 0,
                "targetProfit": 32848.10871451,
                "account": "10023780"
            },
            {
                "orderType": 0,
                "code": "661b4b14d28641fcbb060ad4db266f23",
                "appendCharge": 0,
                "extraData": null,
                "walletType": "USDT",
                "orderStatus": 0,
                "memo": null,
                "type": 1,
                "point": null,
                "settlement": 31792.56705565,
                "holding": true,
                "orderTime": 1653997685192,
                "interest": 0,
                "currency": "btcusdt",
                "profit": 0,
                "direction": 0,
                "pendingTime": null,
                "deviceType": null,
                "normal": true,
                "amount": 5E+2,
                "charge": 1E+1,
                "simulated": 0,
                "passNightFee": 15,
                "currentPrice": null,
                "positions": 0.314538,
                "lever": 2E+1,
                "commissionType": 0,
                "superior": "moonxbt",
                "money": null,
                "nextDeductingOverNightFeeTime": 1654074600000,
                "walletName": "contract_usdt",
                "stopLoss": 30361.90153865,
                "recycleExperienceGold": null,
                "overtime": null,
                "strikePrice": 31792.56705565,
                "settleCharge": 0,
                "targetProfit": 39740.70881865,
                "account": "10023780"
            }
        ],
        "time": "2022-05-31 19:55:28",
        "message": "Success",
        "tid": null
    },
    "id": 5,
    "jsonrpc": "2.0"
}
```



#### Order Entity

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | 1:finish 0:doing 2manually cancel  3.system cancel | number     |
| memo                          | Less than 64kb                           | String     |
| type                          | CFD Order Type                           | integer    |
| point                         | Number point                             | String     |
| settlement                    | Settle price                             | BigDecimal |
| holding                       | Judge the order whether is in position state | Boolean    |
| orderTime                     | Create order time                        | datetime   |
| interest                      | The total swap fee, to pass the midnight | BigDecimal |
| currency                      | Token name: btc,eth                      | String     |
| profit                        | the order profit                         | BigDecimal |
| direction                     | 0: Buy 1:Sell                            | integer    |
| pendingTime                   | Create pending order time                | datetime   |
| deviceType                    | “iOS”,"Android","Web"                    | string     |
| normal                        | true: ordinary order, false: pending order | string     |
| amount                        | The amount to cost to buy other token    | BigDecimal |
| charge                        | Trading fee                              | BigDecimal |
| simulated                     | 0:not simulated, 1:simulated             | number     |
| passNightFee                  | The swap fee, to pass this midnight (per day) | BigDecimal |
| currentPrice                  | Current price                            | BigDecimal |
| positions                     | The position for the repsent             | BigDecimal |
| lever                         | level number: for example 100            | BigDecimal |
| commissionType                | 0:not feedback the commission,1:feedback the commission by  Point | number     |
| superior                      | Superior account, the present linked to the superior account. | String     |
| money                         | the usdt amount to create order          | BigDecimal |
| nextDeductingOverNightFeeTime | time to charge the night fee             | Datetime   |
| walletName                    | “USDT”                                   | String     |
| stopLoss                      | Stop loss                                | BigDecimal |
| recycleExperienceGold         | Recycle the experience money             | BigDecimal |
| overtime                      | The order closing time                   | Datetime   |
| strikePrice                   | Strike price                             | BigDecimal |
| settleCharge                  | The charge fee to close the order        | BigDecimal |
| targetProfit                  | The taken profit                         | BigDecimal |
| account                       | Registered account                       | String     |

# WebSocket Api

**WebSocket Path**: wss://v2api.moonxbt.com/ws/endpoint



API Key Permission：Need, and please refer to the Authentication chapter.

The signature info can be get by Query

This WebSocket returns a list of K-lines history data for all public users.

<aside class="notice">
The WebSocket server will check the idle connections every 60 seconds. If the WebSocket server does not receive any request frame from the connection within a 1 minute period, the connection will be disconnected. Unsolicited request frames are not allowed.
</aside>

## Spots

### K-line Spots Endpoint

#### Meta

Get detailed market trading info about the trading symbol

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{

    }
}
```



| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

##  

**Response Body**:

```json
{
    "result":{
        "spotsSymbols":[
            {
                "supportMarginTrade":true,
                "hidden":false,
                "displayOrder":0,
                "derivative":false,
                "baseMinimumIncrement":0.00001,
                "quoteScale":2,
                "orderBookAccuracy":"0,0.1,0.01",
                "alwaysChargeQuote":true,
                "baseScale":5,
                "zone":"MAIN",
                "quoteMinimumIncrement":0.01,
                "name":"BTC_USDT",
                "baseMaximumQuantity":10000,
                "baseMinimumQuantity":0.00001,
                "id":100105,
                "endTime":4083840000000,
                "openTime":1648818928000,
                "baseName":"BTC",
                "quoteName":"USDT"
            }
        ],
        "spotsCurrencies":[
            "BTC",
            "ETH",
            "USDT"
        ],
        "currencies":[
            {
                "hidden":false,
                "depositOpenTime":0,
                "name":"BNB",
                "displayOrder":0,
                "derivative":false,
                "id":115,
                "iconUrl":"https://moole-verify-img-bucket-dev.s3.ap-southeast-1.amazonaws.com/operation/upload/admin/299924555940761600.png",
                "withdrawOpenTime":0,
                "displayScale":8
            },
            {
                "hidden":false,
                "depositOpenTime":0,
                "name":"USDT",
                "displayOrder":3,
                "derivative":false,
                "id":105,
                "iconUrl":"https://moole-verify-img-bucket-dev.s3.ap-southeast-1.amazonaws.com/operation/upload/jinhaiyun/295202182851203072.png",
                "withdrawOpenTime":0,
                "displayScale":8
            }
        ]
    },
    "id":3,
    "jsonrpc":"2.0"
}
```



####  SpotsList

Get spots trading pair info about price, volume, symbol name.

**Request Body**:

```json
{
    "id": 3,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

##   

**Response Body**:

```json
{
    "result":[
        {
            "volume":"0",
            "symbolId":100105,
            "price":0,
            "name":"BTC_USDT",
            "changes":0
        },
        {
            "volume":"0",
            "symbolId":101105,
            "price":0,
            "name":"ETH_USDT",
            "changes":0
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### Ticker

Get kline info around recent 24h on the fixed symbol

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```



| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

**Response Body**:

```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
            1653557820000,
            null,
            null,
            null,
            null,
            0,
            0
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### AllTicker

The summary of the K-line info, and the frequency is less than 10op/s

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```



**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
            "data":[
                1653557820000,
                null,
                null,
                null,
                null,
                0,
                0
            ],
            "type":"TICKER",
            "sequenceId":"920557",
            "ts":"1652163323227"
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### OrderBook

Get the order book

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```



**Response Body**:

```json
{
    "result":{
        "price":40207.67,
        "sellOrders":[

        ],
        "buyOrders":[

        ],
        "sequenceId":"157"
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### Bars

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:bars",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "type":"MIN15",
        "start":0,
        "end":0,
        "limit":5
    }
}
```

**Request Body**:

```json
{
    "result":[
        
        [
            1652089500000,
            36476.46,
            36476.46,
            36476.42,
            36476.42,
            0.00002
        ],
        [
            1652091300000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ],
        [
            1652096700000,
            39999.99,
            39999.99,
            39999.99,
            39999.99,
            0.01
        ],
        [
            1652151600000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00001
        ],
        [
            1652163300000,
            40000,
            40000,
            40000,
            40000,
            0.0005
        ]
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |



#### Ticks

Get the recent ticks info

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:ticks",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "limit":1,
        "sequenceId":0
    }
}
```



**Response Body**:

```json
{
    "result":[
        {
            "data":[
                1651195258029,
                0,
                39885.68,
                0.00032,
                0
            ],
            "sequenceId":3169352
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### OrderChanges



**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:orderChanges",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```



**Response Body**:

```json
{
    "result":[

    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### OrderMatches



**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:orderMatches",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```



**Response Body**:

```json
{
    "result":[

    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array     |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |



#### Ping

Send ping to check  the service whether available

**Request Body**:

```json
{
    "id":3,
    "method":"spotsKline:ping",
    "jsonrpc":"2.0",
    "params":{
        "ts":1653537731930
    }
}
```



**Response Body**:

```json
{
    "result":{
        "gap":293585010,
        "type":"PONG",
        "ts":1653831316940
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |





## Liquid Contract

###  K-Line WebSocket streams

#### History

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 3,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

**Reqeust Parameters**：

| Parameter    | Description                      |  Mandatory      |  Data Type  |  Value Range
| ------------ | -------------------------------- |------------------|--------------|---------------|
|kType         | Data range                       | true              |integer  | 1: 1 day, 2: 1 min, 3: 5 mins, 4: 15 mins, 5: 1 hour, 7: 4 hours |
|size          | Fetch size                       | true              |integer  | 1-200 |
|symbol        | Trading symbol (wildcard inacceptable)  | true |string  | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |



**Response Body**:

```json
{
    "result": {
        "code": 0,
        "data": [
            {
                "volume": 1.84740664,
                "amount": 16307.12321434,
                "high": 31861.83,
                "low": 31435.82941177,
                "time": 1653955200,
                "close": 31728.01882353,
                "open": 31734.24235294
            }
        ],
        "time": "2022-05-31 19:05:30",
        "message": "Success",
        "tid": null
    },
    "id": 3,
    "jsonrpc": "2.0"
}
```

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Kline Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |
| tid     | tracer id usd for open tracing           | string    |

**Kline Entity**

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | number                                   |
| high   | High price       | number                                   |
| low    | Low price        | number                                   |
| open   | Open price       | number                                   |
| time   | Market Timestamp | long                                     |





