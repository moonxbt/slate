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

## Spot

### K-line Spots Endpoint

####   Meta

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
"id": 3,
"method": "spotsKline:meta",
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

**HTTP Response Body**:

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

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

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

**HTTP Response Body**:

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

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
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

**HTTP Response Body**:

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

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:allTicker",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

**HTTP Response Body**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:orderBook",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT"
    }
}
```

**HTTP Response Body**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:bars",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "type": "MIN15",
        "start": 0,
        "end": 0,
        "limit": 10
    }
}
```

**HTTP Response Body**:

```json
{
    "result": [
        [
            1.6511913E+12,
            39779.99,
            39938.76,
            39728.83,
            39758.81,
            0.20588
        ],
        [
            1.6511922E+12,
            39728.25,
            39797.81,
            39671.75,
            39707.41,
            0.17639
        ],
        [
            1.6511931E+12,
            39691.75,
            39871.4,
            38459.84,
            39837.81,
            0.18259
        ],
        [
            1.651194E+12,
            39829.73,
            39900.58,
            39779.45,
            39871.43,
            0.17355
        ],
        [
            1.6511949E+12,
            39841.26,
            44091.62,
            39773.36,
            39885.68,
            0.13083
        ],
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

#### Ticks



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
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

**HTTP Response Body**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:orderChanges",
    "jsonrpc": "2.0",
    "params": {
        "account": "10009092",
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

**HTTP Response Body**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "account": "10009092",
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

**HTTP Response Body**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```
{
    "id": 3,
    "method": "spotsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "account": "10009092",
        "ts": 1653537731930
    }
}
```

**HTTP Response Body**:

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

####  Batch



Batch to open orders

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```json
{
    "id": 5,
    "method": "spots:batch",
    "jsonrpc": "2.0",
    "version": "2.0",
    "SignatureVersion": "1",
    "Signature": "ydmJ64eozTxrDE7uIACcsmbI9xyPPqUscAQ/Qp3i8TE=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "73915b6e-e41d-43ff-ae18-0c29df541b43",
    "Timestamp": "2022-05-25T10:20:50",
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

**Response Content**:

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

#### Open



Get the open state orders

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```json
{
    "id": 6,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "SignatureVersion": "1",
    "Signature": "8PUDxt/JJdwNnvjEsrnZKPhBkYK/a44rz9UOhX1Ecyw=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
    "params": {
        "marginTrade": false
    }
}
```

**Response Content**:

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



**HTTP Request**: `POST /api/endpoint`

Get the tradable symbols

**HTTP Request Body**:

```json
{
    "id": 6,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "SignatureVersion": "1",
    "Signature": "8PUDxt/JJdwNnvjEsrnZKPhBkYK/a44rz9UOhX1Ecyw=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
    "params": {
        "marginTrade": false,
        "symbolName": "BTC_USDT"
    }
}
```

**Response Content**:

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

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```json
{
    "id": 6,
    "method": "spots:accounts",
    "jsonrpc": "2.0",
    "SignatureVersion": "1",
    "Signature": "8PUDxt/JJdwNnvjEsrnZKPhBkYK/a44rz9UOhX1Ecyw=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
    "params": {
       
    }
}
```

**Response Content**:

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



**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```json
{
    "id": 6,
    "method": "spots:closed",
    "jsonrpc": "2.0",
    "SignatureVersion": "1",
    "Signature": "8PUDxt/JJdwNnvjEsrnZKPhBkYK/a44rz9UOhX1Ecyw=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
    "params": {
        "account": "10119267",
        "range": "",
        "symbolName": "",
        "offsetId": 0,
        "limit": 100
    }
}
```

**Response Content**:

```json
{
    "result": {
        "hasMore": false,
        "nextOffsetId": null,
        "range": "202206",
        "results": []
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

#### GetOrder



 Get the order by order id

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

```json
{
    "id": 6,
    "method": "spots:getOrder",
    "jsonrpc": "2.0",
    "SignatureVersion": "1",
    "Signature": "8PUDxt/JJdwNnvjEsrnZKPhBkYK/a44rz9UOhX1Ecyw=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
    "params": {
        "orderId": 9870812206
    }
}
```

**Response Content**:

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

**HTTP Request**: `POST /api/endpoint`

**HTTP Request Body**:

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

**Response Content**:

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

**HTTP Request**: POST /api/endpoint

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

**Response Content**:

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

**HTTP Request**: POST /api/endpoint

**HTTP Request**:

```json
{
    "id": 5,
    "method": "cfd:holdingList",
    "jsonrpc": "2.0",
    "version": "2.0",
    "SignatureVersion": "1",
    "Signature": "JE1yKgEE1lJ/k08bpGUIZfC3lB4xDLcxHQWypdMoASQ=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
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

**Response Content**:

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

**HTTP Request**: ` Post /api/endpoint`

**HTTP Request Body**: 

```json
{
    "id": 5,
    "method": "cfd:pendingList",
    "jsonrpc": "2.0",
    "version": "2.0",
    "SignatureVersion": "1",
    "Signature": "JE1yKgEE1lJ/k08bpGUIZfC3lB4xDLcxHQWypdMoASQ=",
    "SignatureMethod": "HmacSHA256",
    "AccessKey": "93e357f4-d826-4ba0-951b-694457d73cb7",
    "Timestamp": "2022-05-27T10:20:50",
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

**Response Content**:

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
| walletType                    | Wallet type: 0:BTC,1: USDT, 2:ETH, 3:Gold | integer    |
| orderStatus                   | 1:finish 0:doing 2manually cancel  3.system cancel | number     |
| memo                          | Less than 64kb                           | String     |
| type                          | CFD Order Type                           | integer    |
| point                         | Number point                             | String     |
| settlement                    | Settle price                             | Float      |
| holding                       | Judge the order whether is in position state | Boolean    |
| orderTime                     | Create order time                        | datetime   |
| interest                      | The total swap fee, to pass the midnight | BigDecimal |
| currency                      | Token name: btc,eth                      | String     |
| profit                        | the order profit                         | BigDecimal |
| direction                     | 0: Buy 1:Sell                            | integer    |
| pendingTime                   | Create pending order time                | datetime   |
| deviceType                    | iOS\|Android\|Web                        | string     |
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
| walletName                    | BTC, USDT, ETH, Gold                     | String     |
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

This WebSocket returns a list of K-lines history data for all public users.



<aside class="notice">
The WebSocket server will check the idle connections every 60 seconds. If the WebSocket server does not receive any request frame from the connection within a 1 minute period, the connection will be disconnected. Unsolicited request frames are not allowed.
</aside>

## Spots

### K-line Spots Endpoint

#### Meta



**HTTP Request Body**:

```
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

**HTTP Response Body**:

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



**HTTP Request Body**:

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

**HTTP Response Body**:

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



**HTTP Request Body**:

```
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

**HTTP Response Body**:

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



**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```

**HTTP Response Body**:

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



**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```

**HTTP Response Body**:

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



**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:bars",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "type":"MIN15",
        "start":0,
        "end":0,
        "limit":10
    }
}
```

**HTTP Response Body**:

```json
{
    "result":[
        [
            1651191300000,
            39779.99,
            39938.76,
            39728.83,
            39758.81,
            0.20588
        ],
        [
            1651192200000,
            39728.25,
            39797.81,
            39671.75,
            39707.41,
            0.17639
        ],
        [
            1651193100000,
            39691.75,
            39871.4,
            38459.84,
            39837.81,
            0.18259
        ],
        [
            1651194000000,
            39829.73,
            39900.58,
            39779.45,
            39871.43,
            0.17355
        ],
        [
            1651194900000,
            39841.26,
            44091.62,
            39773.36,
            39885.68,
            0.13083
        ],
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



**HTTP Request Body**:

```
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

**HTTP Response Body**:

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



**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:orderChanges",
    "jsonrpc":"2.0",
    "params":{
        "account":"10009092",
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```

**HTTP Response Body**:

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



**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:orderMatches",
    "jsonrpc":"2.0",
    "params":{
        "account":"10009092",
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```

**HTTP Response Body**:

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

**HTTP Request Body**:

```
{
    "id":3,
    "method":"spotsKline:ping",
    "jsonrpc":"2.0",
    "params":{
        "account":"10009092",
        "ts":1653537731930
    }
}
```

**HTTP Response Body**:

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





