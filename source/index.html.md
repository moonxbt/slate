<<<<<<< Updated upstream
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

## 2022-8-04

Add USDT Perpetual contracts related interfaces.

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

The signature may be different if the request text is different, therefore the request should be normalized and URL encoded before signing. Below we have an example to show how to organize them，currently the first line should be GET, and each line has to be splitted by '\n':

GET <br/>
v2api.moonxbt.com <br />
/api/endpoint <br />
AccessKey=97fde68b-4ca9-4d45-ab09-86821e87080c&SignatureMethod=HmacSHA256&SignatureVersion=1&Timestamp=2022-07-09T07%3A30%3A00

**Http Request Body**:

```json
{ 
    "id": 3, 
    "method": "spotsKline:meta", 
    "jsonrpc": "2.0", 
    "params": { } 
}
```

# Rest Api

The endpoint is: https://v2api.moonxbt.com/api/endpoint

API Key Permission：Need, and please refer to the Authentication chapter.

The signature info can be get by Query

## Spots

### K-line Spots Endpoint

####   Meta
API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":1,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
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
            "BTC", //currency name
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
    "id":1,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| spotsSymbols    | spots symbols info    | Array < SpotsSymbols> |
| spotsCurrencies | spots currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**SpotsSymbols**:

| Field                 | Description                              | Data Type  |
| --------------------- | ---------------------------------------- | ---------- |
| id                    | symbol id                                | Long       |
| name                  | symbol name                              | String     |
| supportMarginTrade    | support margin trade                     | Boolean    |
| hidden                | true: disable, false: un disable         | Boolean    |
| displayOrder          | sort field index                         | Integer    |
| derivative            | derivative                               | Boolean    |
| baseName              | base name                                | String     |
| quoteName             | quote name                               | String     |
| baseScale             | Base currency price precision decimal places | Integer    |
| baseMinimumIncrement  | The minimum change scale of the base currency price | BigDecimal |
| baseMaximumQuantity   | The maximum number of transactions in a single base currency | Integer    |
| baseMinimumQuantity   | The minimum number of transactions in a single base currency | BigDecimal |
| quoteScale            | Denomination currency precision decimal places | Integer    |
| quoteMinimumIncrement | The minimum price change scale of the denominated currency | BigDecimal |
| orderBookAccuracy     | Order Book Accuracy  0,0.1,0.01          | String     |
| alwaysChargeQuote     | Fees are always charged in the currency of denomination | Boolean    |
| zone                  | What regions are allowed to trade        | String     |
| endTime               | end time company：ms                      | Long       |
| openTime              | open time company：ms                     | Long       |


**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |


#### SpotsList

API Key Permission：Read

Get spots trading pair info about price, volume, symbol name.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 2,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
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
    "id":2,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description     | Data Type  |
| -------- | --------------- | ---------- |
| volume   | volume          | String     |
| symbolId | symbol id       | Integer    |
| price    | price           | String     |
| name     | symbol name     | String     |
| changes  | 24h up and down | BigDecimal |



#### Ticker

API Key Permission：Read

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
| Id      | Request id            | Integer   |
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
                1596445560000,
                376.9,
                388.3,
                354.5,
                382.9,
                2196.89,
                67.2
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### AllTicker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 4,
    "method": "spotsKline:allTicker",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
            "data":[
                1657608060000,
                20430.42,
                20704.04,
                19797.62,
                19906.53,
                22.1553,
                -0.025642644644603
            ],
            "type":"TICKER",
            "sequenceId":"28113983",
            "ts":"1657608114573"
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |



#### OrderBook

API Key Permission：Read

Get the order book

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 5,
    "method": "spotsKline:orderBook",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |

**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":5,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**：

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":6,
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        [
            1657599300000,              //timestamp
            19969.28,                   //open
            20023.86,                   //high
            19923.3,                    //low
            20001.42,                   //close
            0.35231                     //amount
        ],
        [
            1657600200000,
            20001.45,
            20036.98,
            19895.45,
            20011.13,
            0.40759
        ],
        [
            1657607400000,
            19888.96,
            19924.63,
            19881.84,
            19906.43,
            0.1317
        ]
    ],
    "id":6,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]


#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 7,
    "method": "spotsKline:ticks",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "limit": 1,
        "sequenceId": 0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |



**Response Body**:

```json
{
    "result":[
        {
            "data":[
                1657608814025,  //timestamp
                1,              //direction 1=buy, 0=sell
                19865.38,       //price
                0.00015,        //amount
                0               //flag 0=ordinary transaction
            ],
            "sequenceId":28131450
        }
    ],
    "id":7,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]



#### OrderChanges

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 8,
    "method": "spotsKline:orderChanges",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |


**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":8,
    "result":[
        {
            "id":281465252207,
            "sequenceId":28146525,
            "userId":10589459,
            "symbolId":100105,
            "type":"LIMIT",
            "status":"FULLY_FILLED",
            "direction":"SHORT",
            "fillPrice":19842.33,
            "price":19842.33,
            "quantity":0.00046,
            "unfilledQuantity":0,
            "makerFeeRate":0.001,
            "takerFeeRate":0.001,
            "fee":0.0091274718,
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "clientOrderId":"2022071215c26165c601b011ed8edc",
            "createAt":1657609421074,
            "updateAt":1657609421074
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | String               |
| jsonrpc | The json-rpc  version | String               |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| sequenceId        | sequence id                              | Long       |
| symbolId          | symbol id                                | Long       |
| type              | order type limit: Limit order, market: market order | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |


[Abort status](#orderStatus)

#### OrderMatches

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 9,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":9,
    "result":[
        {
            "id":112058272207,
            "sequenceId":11205827,
            "userId":10589459,
            "symbolId":100105,
            "orderId":"112057922207",
            "direction":"LONG",
            "price":19145.25,
            "quantity":0.00008,
            "fee":0.00153162,
            "createAt":1656605021615
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Number               |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field      | Description                    | Data Type  |
| ---------- | ------------------------------ | ---------- |
| id         | order id                       | Long       |
| sequenceId | sequence id                    | Long       |
| symbolId   | symbol id                      | Long       |
| orderId    | order id                       | String     |
| direction  | LONG:buy,SHORT:sell            | String     |
| price      | order limit                    | BigDecimal |
| quantity   | quantity of order              | BigDecimal |
| fee        | Total accumulated handling fee | BigDecimal |
| createAt   | create time stamp ms           | Long       |


### Spots Trade Endpoint

####  Create Open Order

API Key Permission：Write

Open one order

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":11,
    "method":"spots:createOrder",
    "jsonrpc":"2.0",
    "params":{
        "order":{
            "direction":"LONG",
            "type":"LIMIT",
            "source":"WEB",
            "symbol":"BTC_USDT",
            "quantity":0.01,
            "postOnly":false,
            "hidden":false,
            "price":22538,
            "fillOrKill":false,
            "immediateOrCancel":false
        }
    }
}
```


| Field             | Type    | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| symbol            | string  | Required , exchage pair to trade, such as `BTC_USDT` |
| type              | enum    | Required order type：limit order="LIMIT"，market order="MARKET" |
| direction         | enum    | Required  order direction：buy="LONG"，sell="SHORT" |
| price             | decimal | Only for Limited Order  order price ，such as`7123.5` |
| quantity          | decimal | Required order amount ,such as`1.02`     |
| fillOrKill        | boolean | Not required  whether to FOK. Order，such as`true` |
| immediateOrCancel | boolean | Not required whether to IOC order，such as`true` |
| postOnly          | boolean | Required  whether negtively to trust orders，such as`true` |
| hidden            | boolean | Required whether to trust iceberg orders，such as `true`，the order fee for iceberg orders is of taker's fee |
| clientOrderId     | string  | Optional customerised OrderId，used to query,canel order, which is available within 24h |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":11,
    "result":{
        "symbol":"BTC_USDT",
        "triggerOn":0,
        "quantity":0.01,
        "makerFeeRate":0.001,
        "trailingDistance":0,
        "fee":0,
        "clientOrderId":null,
        "marginTrade":false,
        "trailingBasePrice":0,
        "type":"LIMIT",
        "fillPrice":0,
        "triggerDirection":"LONG",
        "features":0,
        "createdAt":1657246741388,
        "trailing":false,
        "unfilledQuantity":0.01,
        "price":22538,
        "takerFeeRate":0.001,
        "id":"223193832207",
        "status":"PENDING",
        "direction":"SHORT",
        "updatedAt":1657246741388
    }
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | order quantity                           | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)


####  Batch Cancel

API Key Permission：Write

Batch to cancel orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
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

| Field    | Description           | Data Type      |
| -------- | --------------------- | -------------- |
| id       | The result id         | Integer        |
| Method   | request func          | String         |
| jsonrpc  | The json-rpc  version | string         |
| orderIds | cancel order id       | Array< String> |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0
    }
}
```
| Field   | Description                  | Data Type            |
| ------- | ---------------------------- | -------------------- |
| result  | Return result                | Array< ResultObject> |
| id      | The result id                | Integer              |
| jsonrpc | The json-rpc  version        | String               |
| code    | code 0 success other failure | Integer              |

#### Get Open Orders

API Key Permission：Read

Get the open state orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 13,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false
    }
}
```

| Field       | Description             | Data Type |
| ----------- | ----------------------- | --------- |
| id          | The result id           | Integer   |
| Method      | request func            | String    |
| jsonrpc     | The json-rpc  version   | string    |
| marginTrade | margin trade only false | Boolean   |

**Response Body**:

```json
{
    "result":{
        "account":{
            "BTC":{
                "usdtPrice":19938.53,
                "available":500000000,
                "frozen":0,
                "debt":0
            },
            "ETH":{
                "usdtPrice":1074.83,
                "available":500000000,
                "frozen":0,
                "debt":0
            },
            "USDT":{
                "usdtPrice":0,
                "available":499984985,
                "frozen":15015,
                "debt":0
            }
        },
        "order":[
            {
                "symbol":"BTC_USDT",
                "quantity":0.01,
                "triggerOn":0,
                "makerFeeRate":0.001,
                "trailingDistance":0,
                "clientOrderId":null,
                "fee":0,
                "marginTrade":false,
                "chargeQuote":true,
                "trailingBasePrice":0,
                "type":"LIMIT",
                "fillPrice":0,
                "triggerDirection":"LONG",
                "features":0,
                "createdAt":1657611089461,
                "trailing":false,
                "unfilledQuantity":0.01,
                "price":15000,
                "takerFeeRate":0.001,
                "id":"281893222207",
                "status":"PENDING",
                "direction":"LONG",
                "updatedAt":1657611089461
            }
        ]
    },
    "id":13,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description   | Data Type           |
| ------- | ------------- | ------------------- |
| account | Return result | AccountObject       |
| order   | The result id | Array< OrderObject> |


**AccountObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

**OrderObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | order quantity                           | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)

#### Open Symbol

API Key Permission：Read

Get the trade order by symbol

**Request Path**: `POST /api/endpoint`

Get the tradable symbols

**Request Body**:

```json
{
    "id": 14,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false,
        "symbolName": "BTC_USDT"
    }
}
```

| Field       | Description             | Data Type |
| ----------- | ----------------------- | --------- |
| id          | The result id           | Integer   |
| Method      | request func            | String    |
| jsonrpc     | The json-rpc  version   | string    |
| marginTrade | margin trade only false | Boolean   |
| symbolName  | symbol name             | String    |

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
    "id": 14,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description   | Data Type           |
| ------- | ------------- | ------------------- |
| account | Return result | AccountObject       |
| order   | The result id | Array< OrderObject> |

**AccountObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

**OrderObject**:

| Field            | Description                              | Data Type  |
| ---------------- | ---------------------------------------- | ---------- |
| id               | order id                                 | Long       |
| symbol           | symbol id                                | Long       |
| triggerOn        | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type             | order type limit: Limit order, market: market order | String     |
| marginTrade      | Order feature code                       | Long       |
| features         | Whether it is leveraged trading (currently not open) | String     |
| status           | order status                             | String     |
| direction        | LONG:buy,SHORT:sell                      | String     |
| fillPrice        | Average order price                      | BigDecimal |
| price            | order limit                              | BigDecimal |
| quantity         | quantity of order                        | BigDecimal |
| unfilledQuantity | Number of orders not yet filled          | BigDecimal |
| makerFeeRate     | rate as maker                            | BigDecimal |
| takerFeeRate     | rate as taker                            | BigDecimal |
| fee   | Total accumulated handling fee
| BigDecimal |
| trailingBasePrice   | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique
| String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

[Abort status](#orderStatus)

#### Account

API Key Permission：Read

Get account info

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 15,
    "method": "spots:accounts",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |

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
    "id": 15,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

#### Closed

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 16,
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

| Field      | Description           | Data Type |
| ---------- | --------------------- | --------- |
| id         | The result id         | Integer   |
| Method     | request func          | String    |
| jsonrpc    | The json-rpc  version | string    |
| range      | year month eg: 202207 | String    |
| symbolName | symbol name  version  | String    |
| offsetId   | last order id         | String    |
| limit      | The json-rpc  version | Integer   |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":16,
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

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for stop orders, the trigger price for non-stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | rate as maker                            | BigDecimal |
| takerFeeRate      | rate as taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)

#### GetOrder

API Key Permission：Read

Get the order by order id

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 17,
    "method": "spots:getOrder",
    "jsonrpc": "2.0",
    "params": {
        "orderId": 9870812206
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| orderId | order id              | String    |

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
    "id": 17,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The custom id is globally unique         | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | rate as maker                            | BigDecimal |
| takerFeeRate      | rate as taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique
| String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

<a id = "orderStatus">Abort status</a>

### Order Status
OrderStatus：Order Status Description

| Field             | Description                              |
| ----------------- | ---------------------------------------- |
| STOP_PENDING      | Waiting for the triggered stop order;    |
| PENDING           | Activity orders that are waiting to be filled; |
| FAILED            | Order execution failed (insufficient margin, etc.), the final status; |
| STOP_FAILED       | After the Stop order is triggered, the execution fails (for reasons such as insufficient margin), and the final state; |
| FULLY_FILLED      | All transactions, final status;          |
| PARTIAL_FILLED    | Partially sold;                          |
| PARTIAL_CANCELLED | Waiting for a triggered stop order;      |
| STOP_CANCELLED    | The Stop order is canceled by the user before it is triggered, and the final state; |
| FULLY_CANCELLED   | The order is canceled by the user before it is completed, and the final status; |
| STOP_PENDING      | Waiting for a triggered stop order;      |
| STOP_PENDING      | Waiting for a triggered stop order;      |




## Liquid Contract

### K-line Liquid Contract Endpoint

#### 

####  History

API Key Permission：Read

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 18,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

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
    "id": 18,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description                    | Data Type          |
| ------- | ------------------------------ | ------------------ |
| code    | Return code                    | Integer            |
| data    | Return data                    | Array< DataObject> |
| message | Return message                 | String             |
| time    | Return timestamp               | String             |
| tid     | tracer id usd for open tracing | String             |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                               |
| high   | High price       | BigDecimal                               |
| low    | Low price        | BigDecimal                               |
| open   | Open price       | BigDecimal                               |
| time   | Market Timestamp | long                                     |



### Liquid Contract Endpoint

#### 

### Position History

API Key Permission：Read

This endpoint returns a list of historical orders owned by this API user.

**Request Path**: POST /api/endpoint

```json
{
    "id": 19,
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
    "id": 19,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Position Holdings

API Key Permission：Read

This endpoint returns a list of holding orders owned by this API user.

**Request Path**: POST /api/endpoint

**Request Body**:

```json
{
    "id": 20,
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
    "id": 20,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Position Pendings

API Key Permission：Read

This endpoint returns a list of pending orders owned by this API user.

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
    "id": 21,
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
    "id": 21,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



###  Get Orders

API Key Permission：read

Get order details

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:get",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "tradeVO": {
              "code": "d5c5358530a94e60b28fe515afa25901"
            }
      }
}
```

| Parameter    | Description | Mandatory | Data Type | Value Range                                                 |
| ------------ |-------------|-----------|-----------|-------------------------------------------------------------|
| code         | order code  | true      | String    | -                                                           |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:28:18",
            "code": 0,
            "tid": null,
            "data": {
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
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Pending Order

API Key Permission：Write

create pending orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:pend",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "order": {
                  "pendingPrice": 20000,
                  "symbol": "btcusdt",
                  "leverage": 1,
                  "amount": "2",
                  "direction": 0,
                  "type": 0
            }
      }
}
```

| Parameter    | Description                            | Mandatory | Data Type | Value Range                                                 |
| ------------ |----------------------------------------|-----------|-----------|-------------------------------------------------------------|
| pendingPrice         | pending price                          | true      | Decimal   | -                                                           |
| symbol    | Trading symbol (wildcard inacceptable) | true     | string        | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |
| leverage   | leverage                               | true     | integer   | 1-125                                                       |
| amount        | The amount to cost to buy other token  | true     | Decimal   |                  Decimal                                           |
| direction          | order direction                        | true      | integer   | 0: Buy 1:Sell                                                      |
| type          | Whether to hold the position overnight                         | true     | integer   | 0:order does not stay overnight, 1:order overnight                                                      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:28:18",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "walletName": "contract_usdt",
                  "walletType": "USDT",
                  "orderCode": "9e3144faf99f4e938cbf27284622523e"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| walletName                     | wallet name                  | String   |
| walletType                    | wallet type                  | String    |
| orderCode                   | order code                   | String    |



### Cancel Orders

API Key Permission：Write

cancel orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "cfd:cancel",
  "jsonrpc": "2.0",
  "version": "2.0",
  "params": {
    "code": "9e3144faf99f4e938cbf27284622523e"
  }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range |
| ------------ |---------------------------------------|-----------|-----------|------------|
| code         | order code                            | true      | string   | order code |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:36:56",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Create Orders

API Key Permission：Write

create orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:place",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "order": {
                  "symbol": "btcusdt",
                  "leverage": 1,
                  "amount": "2",
                  "direction": 0,
                  "type": 0
            }
      }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range                                                    |
| ------------ |---------------------------------------|-----------|-----------|----------------------------------------------------------------|
| symbol    | Trading symbol (wildcard inacceptable) | true     | string        | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |
| leverage   | leverage                               | true     | integer   | 1-125                                                          |
| amount        | The amount to cost to buy other token  | true     | Decimal   | Decimal                                                        |
| direction          | order direction                        | true      | integer   | 0: Buy 1:Sell                                                  |
| type          | Whether to hold the position overnight                         | true     | integer   | 0:order does not stay overnight, 1:order overnight             |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:39:23",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "walletName": "contract_usdt",
                  "walletType": "USDT",
                  "orderCode": "01f0a248c8954430a25bad70176561ef"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| walletName                     | wallet name                  | String   |
| walletType                    | wallet type                  | String    |
| orderCode                   | order code                   | String    |



### Settle Orders

API Key Permission：Write

settle orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:settle",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "01f0a248c8954430a25bad70176561ef"
      }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range |
| ------------ |---------------------------------------|-----------|-----------|------------|
| code         | order code                            | true      | string   | order code |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:41:32",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Append Orders

API Key Permission：Write

append orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:append",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "facea80b612a4ca6a1c7c0df5c0375be",
            "appendAmount": 5
      }
}
```

| Parameter    | Description   | Mandatory | Data Type | Value Range |
| ------------ |---------------|-----------|-----------|-------------|
| code         | order code    | true      | string    | order code  |
| appendAmount         | append amount | true      | Decimal        | amount      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 19:20:58",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "appendMarginLog": "{\"account\":\"10119864\",\"appendCharge\":0,\"appendCount\":1,\"appendMargin\":5,\"appendTime\":1657106448875,\"code\":\"facea80b612a4ca6a1c7c0df5c0375be\",\"currency\":\"btcusdt\",\"deviceType\":\"OpenAPI\",\"direction\":0,\"lever\":2.857142,\"stopLoss\":13739.039731,\"strikePrice\":20056.992305,\"targetProfit\":55156.728837,\"walletName\":\"contract_usdt\",\"walletType\":\"USDT\"}"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| appendMarginLog                  | append orders message               | String   |



### Change Over Night Status

API Key Permission：Write

change over night status

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:changeOverNightType",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "3428656c824f43b985f003ee83660c5d",
            "type": 0
      }
}
```

| Parameter    | Description | Mandatory | Data Type | Value Range |
| ------------ |-------------|-----------|-----------|-------------|
| code         | order code  | true      | string    | order code  |
| type         | 1:pass night, 2:no append margin  | true      | integer   | 1 or 2      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:44:07",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Change Target Profit And Stop Loss

API Key Permission：Write

change target profit and stop loss

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:changeTargetProfitAndStopLoss",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "profitAndStopLoss": {
                  "code": "3428656c824f43b985f003ee83660c5d",
                  "targetProfit": 24000,
                  "stopLoss": 19000,
                  "targetProfitRatio": 20,
                  "stopLossRatio": 20
                      
            }
      }
}
```

| Parameter    | Description                                                  | Mandatory | Data Type | Value Range |
| ------------ |--------------------------------------------------------------|-----------|-----------|-------------|
| code         | holding order code                                           | true      | string    | order code  |
| targetProfit         | target Profit. must be used together with stopLossRatio or stopLoss                         | false      | Decimal   | Decimal        |
| stopLoss         | stop loss price. must be used together with targetProfitRatio or targetProfit                         | false      | Decimal   | Decimal        |
| targetProfitRatio         | target profit ratio. must be used together with stopLossRatio or stopLoss                   | false      | Decimal   | 1-100       |
| stopLossRatio         | stop loss ratio. must be used together with targetProfitRatio or targetProfit | false     | Decimal   | 1-100        |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:53:48",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



## USDT Perpetual contracts



### Get Closed Orders

**Meta**

API Key Permission：Read

Get closed orders.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetClosed",
  "jsonrpc": "2.0",
  "params": {
    "range":"",
    "symbol":"",
    "offsetId":0,
    "limit":1
  }
}
```

| Field    | Description                              | Data Type |
| -------- | ---------------------------------------- | --------- |
| Id       | Request id                               | Number    |
| Method   | request func                             | String    |
| jsonrpc  | The json-rpc  version                    | String    |
| range    | Range as int like 201808.                | String    |
| symbol   | Return related symbol-only Default to "" (all symbols). | String    |
| offsetId | From start order id. Default to 0. (latest first). | Number    |
| limit    | Limited number per page                  | Number    |


**Response Body**:

```json
{
  "result": {
    "hasMore": true,
    "nextOffsetId": "12035612208",
    "range": "202208",
    "results": [
      {
        "symbol": "BTC_USDT",
        "quantity": 2,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": "",
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603034843,
        "trailing": false,
        "unfilledQuantity": 0,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035682208",
        "status": "FULLY_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603034843
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| hasMore           | Whether having more data                 | Boolean   |
| nextOffsetId      | Next offset id                           | String    |
| range             | year month eg: 202207                    | String    |
| symbol            | Symbol name                              | String    |
| quantity          | Amount                                   | Number    |
| triggerOn         | The trigger price for stop orders, the trigger price for non-stop orders is always 0 | Decimal   |
| makerFeeRate      | Rate as maker                            | Decimal   |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | Decimal   |
| clientOrderId     | The custom id is globally unique         | String    |
| fee               | Total accumulated handling fee           | Decimal   |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | Decimal   |
| type              | order type limit: Limit order, market: market order | String    |
| fillPrice         | Average order price                      | Decimal   |
| triggerDirection  | LONG:buy,SHORT:sell                      | String    |
| features          | Whether it is leveraged trading (currently not open) | Number    |
| createdAt         | create time stamp                        | Number    |
| trailing          | Whether tracing the SL or TP orders      | Boolean   |
| unfilledQuantity  | Number of orders not yet filled          | Number    |
| price             | Order limit price                        | Decimal   |
| takerFeeRate      | Rate as taker                            | Decimal   |
| id                | Order id                                 | String    |
| status            | Order status                             | String    |
| direction         | LONG:buy,SHORT:sell                      | String    |
| updatedAt         | Update time stamp                        | Number    |



### Get Open Orders

**Meta**

API Key Permission：Read

Get open orders.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpens",
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


**Response Body**:

```json
{
  "result": {
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description | Data Type |
| ------- | ----------- | --------- |
| account | Acount info | Object    |
| order   | Orders info | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |



**order**:

| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Limited Or Market Orders

**Meta**

API Key Permission：Read

To get the Limited or Market orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensNormal",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.16352,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |



**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### SL Or TP Orders

**Meta**

API Key Permission：Read

To Get the SL or TP orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensTrigger",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.16352,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Aggregated Order Position

**Meta**

API Key Permission：Read


Interface used to get aggregated order position

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensData",
  "jsonrpc": "2.0",
  "params": {
    "type":0
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| type    | Type                  | Number    |


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ],
    "positions": [
      {
        "val": 40882,
        "leverage": 0,
        "symbol": "BTC_USDT",
        "margin": 0.0347497,
        "returnRate": 117.65,
        "riskLevel": 0,
        "quantity": 2,
        "maxQuantity": 100000,
        "bankruptcyPrice": 0,
        "minimumMaintenanceMarginRate": 0.005,
        "liquidationPrice": 0,
        "fairPrice": 0,
        "maxLeverage": 100,
        "entryPrice": 20441,
        "realizedPNL": -0.0061323,
        "takerFeeRate": 0.0015,
        "closed": false,
        "id": "10119870_100105",
        "unRealizedPNL": 4.0882,
        "riskLight": 5,
        "updatedAt": 1659603041720,
        "direction": "SHORT"
      }
    ],
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    }
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



**positions**:

| Field                        | Description                              | Data Type |
| ---------------------------- | ---------------------------------------- | --------- |
| val                          | value                                    | Decimal   |
| leverage                     | The leverage                             | Number    |
| symbol                       | The symbol                               | String    |
| margin                       | The margin, should >0                    | Decimal   |
| returnRate                   | Return rate                              | Decimal   |
| riskLevel                    | Risk level for the present account       | Number    |
| quantity                     | The position amount, 0 means closed, need multiply the multiplier in meta | Number    |
| maxQuantity                  | The max quatity under this risk level    | Number    |
| bankruptcyPrice              | Bankruptcy price                         | Decimal   |
| minimumMaintenanceMarginRate | The minimum Maintenance Margin Rate      | Decimal   |
| liquidationPrice             | The liquidation price                    | Decimal   |
| fairPrice                    | The mark price                           | Decimal   |
| maxLeverage                  | The maximum leverage                     | Number    |
| entryPrice                   | Open price                               | Decimal   |
| realizedPNL                  | Realized profit and loss                 | Decimal   |
| takerFeeRate                 | Taker fee rate                           | Decimal   |
| closed                       | Whether closed                           | Boolean   |
| id                           | Position id                              | String    |
| unRealizedPNL                | Unrealized profit and loss               | Decimal   |
| riskLight                    | Risk light,0-no one ligthing, 5- all lighting | Number    |
| updatedAt                    | Updated time                             | Number    |
| direction                    | Direction "SHORT", "LONG"                | String    |



### Orders Info By Symbol

**Meta**

API Key Permission：Read

Get orders by symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetSymbolOpens",
  "jsonrpc": "2.0",
  "params": {
    "symbol":"BTC_USDT"
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | Symbol name           | String    |


**Response Body**:

```json
{
  "result": {
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.061826390000000541222085975110530853271484375,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528000000000034,
        "position": 0.034749700000000007,
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description | Data Type |
| ------- | ----------- | --------- |
| account | Account     | Object    |
| order   | Order list  | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |

**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### Order Info By Order Id

**Meta**

API Key Permission：Read

Get order by id.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetById",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- | --------------------- | --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| encryptOrderId | order id              | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Get Open Order

**Meta**

API Key Permission：Read

Get open order by clientOrderId.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpenByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- |-----------------------| --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| clientOrderId | client order id       | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### Order Info By ClientOrderId

**Meta**

API Key Permission：Read

Get order by clientOrderId.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- |-----------------------| --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| clientOrderId | client order id       | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |





### Position Clearings

**Meta**

API Key Permission：Read

Get position clearings by range.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:positionClearingsGetByRange",
  "jsonrpc": "2.0",
  "params": {
    "range":"",
    "symbol":"BTC_USDT",
    "offsetId":"0",
    "limit":1
  }
}
```

| Field    | Description                              | Data Type |
| -------- | ---------------------------------------- | --------- |
| Id       | Request id                               | Number    |
| Method   | request func                             | String    |
| jsonrpc  | The json-rpc  version                    | String    |
| range    | Range as int like 201808.                | String    |
| symbol   | Return related symbol-only Default to "" (all symbols). | String    |
| offsetId | From start order id. Default to 0. (latest first). | String    |
| limit    | Limited number per page                  | Number    |


**Response Body**:

```json
{
  "result": {
    "hasMore": true,
    "nextOffsetId": "9",
    "range": "202208",
    "results": [
      {
        "symbol": "BTC_USDT",
        "realizedPNLChanged": -0.0061323,
        "orderId": "12035832208",
        "fee": 0.0061323,
        "type": "OPEN",
        "sequenceId": 1203583,
        "quantityChanged": 2,
        "createdAt": 1659603041720,
        "rate": 0.0015,
        "clearingPrice": 20441,
        "quantityAfterClearing": 2,
        "id": 10,
        "positionMargin": 0.034749700000000007,
        "direction": "SHORT"
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                 | Description                              | Data Type |
| --------------------- | ---------------------------------------- | --------- |
| hasMore               | Whether having more data                 | Boolean   |
| nextOffsetId          | Next offset id                           | String    |
| range                 | year month eg: 202207                    | String    |
| symbol                | Symbol name                              | String    |
| realizedPNLChanged    | Realized profit and loss  changed        | Decimal   |
| orderId               | Order id                                 | String    |
| fee                   | The accumulated fee                      | Decimal   |
| type                  | The position type                        | String    |
| sequenceId            | Sequence Id                              | Number    |
| quantityChanged       | Quantity changed, need multiply the multiplier in meta data | Number    |
| createdAt             | Created time                             | Number    |
| rate                  | The fee rate                             | Decimal   |
| clearingPrice         | Liquidation price                        | Decimal   |
| quantityAfterClearing | Quantity after clearing                  | Number    |
| id                    | Id                                       | Number    |
| positionMargin        | Position margin                          | Decimal   |
| direction             | Direction "SHORT" or "LONG"              | String    |




### All Positions.

**Meta**

API Key Permission：Read

get all positions.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:positionGetAllByUser",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "positions": [
      {
        "val": 40882,
        "leverage": 0,
        "symbol": "BTC_USDT",
        "margin": 0.034749700000000007,
        "returnRate": 117.65,
        "riskLevel": 0,
        "quantity": 2,
        "maxQuantity": 100000,
        "bankruptcyPrice": 0,
        "minimumMaintenanceMarginRate": 0.005,
        "liquidationPrice": 0,
        "fairPrice": 0,
        "maxLeverage": 100,
        "entryPrice": 20441,
        "realizedPNL": -0.0061323,
        "takerFeeRate": 0.0015,
        "closed": false,
        "id": "10119870_100105",
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375,
        "riskLight": 5,
        "updatedAt": 1659603041720,
        "direction": "SHORT"
      }
    ],
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.061826390000000541222085975110530853271484375,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528000000000034,
        "position": 0.034749700000000007,
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375
      }
    }
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**positions**:

| Field                        | Description                              | Data Type |
| ---------------------------- | ---------------------------------------- | --------- |
| val                          | value                                    | Decimal   |
| leverage                     | The leverage                             | Number    |
| symbol                       | The symbol                               | String    |
| margin                       | The margin, should >0                    | Decimal   |
| returnRate                   | Return rate                              | Decimal   |
| riskLevel                    | Risk level for the present account       | Number    |
| quantity                     | The position amount, 0 means closed, need multiply the multiplier in meta | Number    |
| maxQuantity                  | The max quatity under this risk level    | Number    |
| bankruptcyPrice              | Bankruptcy price                         | Decimal   |
| minimumMaintenanceMarginRate | The minimum Maintenance Margin Rate      | Decimal   |
| liquidationPrice             | The liquidation price                    | Decimal   |
| fairPrice                    | The mark price                           | Decimal   |
| maxLeverage                  | The maximum leverage                     | Number    |
| entryPrice                   | Open price                               | Decimal   |
| realizedPNL                  | Realized profit and loss                 | Decimal   |
| takerFeeRate                 | Taker fee rate                           | Decimal   |
| closed                       | Whether closed                           | Boolean   |
| id                           | Position id                              | String    |
| unRealizedPNL                | Unrealized profit and loss               | Decimal   |
| riskLight                    | Risk light,0-no one ligthing, 5- all lighting | Number    |
| updatedAt                    | Updated time                             | Number    |
| direction                    | Direction "SHORT", "LONG"                | String    |



### Order Trading Entries

**Meta**

API Key Permission：Read

Get order trading entries

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetMatchDetails",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- | --------------------- | --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| encryptOrderId | order id              | String    |


**Response Body**:

```json
{
  "result": [
    {
      "createdAt": 1659603041720,
      "symbol": "BTC_USDT",
      "quantity": 2,
      "price": 20441,
      "fee": 0.0061323,
      "taker": true,
      "type": "TAKER",
      "direction": "SHORT"
    }
  ],
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description                              | Data Type |
| --------- | ---------------------------------------- | --------- |
| createdAt | Created time                             | Number    |
| symbol    | Symbol                                   | String    |
| quantity  | Quantity, need to multiply the multiplier in meta data | Number    |
| price     | Trade price                              | Decimal   |
| fee       | Accumulated fee                          | Decimal   |
| taker     | Whether taker                            | Boolean   |
| type      | Position type                            | String    |
| direction | "SHORT" or "LONG"                        | String    |



### Order Info 

**Meta**

API Key Permission：Read

Get order info by client order id

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetMatchDetailsByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field         | Description           | Data Type |
| ------------- | --------------------- | --------- |
| Id            | Request id            | Number    |
| Method        | request func          | String    |
| jsonrpc       | The json-rpc  version | String    |
| clientOrderId | Client order id       | String    |


**Response Body**:

```json
{
  "result": [
    {
      "createdAt": 1659603041720,
      "symbol": "BTC_USDT",
      "quantity": 2,
      "price": 20441,
      "fee": 0.0061323,
      "taker": true,
      "type": "TAKER",
      "direction": "SHORT"
    }
  ],
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description                              | Data Type |
| --------- | ---------------------------------------- | --------- |
| createdAt | Created time                             | Number    |
| symbol    | Symbol                                   | String    |
| quantity  | Quantity, need to multiply the multiplier in meta data | Number    |
| price     | Trade price                              | Decimal   |
| fee       | Accumulated fee                          | Decimal   |
| taker     | Whether taker                            | Boolean   |
| type      | Position type                            | String    |
| direction | "SHORT" or "LONG"                        | String    |



### Cancel Order

**Meta**

API Key Permission：Read

cancel order

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderCancel",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field         | Description           | Data Type |
| ------------- | --------------------- | --------- |
| Id            | Request id            | Number    |
| Method        | request func          | String    |
| jsonrpc       | The json-rpc  version | String    |
| encryptOrderId | order id       | String    |


**Response Body**:

```json
{
  "result":{
    "code":0
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type         |
| ------- | --------------------- | ----------------- |
| id      | Request id            | Integer           |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String            |
| code    | code 0 success other failure | Integer            |



### Query Fee

**Meta**

API Key Permission：Read

Query user's fee rate at this time.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:feeRateQuery",
  "jsonrpc": "2.0",
  "params": {
    "symbol":"BTC_USDT"
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | Symbol name           | String    |


**Response Body**:

```json
{
  "result": {
    "maker": 0.001,
    "taker": 0.0015,
    "timestamp": 1659667196592
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description | Data Type |
| --------- | ----------- | --------- |
| maker     | Maker fee   | Decimal   |
| taker     | Taker fee   | Decimal   |
| timestamp | Timestamp   | Number    |



###  Change Leverage

API Key Permission：Write

Change leverage for specific symbol. Margin will be adjusted if there is open position, frozen will be adjusted if there is active orders. Change leverage may failed if there is no enough available.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionSetLeverage",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "leverage":1
    }
}
```

| Field    | Description           | Data Type     |
| -------- | --------------------- | ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name       | String |
| leverage | leverage       | Integer |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0,
        "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |


###  Change Margin

API Key Permission：Write

Change margin of open position.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionChangeMargin",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "margin":0.1
    }
}
```

| Field    | Description           | Data Type     |
| -------- | --------------------- | ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name       | String |
| margin | margin       | BigDecimal |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
      "code":0,
      "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |



###  Change Risk Level

API Key Permission：Write

Change risk level for symbol.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionSetRiskLevel",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "riskLevel":5
    }
}
```

| Field    | Description           | Data Type     |
| -------- |-----------------------| ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name           | String |
| riskLevel | risk level            | Integer |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0,
        "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |





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

API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Body**:

```json
{
    "id":22,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
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
    "id":22,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| spotsSymbols    | spots symbols info    | Array < SpotsSymbols> |
| spotsCurrencies | spots currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**SpotsSymbols**:

| Field                 | Description                              | Data Type  |
| --------------------- | ---------------------------------------- | ---------- |
| id                    | symbol id                                | Long       |
| name                  | symbol name                              | String     |
| supportMarginTrade    | support margin trade                     | Boolean    |
| hidden                | true: disable, false: un disable         | Boolean    |
| displayOrder          | sort field index                         | Integer    |
| derivative            | derivative                               | Boolean    |
| baseName              | base name                                | String     |
| quoteName             | quote name                               | String     |
| baseScale             | Base currency price precision decimal places | Integer    |
| baseMinimumIncrement  | The minimum change scale of the base currency price | BigDecimal |
| baseMaximumQuantity   | The maximum number of transactions in a single base currency | Integer    |
| baseMinimumQuantity   | The minimum number of transactions in a single base currency | BigDecimal |
| quoteScale            | Denomination currency precision decimal places | Integer    |
| quoteMinimumIncrement | The minimum price change scale of the denominated currency | BigDecimal |
| orderBookAccuracy     | Order Book Accuracy  0,0.1,0.01          | String     |
| alwaysChargeQuote     | Fees are always charged in the currency of denomination | Boolean    |
| zone                  | What regions are allowed to trade        | String     |
| endTime               | end time company：ms                      | Long       |
| openTime              | open time company：ms                     | Long       |


**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |


####  SpotsList

API Key Permission：Read

Get spots trading pair info about price, volume, symbol name.

**Request Body**:

```json
{
    "id": 23,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
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
    "id":23,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description     | Data Type  |
| -------- | --------------- | ---------- |
| volume   | volume          | String     |
| symbolId | symbol id       | Integer    |
| price    | price           | String     |
| name     | symbol name     | String     |
| changes  | 24h up and down | BigDecimal |


#### Ticker

API Key Permission：Read

Get kline info around recent 24h on the fixed symbol

**Request Body**:

```json
{
    "id":24,
    "method":"spotsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

**Response Body**:



```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
          1594973040000,		//timestamp
          9100.8,				//open
          9109.4,				//high
          9099.7,				//low
          9109.4,				//close
          0.2004,				//amount
          2.1					//volume
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":24,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

Format Explanation:

```json
[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]
```



#### AllTicker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Body**:

```json
{
    "id":25,
    "method":"spotsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
        	"data":[
        	  1594973040000,		//timestamp
        	  9100.8,				//open
        	  9109.4,				//high
        	  9099.7,				//low
        	  9109.4,				//close
        	  0.2004,				//amount
        	  2.1					//volume
        	],
            "type":"TICKER",
            "sequenceId":"920557",
            "ts":"1652163323227"
        }
    ],
    "id":25,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### OrderBook

API Key Permission：Read

Get the order book

**Request Body**:

```json
{
    "id":26,
    "method":"spotsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |


**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":26,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**:

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Body**:

```json
{
    "id":27,
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        
        [
            1652089500000,	//timestamp
            36476.46,		//open
            36476.46,		//high
            36476.42,		//low
            36476.42,		//close
            0.00002			//amount
        ],
        [
            1652091300000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ]

    ],
    "id":27,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]

#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Body**:

```json
{
    "id":28,
    "method":"spotsKline:ticks",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "limit":1,
        "sequenceId":0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |


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
    "id":28,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]

#### OrderChanges

API Key Permission：Read

**Request Body**:

```json
{
    "id":29,
    "method":"spotsKline:orderChanges",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":29,
    "result":[
        {
            "id":281465252207,
            "sequenceId":28146525,
            "userId":10589459,
            "symbolId":100105,
            "type":"LIMIT",
            "status":"FULLY_FILLED",
            "direction":"SHORT",
            "fillPrice":19842.33,
            "price":19842.33,
            "quantity":0.00046,
            "unfilledQuantity":0,
            "makerFeeRate":0.001,
            "takerFeeRate":0.001,
            "fee":0.0091274718,
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "clientOrderId":"2022071215c26165c601b011ed8edc",
            "createAt":1657609421074,
            "updateAt":1657609421074
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | String               |
| jsonrpc | The json-rpc  version | String               |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| sequenceId        | sequence id                              | Long       |
| symbolId          | symbol id                                | Long       |
| type              | order type limit: Limit order, market: market order | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |


[Abort status](#orderStatus)



#### OrderMatches

API Key Permission：Read

**Request Body**:

```json
{
    "id": 30,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":30,
    "result":[
        {
            "id":112058272207,
            "sequenceId":11205827,
            "userId":10589459,
            "symbolId":100105,
            "orderId":"112057922207",
            "direction":"LONG",
            "price":19145.25,
            "quantity":0.00008,
            "fee":0.00153162,
            "createAt":1656605021615
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Number               |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field      | Description                    | Data Type  |
| ---------- | ------------------------------ | ---------- |
| id         | order id                       | Long       |
| sequenceId | sequence id                    | Long       |
| symbolId   | symbol id                      | Long       |
| orderId    | order id                       | String     |
| direction  | LONG:buy,SHORT:sell            | String     |
| price      | order limit                    | BigDecimal |
| quantity   | quantity of order              | BigDecimal |
| fee        | Total accumulated handling fee | BigDecimal |
| createAt   | create time stamp ms           | Long       |



#### Ping

API Key Permission：Read

Send ping to check  the service whether available

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 31,
    "method": "spotsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "ts": 1653537731930
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| ts      | now time stamp ms     | Long      |

**Response Body**:

```json
{
    "result": {
        "gap": 293585010,
        "type": "PONG",
        "ts": 1653831316940
    },
    "id": 31,
    "jsonrpc": "2.0"
}
```

| Field   | Description                | Data Type |
| ------- | -------------------------- | --------- |
| result  | Return result              | Struct    |
| id      | The result id              | Number    |
| jsonrpc | The json-rpc  version      | String    |
| ts      | server time stamp ms       | Long      |
| type    | service type PONG          | String    |
| gap     | abs(server ts - client ts) | Long      |


## USDT Perpetual contracts

###  K-Line contracts streams



#### Check Service

API Key Permission：Read

Send ping to check  the service whether available

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 31,
    "method": "contractsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "ts": 1653537731930
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| ts      | now time stamp ms     | Long      |

**Response Body**:

```json
{
    "result": {
        "gap": 293585010,
        "type": "PONG",
        "ts": 1653831316940
    },
    "id": 31,
    "jsonrpc": "2.0"
}
```

| Field   | Description                | Data Type |
| ------- | -------------------------- | --------- |
| result  | Return result              | Struct    |
| id      | The result id              | Number    |
| jsonrpc | The json-rpc  version      | String    |
| ts      | server time stamp ms       | Long      |
| type    | service type PONG          | String    |
| gap     | abs(server ts - client ts) | Long      |

#### Detailed Market

API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Body**:

```json
{
    "id":22,
    "method":"contractsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":{
        "contractsSymbols":[
            {
                "id":100105,
                "name":"BTC_USDT",
                "endTime":4083840000000,
                "openTime":1648818928000,
                "type":"PERPETUAL",
                "marginCurrency":"BTC",
                "inverse":false,
                "liquidateBy":"MARKET_PRICE",
                "multiplier":0.001,
                "minimumPriceIncrement":0.001,
                "priceStep":1,
                "priceScale":3,
                "quoteScale":2,
                "maximumQuantityPerOrder":10000,
                "riskLimit": {
                  "id":1,
                  "initialMarginRate":0.01,
                  "maintenanceMarginRateStep":0.005,
                  "maxLeverage":10,
                  "riskLimitBase":10,
                  "riskLimitStep":1,
                  "maxRiskLimitSteps":10,
                  "createdAt":1652266654968
                },
                "settlementFeeRate":0.001,
                "displayOrder":false,
                "hidden":false,
                "zone":"MAIN"
            }
        ],
        "contractsCurrencies":[
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
    "id":22,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| contractsSymbols    | contracts symbols info    | Array < contractsSymbols> |
| contractsCurrencies | contracts currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**contractsSymbols**:

| Field                 | Description                       | Data Type  |
| --------------------- |-----------------------------------|------------|
| id                    | symbol id                         | Long       |
| name                  | symbol name                       | String     |
| endTime               | end time company：ms               | Long       |
| openTime              | open time company：ms              | Long       |
| type    | contracts type                    | String     |
| marginCurrency    | margin symbol                     | String     |
| inverse    | is Reverse contract               | Boolean    |
| liquidateBy    | forced Liquidation type           | String     |
| multiplier    | contract multiplier               | BigDecimal |
| minimumPriceIncrement    | minimum price increment           | BigDecimal |
| priceStep    | price step                        | Integer    |
| priceScale    | price scale                       | Integer    |
| quantityScale    | quantity scale                    | Integer    |
| maximumQuantityPerOrder    | maximum quantity per order        | Integer    |
| riskLimit    | risk limit                        | Object     |
| settlementFeeRate    | settlement fee rate               | BigDecimal |
| hidden                | true: disable, false: un disable  | Boolean    |
| displayOrder          | sort field index                  | Integer    |
| zone                  | What regions are allowed to trade | String     |

**riskLimit**:

| Field                 | Description                                                                                                                                     | Data Type  |
| --------------------- |-------------------------------------------------------------------------------------------------------------------------------------------------|------------|
| id                    | risk id                                                                                                                                         | Long       |
| initialMarginRate                    | initial margin rate                                                                                                                             | BigDecimal       |
| maintenanceMarginRateStep                    | maintenance margin rate                                                                                                                         | BigDecimal       |
| maxLeverage                    | Maximum leverage                                                                                                                                | Integer       |
| riskLimitBase                    | Base risk limit, the number of contracts under which the first leg of the initial maintenance margin/maintenance margin is calculated           | Long       |
| riskLimitStep                    | Incremental risk limit.  The maintenance margin rate will be automatically increased N times for every N increments beyond the basic risk limit | Long       |
| maxRiskLimitSteps                    | The maximum number of times to increase the risk limit                                                                                          | Integer       |
| createdAt                    | create time                                                                                                                                     | Long       |

**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |



####  Contracts Order List

API Key Permission：Read

Get contracts trading pair info about price, volume, symbol name.

**Request Body**:

```json
{
    "id": 23,
    "method": "contractsKline:contractsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "volume":"0",
            "symbolId":100105,
            "price":23124.1,
            "name":"BTC_USDT",
            "type":"PERPETUAL",
            "marginCurrency":"BTC",
            "leverage":10,
            "changes":-13.23,
            "quantity":3,
            "multiplier":0.001,
            "priceScale":3,
            "quantityScale":3,
            "displayOrder":1
        },
        {
            "volume":"0",
            "symbolId":101105,
            "price":1552.20,
            "name":"ETH_USDT",
            "type":"PERPETUAL",
            "marginCurrency":"BTC",
            "leverage":10,
            "changes":-1.23,
            "quantity":2,
            "multiplier":0.01,
            "priceScale":2,
            "quantityScale":2,
            "displayOrder":1
        }
    ],
    "id":23,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description       | Data Type  |
| -------- |-------------------| ---------- |
| symbolId   | symbol id         | Long     |
| name     | symbol name       | String     |
| type     | contracts type    | String     |
| marginCurrency     | settlement symbol | String     |
| leverage     | leverage          | Integer     |
| price    | price             | BigDecimal     |
| changes  | 24h up and down   | BigDecimal |
| quantity     | quantity          | Long     |
| multiplier     | multiplier        | BigDecimal     |
| priceScale   | price scale       | Integer     |
| quantityScale  | quantity scale    | Integer |
| displayOrder  | display order     | Integer |


#### Ticker

API Key Permission：Read

Get kline info around recent 24h on the fixed symbol

**Request Body**:

```json
{
    "id":24,
    "method":"contractsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

**Response Body**:



```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
          1594973040000,		//timestamp
          9100.8,				//open
          9109.4,				//high
          9099.7,				//low
          9109.4,				//close
          0.2004,				//amount
          2.1					//volume
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":24,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

Format Explanation:

```json
[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]
```



#### All Ticker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Body**:

```json
{
    "id":25,
    "method":"contractsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
        	"data":[
        	  1594973040000,		//timestamp
        	  9100.8,				//open
        	  9109.4,				//high
        	  9099.7,				//low
        	  9109.4,				//close
        	  0.2004,				//amount
        	  2.1					//volume
        	],
            "type":"TICKER",
            "sequenceId":"920557",
            "ts":"1652163323227"
        }
    ],
    "id":25,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### Order Book

API Key Permission：Read

Get the order book

**Request Body**:

```json
{
    "id":26,
    "method":"contractsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |


**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":26,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**:

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between contracts and CFD

**Request Body**:

```json
{
    "id":27,
    "method":"contractsKline:bars",
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        
        [
            1652089500000,	//timestamp
            36476.46,		//open
            36476.46,		//high
            36476.42,		//low
            36476.42,		//close
            0.00002			//amount
        ],
        [
            1652091300000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ]

    ],
    "id":27,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]

#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Body**:

```json
{
    "id":28,
    "method":"contractsKline:ticks",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "limit":1,
        "sequenceId":0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |


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
    "id":28,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]



## Liquid Contract

###  K-Line WebSocket streams

#### History

API Key Permission：Read

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 32,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

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
    "id": 32,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description                    | Data Type          |
| ------- | ------------------------------ | ------------------ |
| code    | Return code                    | Integer            |
| data    | Return data                    | Array< DataObject> |
| message | Return message                 | String             |
| time    | Return timestamp               | String             |
| tid     | tracer id usd for open tracing | String             |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                               |
| high   | High price       | BigDecimal                               |
| low    | Low price        | BigDecimal                               |
| open   | Open price       | BigDecimal                               |
| time   | Market Timestamp | long                                     |


=======
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

## 2022-8-04

Add USDT Perpetual contracts related interfaces.

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

The signature may be different if the request text is different, therefore the request should be normalized and URL encoded before signing. Below we have an example to show how to organize them，currently the first line should be GET, and each line has to be splitted by '\n':

GET <br/>
v2api.moonxbt.com <br />
/api/endpoint <br />
AccessKey=97fde68b-4ca9-4d45-ab09-86821e87080c&SignatureMethod=HmacSHA256&SignatureVersion=1&Timestamp=2022-07-09T07%3A30%3A00

**Http Request Body**:

```json
{ 
    "id": 3, 
    "method": "spotsKline:meta", 
    "jsonrpc": "2.0", 
    "params": { } 
}
```

# Rest Api

The endpoint is: https://v2api.moonxbt.com/api/endpoint

API Key Permission：Need, and please refer to the Authentication chapter.

The signature info can be get by Query

## Spots

### K-line Spots Endpoint

####   Meta
API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":1,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
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
            "BTC", //currency name
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
    "id":1,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| spotsSymbols    | spots symbols info    | Array < SpotsSymbols> |
| spotsCurrencies | spots currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**SpotsSymbols**:

| Field                 | Description                              | Data Type  |
| --------------------- | ---------------------------------------- | ---------- |
| id                    | symbol id                                | Long       |
| name                  | symbol name                              | String     |
| supportMarginTrade    | support margin trade                     | Boolean    |
| hidden                | true: disable, false: un disable         | Boolean    |
| displayOrder          | sort field index                         | Integer    |
| derivative            | derivative                               | Boolean    |
| baseName              | base name                                | String     |
| quoteName             | quote name                               | String     |
| baseScale             | Base currency price precision decimal places | Integer    |
| baseMinimumIncrement  | The minimum change scale of the base currency price | BigDecimal |
| baseMaximumQuantity   | The maximum number of transactions in a single base currency | Integer    |
| baseMinimumQuantity   | The minimum number of transactions in a single base currency | BigDecimal |
| quoteScale            | Denomination currency precision decimal places | Integer    |
| quoteMinimumIncrement | The minimum price change scale of the denominated currency | BigDecimal |
| orderBookAccuracy     | Order Book Accuracy  0,0.1,0.01          | String     |
| alwaysChargeQuote     | Fees are always charged in the currency of denomination | Boolean    |
| zone                  | What regions are allowed to trade        | String     |
| endTime               | end time company：ms                      | Long       |
| openTime              | open time company：ms                     | Long       |


**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |


#### SpotsList

API Key Permission：Read

Get spots trading pair info about price, volume, symbol name.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 2,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
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
    "id":2,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description     | Data Type  |
| -------- | --------------- | ---------- |
| volume   | volume          | String     |
| symbolId | symbol id       | Integer    |
| price    | price           | String     |
| name     | symbol name     | String     |
| changes  | 24h up and down | BigDecimal |



#### Ticker

API Key Permission：Read

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
| Id      | Request id            | Integer   |
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
                1596445560000,
                376.9,
                388.3,
                354.5,
                382.9,
                2196.89,
                67.2
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### AllTicker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 4,
    "method": "spotsKline:allTicker",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |

**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
            "data":[
                1657608060000,
                20430.42,
                20704.04,
                19797.62,
                19906.53,
                22.1553,
                -0.025642644644603
            ],
            "type":"TICKER",
            "sequenceId":"28113983",
            "ts":"1657608114573"
        }
    ],
    "id":3,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |



#### OrderBook

API Key Permission：Read

Get the order book

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 5,
    "method": "spotsKline:orderBook",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |

**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":5,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**：

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":6,
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        [
            1657599300000,              //timestamp
            19969.28,                   //open
            20023.86,                   //high
            19923.3,                    //low
            20001.42,                   //close
            0.35231                     //amount
        ],
        [
            1657600200000,
            20001.45,
            20036.98,
            19895.45,
            20011.13,
            0.40759
        ],
        [
            1657607400000,
            19888.96,
            19924.63,
            19881.84,
            19906.43,
            0.1317
        ]
    ],
    "id":6,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]


#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 7,
    "method": "spotsKline:ticks",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "limit": 1,
        "sequenceId": 0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |



**Response Body**:

```json
{
    "result":[
        {
            "data":[
                1657608814025,  //timestamp
                1,              //direction 1=buy, 0=sell
                19865.38,       //price
                0.00015,        //amount
                0               //flag 0=ordinary transaction
            ],
            "sequenceId":28131450
        }
    ],
    "id":7,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]



#### OrderChanges

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 8,
    "method": "spotsKline:orderChanges",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |


**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":8,
    "result":[
        {
            "id":281465252207,
            "sequenceId":28146525,
            "userId":10589459,
            "symbolId":100105,
            "type":"LIMIT",
            "status":"FULLY_FILLED",
            "direction":"SHORT",
            "fillPrice":19842.33,
            "price":19842.33,
            "quantity":0.00046,
            "unfilledQuantity":0,
            "makerFeeRate":0.001,
            "takerFeeRate":0.001,
            "fee":0.0091274718,
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "clientOrderId":"2022071215c26165c601b011ed8edc",
            "createAt":1657609421074,
            "updateAt":1657609421074
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | String               |
| jsonrpc | The json-rpc  version | String               |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| sequenceId        | sequence id                              | Long       |
| symbolId          | symbol id                                | Long       |
| type              | order type limit: Limit order, market: market order | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |


[Abort status](#orderStatus)

#### OrderMatches

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 9,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":9,
    "result":[
        {
            "id":112058272207,
            "sequenceId":11205827,
            "userId":10589459,
            "symbolId":100105,
            "orderId":"112057922207",
            "direction":"LONG",
            "price":19145.25,
            "quantity":0.00008,
            "fee":0.00153162,
            "createAt":1656605021615
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Number               |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field      | Description                    | Data Type  |
| ---------- | ------------------------------ | ---------- |
| id         | order id                       | Long       |
| sequenceId | sequence id                    | Long       |
| symbolId   | symbol id                      | Long       |
| orderId    | order id                       | String     |
| direction  | LONG:buy,SHORT:sell            | String     |
| price      | order limit                    | BigDecimal |
| quantity   | quantity of order              | BigDecimal |
| fee        | Total accumulated handling fee | BigDecimal |
| createAt   | create time stamp ms           | Long       |


### Spots Trade Endpoint

####  Create Open Order

API Key Permission：Write

Open one order

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":11,
    "method":"spots:createOrder",
    "jsonrpc":"2.0",
    "params":{
        "order":{
            "direction":"LONG",
            "type":"LIMIT",
            "source":"WEB",
            "symbol":"BTC_USDT",
            "quantity":0.01,
            "postOnly":false,
            "hidden":false,
            "price":22538,
            "fillOrKill":false,
            "immediateOrCancel":false
        }
    }
}
```


| Field             | Type    | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| symbol            | string  | Required , exchage pair to trade, such as `BTC_USDT` |
| type              | enum    | Required order type：limit order="LIMIT"，market order="MARKET" |
| direction         | enum    | Required  order direction：buy="LONG"，sell="SHORT" |
| price             | decimal | Only for Limited Order  order price ，such as`7123.5` |
| quantity          | decimal | Required order amount ,such as`1.02`     |
| fillOrKill        | boolean | Not required  whether to FOK. Order，such as`true` |
| immediateOrCancel | boolean | Not required whether to IOC order，such as`true` |
| postOnly          | boolean | Required  whether negtively to trust orders，such as`true` |
| hidden            | boolean | Required whether to trust iceberg orders，such as `true`，the order fee for iceberg orders is of taker's fee |
| clientOrderId     | string  | Optional customerised OrderId，used to query,canel order, which is available within 24h |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":11,
    "result":{
        "symbol":"BTC_USDT",
        "triggerOn":0,
        "quantity":0.01,
        "makerFeeRate":0.001,
        "trailingDistance":0,
        "fee":0,
        "clientOrderId":null,
        "marginTrade":false,
        "trailingBasePrice":0,
        "type":"LIMIT",
        "fillPrice":0,
        "triggerDirection":"LONG",
        "features":0,
        "createdAt":1657246741388,
        "trailing":false,
        "unfilledQuantity":0.01,
        "price":22538,
        "takerFeeRate":0.001,
        "id":"223193832207",
        "status":"PENDING",
        "direction":"SHORT",
        "updatedAt":1657246741388
    }
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | order quantity                           | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)


####  Batch Cancel

API Key Permission：Write

Batch to cancel orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
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

| Field    | Description           | Data Type      |
| -------- | --------------------- | -------------- |
| id       | The result id         | Integer        |
| Method   | request func          | String         |
| jsonrpc  | The json-rpc  version | string         |
| orderIds | cancel order id       | Array< String> |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0
    }
}
```
| Field   | Description                  | Data Type            |
| ------- | ---------------------------- | -------------------- |
| result  | Return result                | Array< ResultObject> |
| id      | The result id                | Integer              |
| jsonrpc | The json-rpc  version        | String               |
| code    | code 0 success other failure | Integer              |

#### Get Open Orders

API Key Permission：Read

Get the open state orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 13,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false
    }
}
```

| Field       | Description             | Data Type |
| ----------- | ----------------------- | --------- |
| id          | The result id           | Integer   |
| Method      | request func            | String    |
| jsonrpc     | The json-rpc  version   | string    |
| marginTrade | margin trade only false | Boolean   |

**Response Body**:

```json
{
    "result":{
        "account":{
            "BTC":{
                "usdtPrice":19938.53,
                "available":500000000,
                "frozen":0,
                "debt":0
            },
            "ETH":{
                "usdtPrice":1074.83,
                "available":500000000,
                "frozen":0,
                "debt":0
            },
            "USDT":{
                "usdtPrice":0,
                "available":499984985,
                "frozen":15015,
                "debt":0
            }
        },
        "order":[
            {
                "symbol":"BTC_USDT",
                "quantity":0.01,
                "triggerOn":0,
                "makerFeeRate":0.001,
                "trailingDistance":0,
                "clientOrderId":null,
                "fee":0,
                "marginTrade":false,
                "chargeQuote":true,
                "trailingBasePrice":0,
                "type":"LIMIT",
                "fillPrice":0,
                "triggerDirection":"LONG",
                "features":0,
                "createdAt":1657611089461,
                "trailing":false,
                "unfilledQuantity":0.01,
                "price":15000,
                "takerFeeRate":0.001,
                "id":"281893222207",
                "status":"PENDING",
                "direction":"LONG",
                "updatedAt":1657611089461
            }
        ]
    },
    "id":13,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description   | Data Type           |
| ------- | ------------- | ------------------- |
| account | Return result | AccountObject       |
| order   | The result id | Array< OrderObject> |


**AccountObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

**OrderObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | order quantity                           | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)

#### Open Symbol

API Key Permission：Read

Get the trade order by symbol

**Request Path**: `POST /api/endpoint`

Get the tradable symbols

**Request Body**:

```json
{
    "id": 14,
    "method": "spots:open",
    "jsonrpc": "2.0",
    "params": {
        "marginTrade": false,
        "symbolName": "BTC_USDT"
    }
}
```

| Field       | Description             | Data Type |
| ----------- | ----------------------- | --------- |
| id          | The result id           | Integer   |
| Method      | request func            | String    |
| jsonrpc     | The json-rpc  version   | string    |
| marginTrade | margin trade only false | Boolean   |
| symbolName  | symbol name             | String    |

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
    "id": 14,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description   | Data Type           |
| ------- | ------------- | ------------------- |
| account | Return result | AccountObject       |
| order   | The result id | Array< OrderObject> |

**AccountObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

**OrderObject**:

| Field            | Description                              | Data Type  |
| ---------------- | ---------------------------------------- | ---------- |
| id               | order id                                 | Long       |
| symbol           | symbol id                                | Long       |
| triggerOn        | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type             | order type limit: Limit order, market: market order | String     |
| marginTrade      | Order feature code                       | Long       |
| features         | Whether it is leveraged trading (currently not open) | String     |
| status           | order status                             | String     |
| direction        | LONG:buy,SHORT:sell                      | String     |
| fillPrice        | Average order price                      | BigDecimal |
| price            | order limit                              | BigDecimal |
| quantity         | quantity of order                        | BigDecimal |
| unfilledQuantity | Number of orders not yet filled          | BigDecimal |
| makerFeeRate     | rate as maker                            | BigDecimal |
| takerFeeRate     | rate as taker                            | BigDecimal |
| fee   | Total accumulated handling fee
| BigDecimal |
| trailingBasePrice   | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique
| String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

[Abort status](#orderStatus)

#### Account

API Key Permission：Read

Get account info

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 15,
    "method": "spots:accounts",
    "jsonrpc": "2.0",
    "params": {}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |

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
    "id": 15,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field     | Description                              | Data Type  |
| --------- | ---------------------------------------- | ---------- |
| key       | BTC,USDT..... is currency name           | String     |
| usdtPrice | The price of usdt corresponding to the currency | BigDecimal |
| available | Available Balance                        | BigDecimal |
| frozen    | freeze                                   | BigDecimal |

#### Closed

API Key Permission：Read

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 16,
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

| Field      | Description           | Data Type |
| ---------- | --------------------- | --------- |
| id         | The result id         | Integer   |
| Method     | request func          | String    |
| jsonrpc    | The json-rpc  version | string    |
| range      | year month eg: 202207 | String    |
| symbolName | symbol name  version  | String    |
| offsetId   | last order id         | String    |
| limit      | The json-rpc  version | Integer   |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":16,
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

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Number       |
| jsonrpc | The json-rpc  version | String       |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The trigger price for stop orders, the trigger price for non-stop orders is always 0 | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | rate as maker                            | BigDecimal |
| takerFeeRate      | rate as taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |

[Abort status](#orderStatus)

#### GetOrder

API Key Permission：Read

Get the order by order id

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 17,
    "method": "spots:getOrder",
    "jsonrpc": "2.0",
    "params": {
        "orderId": 9870812206
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| orderId | order id              | String    |

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
    "id": 17,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| symbol            | symbol id                                | Long       |
| triggerOn         | The custom id is globally unique         | BigDecimal |
| type              | order type limit: Limit order, market: market order | String     |
| marginTrade       | Order feature code                       | Long       |
| features          | Whether it is leveraged trading (currently not open) | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | rate as maker                            | BigDecimal |
| takerFeeRate      | rate as taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique
| String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

<a id = "orderStatus">Abort status</a>

### Order Status
OrderStatus：Order Status Description

| Field             | Description                              |
| ----------------- | ---------------------------------------- |
| STOP_PENDING      | Waiting for the triggered stop order;    |
| PENDING           | Activity orders that are waiting to be filled; |
| FAILED            | Order execution failed (insufficient margin, etc.), the final status; |
| STOP_FAILED       | After the Stop order is triggered, the execution fails (for reasons such as insufficient margin), and the final state; |
| FULLY_FILLED      | All transactions, final status;          |
| PARTIAL_FILLED    | Partially sold;                          |
| PARTIAL_CANCELLED | Waiting for a triggered stop order;      |
| STOP_CANCELLED    | The Stop order is canceled by the user before it is triggered, and the final state; |
| FULLY_CANCELLED   | The order is canceled by the user before it is completed, and the final status; |
| STOP_PENDING      | Waiting for a triggered stop order;      |
| STOP_PENDING      | Waiting for a triggered stop order;      |




## Liquid Contract

### K-line Liquid Contract Endpoint

#### 

####  History

API Key Permission：Read

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 18,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

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
    "id": 18,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description                    | Data Type          |
| ------- | ------------------------------ | ------------------ |
| code    | Return code                    | Integer            |
| data    | Return data                    | Array< DataObject> |
| message | Return message                 | String             |
| time    | Return timestamp               | String             |
| tid     | tracer id usd for open tracing | String             |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                               |
| high   | High price       | BigDecimal                               |
| low    | Low price        | BigDecimal                               |
| open   | Open price       | BigDecimal                               |
| time   | Market Timestamp | long                                     |



### Liquid Contract Endpoint

#### 

### Position History

API Key Permission：Read

This endpoint returns a list of historical orders owned by this API user.

**Request Path**: POST /api/endpoint

```json
{
    "id": 19,
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
    "id": 19,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Position Holdings

API Key Permission：Read

This endpoint returns a list of holding orders owned by this API user.

**Request Path**: POST /api/endpoint

**Request Body**:

```json
{
    "id": 20,
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
    "id": 20,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Position Pendings

API Key Permission：Read

This endpoint returns a list of pending orders owned by this API user.

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
    "id": 21,
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
    "id": 21,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



###  Get Orders

API Key Permission：read

Get order details

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:get",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "tradeVO": {
              "code": "d5c5358530a94e60b28fe515afa25901"
            }
      }
}
```

| Parameter    | Description | Mandatory | Data Type | Value Range                                                 |
| ------------ |-------------|-----------|-----------|-------------------------------------------------------------|
| code         | order code  | true      | String    | -                                                           |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:28:18",
            "code": 0,
            "tid": null,
            "data": {
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
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                              | Data Type  |
| ----------------------------- | ---------------------------------------- | ---------- |
| orderType                     | 0:ordinary order, 1: following order 2: followed order | integer    |
| code                          | Order id                                 | String     |
| appendCharge                  | Fee to append                            | integer    |
| extraData                     | Order extra data                         | integer    |
| walletType                    | “contract_usdt”，“Gold”                   | String     |
| orderStatus                   | '': pending, 	0: holdinig, 	2: cancel by user, 	3: cancel by system, 	4: cancel by system admin, 	10: settle by leader, 	11: settle by PnL, 	12: settle by not allow pass night, 	13: settle by user, 	14: settle by pass night fee insufficent, 	15: settle by system | number     |
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



### Pending Order

API Key Permission：Write

create pending orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:pend",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "order": {
                  "pendingPrice": 20000,
                  "symbol": "btcusdt",
                  "leverage": 1,
                  "amount": "2",
                  "direction": 0,
                  "type": 0
            }
      }
}
```

| Parameter    | Description                            | Mandatory | Data Type | Value Range                                                 |
| ------------ |----------------------------------------|-----------|-----------|-------------------------------------------------------------|
| pendingPrice         | pending price                          | true      | Decimal   | -                                                           |
| symbol    | Trading symbol (wildcard inacceptable) | true     | string        | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |
| leverage   | leverage                               | true     | integer   | 1-125                                                       |
| amount        | The amount to cost to buy other token  | true     | Decimal   |                  Decimal                                           |
| direction          | order direction                        | true      | integer   | 0: Buy 1:Sell                                                      |
| type          | Whether to hold the position overnight                         | true     | integer   | 0:order does not stay overnight, 1:order overnight                                                      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:28:18",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "walletName": "contract_usdt",
                  "walletType": "USDT",
                  "orderCode": "9e3144faf99f4e938cbf27284622523e"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| walletName                     | wallet name                  | String   |
| walletType                    | wallet type                  | String    |
| orderCode                   | order code                   | String    |



### Cancel Orders

API Key Permission：Write

cancel orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "cfd:cancel",
  "jsonrpc": "2.0",
  "version": "2.0",
  "params": {
    "code": "9e3144faf99f4e938cbf27284622523e"
  }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range |
| ------------ |---------------------------------------|-----------|-----------|------------|
| code         | order code                            | true      | string   | order code |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:36:56",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Create Orders

API Key Permission：Write

create orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:place",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "order": {
                  "symbol": "btcusdt",
                  "leverage": 1,
                  "amount": "2",
                  "direction": 0,
                  "type": 0
            }
      }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range                                                    |
| ------------ |---------------------------------------|-----------|-----------|----------------------------------------------------------------|
| symbol    | Trading symbol (wildcard inacceptable) | true     | string        | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |
| leverage   | leverage                               | true     | integer   | 1-125                                                          |
| amount        | The amount to cost to buy other token  | true     | Decimal   | Decimal                                                        |
| direction          | order direction                        | true      | integer   | 0: Buy 1:Sell                                                  |
| type          | Whether to hold the position overnight                         | true     | integer   | 0:order does not stay overnight, 1:order overnight             |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:39:23",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "walletName": "contract_usdt",
                  "walletType": "USDT",
                  "orderCode": "01f0a248c8954430a25bad70176561ef"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| walletName                     | wallet name                  | String   |
| walletType                    | wallet type                  | String    |
| orderCode                   | order code                   | String    |



### Settle Orders

API Key Permission：Write

settle orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:settle",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "01f0a248c8954430a25bad70176561ef"
      }
}
```

| Parameter    | Description                           | Mandatory | Data Type | Value Range |
| ------------ |---------------------------------------|-----------|-----------|------------|
| code         | order code                            | true      | string   | order code |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:41:32",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Append Orders

API Key Permission：Write

append orders

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:append",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "facea80b612a4ca6a1c7c0df5c0375be",
            "appendAmount": 5
      }
}
```

| Parameter    | Description   | Mandatory | Data Type | Value Range |
| ------------ |---------------|-----------|-----------|-------------|
| code         | order code    | true      | string    | order code  |
| appendAmount         | append amount | true      | Decimal        | amount      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 19:20:58",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0,
                  "appendMarginLog": "{\"account\":\"10119864\",\"appendCharge\":0,\"appendCount\":1,\"appendMargin\":5,\"appendTime\":1657106448875,\"code\":\"facea80b612a4ca6a1c7c0df5c0375be\",\"currency\":\"btcusdt\",\"deviceType\":\"OpenAPI\",\"direction\":0,\"lever\":2.857142,\"stopLoss\":13739.039731,\"strikePrice\":20056.992305,\"targetProfit\":55156.728837,\"walletName\":\"contract_usdt\",\"walletType\":\"USDT\"}"
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |
| appendMarginLog                  | append orders message               | String   |



### Change Over Night Status

API Key Permission：Write

change over night status

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:changeOverNightType",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "code": "3428656c824f43b985f003ee83660c5d",
            "type": 0
      }
}
```

| Parameter    | Description | Mandatory | Data Type | Value Range |
| ------------ |-------------|-----------|-----------|-------------|
| code         | order code  | true      | string    | order code  |
| type         | 1:pass night, 2:no append margin  | true      | integer   | 1 or 2      |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:44:07",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



### Change Target Profit And Stop Loss

API Key Permission：Write

change target profit and stop loss

**Request Path**: ` Post /api/endpoint`

**Request Body**:

```json
{
      "id": 5,
      "method": "cfd:changeTargetProfitAndStopLoss",
      "jsonrpc": "2.0",
      "version": "2.0",
      "params": {
            "profitAndStopLoss": {
                  "code": "3428656c824f43b985f003ee83660c5d",
                  "targetProfit": 24000,
                  "stopLoss": 19000,
                  "targetProfitRatio": 20,
                  "stopLossRatio": 20
                      
            }
      }
}
```

| Parameter    | Description                                                  | Mandatory | Data Type | Value Range |
| ------------ |--------------------------------------------------------------|-----------|-----------|-------------|
| code         | holding order code                                           | true      | string    | order code  |
| targetProfit         | target Profit. must be used together with stopLossRatio or stopLoss                         | false      | Decimal   | Decimal        |
| stopLoss         | stop loss price. must be used together with targetProfitRatio or targetProfit                         | false      | Decimal   | Decimal        |
| targetProfitRatio         | target profit ratio. must be used together with stopLossRatio or stopLoss                   | false      | Decimal   | 1-100       |
| stopLossRatio         | stop loss ratio. must be used together with targetProfitRatio or targetProfit | false     | Decimal   | 1-100        |

**Response Content**:

```json
{
      "jsonrpc": "2.0",
      "id": 5,
      "result": {
            "message": "Success",
            "time": "2022-07-06 17:53:48",
            "code": 0,
            "tid": null,
            "data": {
                  "code": 0
            }
      }
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

ResultObject:

| Field                         | Description                  | Data Type |
| ----------------------------- |------------------------------|-----------|
| message                     | result message               | String    |
| code                          | code 0 success other failure | integer    |
| time                  | Response time                | String   |



## USDT Perpetual contracts



### Get Closed Orders

**Meta**

API Key Permission：Read

Get closed orders.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetClosed",
  "jsonrpc": "2.0",
  "params": {
    "range":"",
    "symbol":"",
    "offsetId":0,
    "limit":1
  }
}
```

| Field    | Description                              | Data Type |
| -------- | ---------------------------------------- | --------- |
| Id       | Request id                               | Number    |
| Method   | request func                             | String    |
| jsonrpc  | The json-rpc  version                    | String    |
| range    | Range as int like 201808.                | String    |
| symbol   | Return related symbol-only Default to "" (all symbols). | String    |
| offsetId | From start order id. Default to 0. (latest first). | Number    |
| limit    | Limited number per page                  | Number    |


**Response Body**:

```json
{
  "result": {
    "hasMore": true,
    "nextOffsetId": "12035612208",
    "range": "202208",
    "results": [
      {
        "symbol": "BTC_USDT",
        "quantity": 2,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": "",
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603034843,
        "trailing": false,
        "unfilledQuantity": 0,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035682208",
        "status": "FULLY_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603034843
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| hasMore           | Whether having more data                 | Boolean   |
| nextOffsetId      | Next offset id                           | String    |
| range             | year month eg: 202207                    | String    |
| symbol            | Symbol name                              | String    |
| quantity          | Amount                                   | Number    |
| triggerOn         | The trigger price for stop orders, the trigger price for non-stop orders is always 0 | Decimal   |
| makerFeeRate      | Rate as maker                            | Decimal   |
| trailingDistance  | The trigger price distance for Trailing stop orders, otherwise it is always 0 | Decimal   |
| clientOrderId     | The custom id is globally unique         | String    |
| fee               | Total accumulated handling fee           | Decimal   |
| trailingBasePrice | The base price for trailing stop orders, otherwise it will always be 0 | Decimal   |
| type              | order type limit: Limit order, market: market order | String    |
| fillPrice         | Average order price                      | Decimal   |
| triggerDirection  | LONG:buy,SHORT:sell                      | String    |
| features          | Whether it is leveraged trading (currently not open) | Number    |
| createdAt         | create time stamp                        | Number    |
| trailing          | Whether tracing the SL or TP orders      | Boolean   |
| unfilledQuantity  | Number of orders not yet filled          | Number    |
| price             | Order limit price                        | Decimal   |
| takerFeeRate      | Rate as taker                            | Decimal   |
| id                | Order id                                 | String    |
| status            | Order status                             | String    |
| direction         | LONG:buy,SHORT:sell                      | String    |
| updatedAt         | Update time stamp                        | Number    |



### Get Open Orders

**Meta**

API Key Permission：Read

Get open orders.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpens",
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


**Response Body**:

```json
{
  "result": {
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description | Data Type |
| ------- | ----------- | --------- |
| account | Acount info | Object    |
| order   | Orders info | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |



**order**:

| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Limited Or Market Orders

**Meta**

API Key Permission：Read

To get the Limited or Market orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensNormal",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.16352,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |



**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### SL Or TP Orders

**Meta**

API Key Permission：Read

To Get the SL or TP orders

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensTrigger",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.16352,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Aggregated Order Position

**Meta**

API Key Permission：Read


Interface used to get aggregated order position

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpensData",
  "jsonrpc": "2.0",
  "params": {
    "type":0
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| type    | Type                  | Number    |


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ],
    "positions": [
      {
        "val": 40882,
        "leverage": 0,
        "symbol": "BTC_USDT",
        "margin": 0.0347497,
        "returnRate": 117.65,
        "riskLevel": 0,
        "quantity": 2,
        "maxQuantity": 100000,
        "bankruptcyPrice": 0,
        "minimumMaintenanceMarginRate": 0.005,
        "liquidationPrice": 0,
        "fairPrice": 0,
        "maxLeverage": 100,
        "entryPrice": 20441,
        "realizedPNL": -0.0061323,
        "takerFeeRate": 0.0015,
        "closed": false,
        "id": "10119870_100105",
        "unRealizedPNL": 4.0882,
        "riskLight": 5,
        "updatedAt": 1659603041720,
        "direction": "SHORT"
      }
    ],
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.06182639,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528,
        "position": 0.0347497,
        "unRealizedPNL": 4.0882
      }
    }
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



**positions**:

| Field                        | Description                              | Data Type |
| ---------------------------- | ---------------------------------------- | --------- |
| val                          | value                                    | Decimal   |
| leverage                     | The leverage                             | Number    |
| symbol                       | The symbol                               | String    |
| margin                       | The margin, should >0                    | Decimal   |
| returnRate                   | Return rate                              | Decimal   |
| riskLevel                    | Risk level for the present account       | Number    |
| quantity                     | The position amount, 0 means closed, need multiply the multiplier in meta | Number    |
| maxQuantity                  | The max quatity under this risk level    | Number    |
| bankruptcyPrice              | Bankruptcy price                         | Decimal   |
| minimumMaintenanceMarginRate | The minimum Maintenance Margin Rate      | Decimal   |
| liquidationPrice             | The liquidation price                    | Decimal   |
| fairPrice                    | The mark price                           | Decimal   |
| maxLeverage                  | The maximum leverage                     | Number    |
| entryPrice                   | Open price                               | Decimal   |
| realizedPNL                  | Realized profit and loss                 | Decimal   |
| takerFeeRate                 | Taker fee rate                           | Decimal   |
| closed                       | Whether closed                           | Boolean   |
| id                           | Position id                              | String    |
| unRealizedPNL                | Unrealized profit and loss               | Decimal   |
| riskLight                    | Risk light,0-no one ligthing, 5- all lighting | Number    |
| updatedAt                    | Updated time                             | Number    |
| direction                    | Direction "SHORT", "LONG"                | String    |



### Orders Info By Symbol

**Meta**

API Key Permission：Read

Get orders by symbol

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetSymbolOpens",
  "jsonrpc": "2.0",
  "params": {
    "symbol":"BTC_USDT"
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | Symbol name           | String    |


**Response Body**:

```json
{
  "result": {
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.061826390000000541222085975110530853271484375,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528000000000034,
        "position": 0.034749700000000007,
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375
      }
    },
    "order": [
      {
        "symbol": "BTC_USDT",
        "quantity": 10,
        "triggerOn": 0,
        "makerFeeRate": 0.001,
        "trailingDistance": 0,
        "clientOrderId": null,
        "fee": 0.0061323,
        "trailingBasePrice": 0,
        "type": "LIMIT",
        "fillPrice": 20441,
        "triggerDirection": "LONG",
        "features": 0,
        "createdAt": 1659603041720,
        "trailing": false,
        "unfilledQuantity": 8,
        "price": 20441,
        "takerFeeRate": 0.0015,
        "id": "12035832208",
        "status": "PARTIAL_FILLED",
        "direction": "SHORT",
        "updatedAt": 1659603041720
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description | Data Type |
| ------- | ----------- | --------- |
| account | Account     | Object    |
| order   | Order list  | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |

**order**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### Order Info By Order Id

**Meta**

API Key Permission：Read

Get order by id.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetById",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- | --------------------- | --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| encryptOrderId | order id              | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |



### Get Open Order

**Meta**

API Key Permission：Read

Get open order by clientOrderId.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetOpenByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- |-----------------------| --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| clientOrderId | client order id       | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |




### Order Info By ClientOrderId

**Meta**

API Key Permission：Read

Get order by clientOrderId.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- |-----------------------| --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| clientOrderId | client order id       | String    |


**Response Body**:

```json
{
  "result": {
    "symbol": "BTC_USDT",
    "quantity": 10,
    "triggerOn": 0,
    "makerFeeRate": 0.001,
    "trailingDistance": 0,
    "clientOrderId": null,
    "fee": 0.0061323,
    "trailingBasePrice": 0,
    "type": "LIMIT",
    "fillPrice": 20441,
    "triggerDirection": "LONG",
    "features": 0,
    "createdAt": 1659603041720,
    "trailing": false,
    "unfilledQuantity": 8,
    "price": 20441,
    "takerFeeRate": 0.0015,
    "id": "12035832208",
    "status": "PARTIAL_FILLED",
    "direction": "SHORT",
    "updatedAt": 1659603041720
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:


| Field             | Description                              | Data Type |
| ----------------- | ---------------------------------------- | --------- |
| symbol            | Symbol name                              | String    |
| quantity          | Order quantity                           | Number    |
| triggerOn         | The trigger price                        | Decimal   |
| makerFeeRate      | Maker fee rate                           | Decimal   |
| trailingDistance  | Traceing price distance,only for market order, can not set with triggerOn at the same time | Decimal   |
| clientOrderId     | Client order id                          | String    |
| fee               | The accumulated fee                      | Decimal   |
| trailingBasePrice | The tracing base price                   | Decimal   |
| type              | Order type: "LIMIT", "MARKET"            | String    |
| fillPrice         | The average trading price , only for display | Decimal   |
| triggerDirection  | Trigger direction, for instance "LONG"   | String    |
| features          | The order feature code                   | Number    |
| createdAt         | Created time                             | Number    |
| trailing          | Whether to trace                         | Boolean   |
| unfilledQuantity  | The order amount not to be trade; need multiply the  multiplier in meta data | Number    |
| price             | The limit price                          | Decimal   |
| takerFeeRate      | The taker fee rate                       | Decimal   |
| id                | Order ID                                 | String    |
| status            | The order state                          | String    |
| direction         | Order direction:  "LONG", "SHORT"        | String    |
| updatedAt         | Updated time                             | Number    |





### Position Clearings

**Meta**

API Key Permission：Read

Get position clearings by range.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:positionClearingsGetByRange",
  "jsonrpc": "2.0",
  "params": {
    "range":"",
    "symbol":"BTC_USDT",
    "offsetId":"0",
    "limit":1
  }
}
```

| Field    | Description                              | Data Type |
| -------- | ---------------------------------------- | --------- |
| Id       | Request id                               | Number    |
| Method   | request func                             | String    |
| jsonrpc  | The json-rpc  version                    | String    |
| range    | Range as int like 201808.                | String    |
| symbol   | Return related symbol-only Default to "" (all symbols). | String    |
| offsetId | From start order id. Default to 0. (latest first). | String    |
| limit    | Limited number per page                  | Number    |


**Response Body**:

```json
{
  "result": {
    "hasMore": true,
    "nextOffsetId": "9",
    "range": "202208",
    "results": [
      {
        "symbol": "BTC_USDT",
        "realizedPNLChanged": -0.0061323,
        "orderId": "12035832208",
        "fee": 0.0061323,
        "type": "OPEN",
        "sequenceId": 1203583,
        "quantityChanged": 2,
        "createdAt": 1659603041720,
        "rate": 0.0015,
        "clearingPrice": 20441,
        "quantityAfterClearing": 2,
        "id": 10,
        "positionMargin": 0.034749700000000007,
        "direction": "SHORT"
      }
    ]
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field                 | Description                              | Data Type |
| --------------------- | ---------------------------------------- | --------- |
| hasMore               | Whether having more data                 | Boolean   |
| nextOffsetId          | Next offset id                           | String    |
| range                 | year month eg: 202207                    | String    |
| symbol                | Symbol name                              | String    |
| realizedPNLChanged    | Realized profit and loss  changed        | Decimal   |
| orderId               | Order id                                 | String    |
| fee                   | The accumulated fee                      | Decimal   |
| type                  | The position type                        | String    |
| sequenceId            | Sequence Id                              | Number    |
| quantityChanged       | Quantity changed, need multiply the multiplier in meta data | Number    |
| createdAt             | Created time                             | Number    |
| rate                  | The fee rate                             | Decimal   |
| clearingPrice         | Liquidation price                        | Decimal   |
| quantityAfterClearing | Quantity after clearing                  | Number    |
| id                    | Id                                       | Number    |
| positionMargin        | Position margin                          | Decimal   |
| direction             | Direction "SHORT" or "LONG"              | String    |




### All Positions.

**Meta**

API Key Permission：Read

get all positions.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:positionGetAllByUser",
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


**Response Body**:

```json
{
  "result": {
    "positionNum": 1,
    "openNum": 1,
    "positions": [
      {
        "val": 40882,
        "leverage": 0,
        "symbol": "BTC_USDT",
        "margin": 0.034749700000000007,
        "returnRate": 117.65,
        "riskLevel": 0,
        "quantity": 2,
        "maxQuantity": 100000,
        "bankruptcyPrice": 0,
        "minimumMaintenanceMarginRate": 0.005,
        "liquidationPrice": 0,
        "fairPrice": 0,
        "maxLeverage": 100,
        "entryPrice": 20441,
        "realizedPNL": -0.0061323,
        "takerFeeRate": 0.0015,
        "closed": false,
        "id": "10119870_100105",
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375,
        "riskLight": 5,
        "updatedAt": 1659603041720,
        "direction": "SHORT"
      }
    ],
    "triggerNum": 0,
    "account": {
      "USDT": {
        "usdtPrice": 0,
        "assets": 1004.061826390000000541222085975110530853271484375,
        "available": 999.77534869,
        "transferAmount": 999.77534869,
        "frozen": 0.163528000000000034,
        "position": 0.034749700000000007,
        "unRealizedPNL": 4.088200000000000500222085975110530853271484375
      }
    }
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field       | Description                              | Data Type |
| ----------- | ---------------------------------------- | --------- |
| positionNum | The position num                         | Number    |
| openNum     | The open order num, only owned by trust or SL interface | Number    |
| triggerNum  | The trigger order num                    | Number    |
| account     | The acount                               | Object    |
| order       | The order list                           | List      |

**account**:

| Field          | Description                         | Data Type |
| -------------- | ----------------------------------- | --------- |
| usdtPrice      | Usdt price                          | Decimal   |
| assets         | Total asset (It is used in example) | Decimal   |
| available      | The amount of asset can be used     | Decimal   |
| transferAmount | The balance can be transfer         | Decimal   |
| frozen         | The Amount  to be frozen            | Decimal   |
| position       | The amount used by position         | Decimal   |
| unRealizedPNL  | Unrealized profit and loss          | Decimal   |


**positions**:

| Field                        | Description                              | Data Type |
| ---------------------------- | ---------------------------------------- | --------- |
| val                          | value                                    | Decimal   |
| leverage                     | The leverage                             | Number    |
| symbol                       | The symbol                               | String    |
| margin                       | The margin, should >0                    | Decimal   |
| returnRate                   | Return rate                              | Decimal   |
| riskLevel                    | Risk level for the present account       | Number    |
| quantity                     | The position amount, 0 means closed, need multiply the multiplier in meta | Number    |
| maxQuantity                  | The max quatity under this risk level    | Number    |
| bankruptcyPrice              | Bankruptcy price                         | Decimal   |
| minimumMaintenanceMarginRate | The minimum Maintenance Margin Rate      | Decimal   |
| liquidationPrice             | The liquidation price                    | Decimal   |
| fairPrice                    | The mark price                           | Decimal   |
| maxLeverage                  | The maximum leverage                     | Number    |
| entryPrice                   | Open price                               | Decimal   |
| realizedPNL                  | Realized profit and loss                 | Decimal   |
| takerFeeRate                 | Taker fee rate                           | Decimal   |
| closed                       | Whether closed                           | Boolean   |
| id                           | Position id                              | String    |
| unRealizedPNL                | Unrealized profit and loss               | Decimal   |
| riskLight                    | Risk light,0-no one ligthing, 5- all lighting | Number    |
| updatedAt                    | Updated time                             | Number    |
| direction                    | Direction "SHORT", "LONG"                | String    |



### Order Trading Entries

**Meta**

API Key Permission：Read

Get order trading entries

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetMatchDetails",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field          | Description           | Data Type |
| -------------- | --------------------- | --------- |
| Id             | Request id            | Number    |
| Method         | request func          | String    |
| jsonrpc        | The json-rpc  version | String    |
| encryptOrderId | order id              | String    |


**Response Body**:

```json
{
  "result": [
    {
      "createdAt": 1659603041720,
      "symbol": "BTC_USDT",
      "quantity": 2,
      "price": 20441,
      "fee": 0.0061323,
      "taker": true,
      "type": "TAKER",
      "direction": "SHORT"
    }
  ],
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description                              | Data Type |
| --------- | ---------------------------------------- | --------- |
| createdAt | Created time                             | Number    |
| symbol    | Symbol                                   | String    |
| quantity  | Quantity, need to multiply the multiplier in meta data | Number    |
| price     | Trade price                              | Decimal   |
| fee       | Accumulated fee                          | Decimal   |
| taker     | Whether taker                            | Boolean   |
| type      | Position type                            | String    |
| direction | "SHORT" or "LONG"                        | String    |



### Order Info 

**Meta**

API Key Permission：Read

Get order info by client order id

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderGetMatchDetailsByClientOrderId",
  "jsonrpc": "2.0",
  "params": {
    "clientOrderId":"12035832208"
  }
}
```

| Field         | Description           | Data Type |
| ------------- | --------------------- | --------- |
| Id            | Request id            | Number    |
| Method        | request func          | String    |
| jsonrpc       | The json-rpc  version | String    |
| clientOrderId | Client order id       | String    |


**Response Body**:

```json
{
  "result": [
    {
      "createdAt": 1659603041720,
      "symbol": "BTC_USDT",
      "quantity": 2,
      "price": 20441,
      "fee": 0.0061323,
      "taker": true,
      "type": "TAKER",
      "direction": "SHORT"
    }
  ],
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description                              | Data Type |
| --------- | ---------------------------------------- | --------- |
| createdAt | Created time                             | Number    |
| symbol    | Symbol                                   | String    |
| quantity  | Quantity, need to multiply the multiplier in meta data | Number    |
| price     | Trade price                              | Decimal   |
| fee       | Accumulated fee                          | Decimal   |
| taker     | Whether taker                            | Boolean   |
| type      | Position type                            | String    |
| direction | "SHORT" or "LONG"                        | String    |



### Cancel Order

**Meta**

API Key Permission：Read

cancel order

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:orderCancel",
  "jsonrpc": "2.0",
  "params": {
    "encryptOrderId":"12035832208"
  }
}
```

| Field         | Description           | Data Type |
| ------------- | --------------------- | --------- |
| Id            | Request id            | Number    |
| Method        | request func          | String    |
| jsonrpc       | The json-rpc  version | String    |
| encryptOrderId | order id       | String    |


**Response Body**:

```json
{
  "result":{
    "code":0
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type         |
| ------- | --------------------- | ----------------- |
| id      | Request id            | Integer           |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String            |
| code    | code 0 success other failure | Integer            |



### Query Fee

**Meta**

API Key Permission：Read

Query user's fee rate at this time.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
  "id": 5,
  "method": "contracts:feeRateQuery",
  "jsonrpc": "2.0",
  "params": {
    "symbol":"BTC_USDT"
  }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | Symbol name           | String    |


**Response Body**:

```json
{
  "result": {
    "maker": 0.001,
    "taker": 0.0015,
    "timestamp": 1659667196592
  },
  "id": 5,
  "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type          |
| ------- | --------------------- | ------------------ |
| id      | Request id            | Integer            |
| result  | request func          | List<ResultObject> |
| jsonrpc | The json-rpc  version | String             |

**ResultObject**:

| Field     | Description | Data Type |
| --------- | ----------- | --------- |
| maker     | Maker fee   | Decimal   |
| taker     | Taker fee   | Decimal   |
| timestamp | Timestamp   | Number    |



###  Change Leverage

API Key Permission：Write

Change leverage for specific symbol. Margin will be adjusted if there is open position, frozen will be adjusted if there is active orders. Change leverage may failed if there is no enough available.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionSetLeverage",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "leverage":1
    }
}
```

| Field    | Description           | Data Type     |
| -------- | --------------------- | ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name       | String |
| leverage | leverage       | Integer |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0,
        "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |


###  Change Margin

API Key Permission：Write

Change margin of open position.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionChangeMargin",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "margin":0.1
    }
}
```

| Field    | Description           | Data Type     |
| -------- | --------------------- | ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name       | String |
| margin | margin       | BigDecimal |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
      "code":0,
      "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |



###  Change Risk Level

API Key Permission：Write

Change risk level for symbol.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id":12,
    "method":"contracts:positionSetRiskLevel",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "riskLevel":5
    }
}
```

| Field    | Description           | Data Type     |
| -------- |-----------------------| ------------- |
| id       | The result id         | Integer       |
| Method   | request func          | String        |
| jsonrpc  | The json-rpc  version | string        |
| symbol | Symbol name           | String |
| riskLevel | risk level            | Integer |

**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":12,
    "result":{
        "code":0,
        "msg":"success"
    }
}
```
| Field   | Description                  | Data Type           |
| ------- | ---------------------------- | ------------------- |
| result  | Return result                | ResultObject |
| id      | The result id                | Integer             |
| jsonrpc | The json-rpc  version        | String              |
| code    | code 0 success other failure | Integer             |
| msg    | response message | String             |





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

API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Body**:

```json
{
    "id":22,
    "method":"spotsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
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
    "id":22,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| spotsSymbols    | spots symbols info    | Array < SpotsSymbols> |
| spotsCurrencies | spots currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**SpotsSymbols**:

| Field                 | Description                              | Data Type  |
| --------------------- | ---------------------------------------- | ---------- |
| id                    | symbol id                                | Long       |
| name                  | symbol name                              | String     |
| supportMarginTrade    | support margin trade                     | Boolean    |
| hidden                | true: disable, false: un disable         | Boolean    |
| displayOrder          | sort field index                         | Integer    |
| derivative            | derivative                               | Boolean    |
| baseName              | base name                                | String     |
| quoteName             | quote name                               | String     |
| baseScale             | Base currency price precision decimal places | Integer    |
| baseMinimumIncrement  | The minimum change scale of the base currency price | BigDecimal |
| baseMaximumQuantity   | The maximum number of transactions in a single base currency | Integer    |
| baseMinimumQuantity   | The minimum number of transactions in a single base currency | BigDecimal |
| quoteScale            | Denomination currency precision decimal places | Integer    |
| quoteMinimumIncrement | The minimum price change scale of the denominated currency | BigDecimal |
| orderBookAccuracy     | Order Book Accuracy  0,0.1,0.01          | String     |
| alwaysChargeQuote     | Fees are always charged in the currency of denomination | Boolean    |
| zone                  | What regions are allowed to trade        | String     |
| endTime               | end time company：ms                      | Long       |
| openTime              | open time company：ms                     | Long       |


**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |


####  SpotsList

API Key Permission：Read

Get spots trading pair info about price, volume, symbol name.

**Request Body**:

```json
{
    "id": 23,
    "method": "spotsKline:spotsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
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
    "id":23,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description     | Data Type  |
| -------- | --------------- | ---------- |
| volume   | volume          | String     |
| symbolId | symbol id       | Integer    |
| price    | price           | String     |
| name     | symbol name     | String     |
| changes  | 24h up and down | BigDecimal |


#### Ticker

API Key Permission：Read

Get kline info around recent 24h on the fixed symbol

**Request Body**:

```json
{
    "id":24,
    "method":"spotsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

**Response Body**:



```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
          1594973040000,		//timestamp
          9100.8,				//open
          9109.4,				//high
          9099.7,				//low
          9109.4,				//close
          0.2004,				//amount
          2.1					//volume
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":24,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

Format Explanation:

```json
[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]
```



#### AllTicker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Body**:

```json
{
    "id":25,
    "method":"spotsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
        	"data":[
        	  1594973040000,		//timestamp
        	  9100.8,				//open
        	  9109.4,				//high
        	  9099.7,				//low
        	  9109.4,				//close
        	  0.2004,				//amount
        	  2.1					//volume
        	],
            "type":"TICKER",
            "sequenceId":"920557",
            "ts":"1652163323227"
        }
    ],
    "id":25,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### OrderBook

API Key Permission：Read

Get the order book

**Request Body**:

```json
{
    "id":26,
    "method":"spotsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |


**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":26,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**:

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between spots and CFD

**Request Body**:

```json
{
    "id":27,
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        
        [
            1652089500000,	//timestamp
            36476.46,		//open
            36476.46,		//high
            36476.42,		//low
            36476.42,		//close
            0.00002			//amount
        ],
        [
            1652091300000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ]

    ],
    "id":27,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]

#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Body**:

```json
{
    "id":28,
    "method":"spotsKline:ticks",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "limit":1,
        "sequenceId":0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |


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
    "id":28,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]

#### OrderChanges

API Key Permission：Read

**Request Body**:

```json
{
    "id":29,
    "method":"spotsKline:orderChanges",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "updateAt":1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":29,
    "result":[
        {
            "id":281465252207,
            "sequenceId":28146525,
            "userId":10589459,
            "symbolId":100105,
            "type":"LIMIT",
            "status":"FULLY_FILLED",
            "direction":"SHORT",
            "fillPrice":19842.33,
            "price":19842.33,
            "quantity":0.00046,
            "unfilledQuantity":0,
            "makerFeeRate":0.001,
            "takerFeeRate":0.001,
            "fee":0.0091274718,
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "clientOrderId":"2022071215c26165c601b011ed8edc",
            "createAt":1657609421074,
            "updateAt":1657609421074
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | String               |
| jsonrpc | The json-rpc  version | String               |


**ResultObject**:

| Field             | Description                              | Data Type  |
| ----------------- | ---------------------------------------- | ---------- |
| id                | order id                                 | Long       |
| sequenceId        | sequence id                              | Long       |
| symbolId          | symbol id                                | Long       |
| type              | order type limit: Limit order, market: market order | String     |
| status            | order status                             | String     |
| direction         | LONG:buy,SHORT:sell                      | String     |
| fillPrice         | Average order price                      | BigDecimal |
| price             | order limit                              | BigDecimal |
| quantity          | quantity of order                        | BigDecimal |
| unfilledQuantity  | Number of orders not yet filled          | BigDecimal |
| makerFeeRate      | Rate as Maker                            | BigDecimal |
| takerFeeRate      | Rate as Taker                            | BigDecimal |
| fee               | Total accumulated handling fee           | BigDecimal |
| triggerOn         | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance  | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId     | The custom id is globally unique         | String     |
| createAt          | create time stamp                        | Long       |
| updateAt          | update time stamp                        | Long       |


[Abort status](#orderStatus)



#### OrderMatches

API Key Permission：Read

**Request Body**:

```json
{
    "id": 30,
    "method": "spotsKline:orderMatches",
    "jsonrpc": "2.0",
    "params": {
        "symbol": "BTC_USDT",
        "updateAt": 1653537731930
    }
}
```

| Field    | Description           | Data Type |
| -------- | --------------------- | --------- |
| id       | The result id         | Integer   |
| Method   | request func          | String    |
| jsonrpc  | The json-rpc  version | string    |
| symbol   | name of the symbol    | String    |
| updateAt | latest timestamp      | Long      |



**Response Body**:

```json
{
    "jsonrpc":"2.0",
    "id":30,
    "result":[
        {
            "id":112058272207,
            "sequenceId":11205827,
            "userId":10589459,
            "symbolId":100105,
            "orderId":"112057922207",
            "direction":"LONG",
            "price":19145.25,
            "quantity":0.00008,
            "fee":0.00153162,
            "createAt":1656605021615
        }
    ]
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Number               |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field      | Description                    | Data Type  |
| ---------- | ------------------------------ | ---------- |
| id         | order id                       | Long       |
| sequenceId | sequence id                    | Long       |
| symbolId   | symbol id                      | Long       |
| orderId    | order id                       | String     |
| direction  | LONG:buy,SHORT:sell            | String     |
| price      | order limit                    | BigDecimal |
| quantity   | quantity of order              | BigDecimal |
| fee        | Total accumulated handling fee | BigDecimal |
| createAt   | create time stamp ms           | Long       |



#### Ping

API Key Permission：Read

Send ping to check  the service whether available

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 31,
    "method": "spotsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "ts": 1653537731930
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| ts      | now time stamp ms     | Long      |

**Response Body**:

```json
{
    "result": {
        "gap": 293585010,
        "type": "PONG",
        "ts": 1653831316940
    },
    "id": 31,
    "jsonrpc": "2.0"
}
```

| Field   | Description                | Data Type |
| ------- | -------------------------- | --------- |
| result  | Return result              | Struct    |
| id      | The result id              | Number    |
| jsonrpc | The json-rpc  version      | String    |
| ts      | server time stamp ms       | Long      |
| type    | service type PONG          | String    |
| gap     | abs(server ts - client ts) | Long      |


## USDT Perpetual contracts

###  K-Line contracts streams



#### Check Service

API Key Permission：Read

Send ping to check  the service whether available

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 31,
    "method": "contractsKline:ping",
    "jsonrpc": "2.0",
    "params": {
        "ts": 1653537731930
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | The result id         | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| ts      | now time stamp ms     | Long      |

**Response Body**:

```json
{
    "result": {
        "gap": 293585010,
        "type": "PONG",
        "ts": 1653831316940
    },
    "id": 31,
    "jsonrpc": "2.0"
}
```

| Field   | Description                | Data Type |
| ------- | -------------------------- | --------- |
| result  | Return result              | Struct    |
| id      | The result id              | Number    |
| jsonrpc | The json-rpc  version      | String    |
| ts      | server time stamp ms       | Long      |
| type    | service type PONG          | String    |
| gap     | abs(server ts - client ts) | Long      |

#### Detailed Market

API Key Permission：Read

Get detailed market trading info about the trading symbol

**Request Body**:

```json
{
    "id":22,
    "method":"contractsKline:meta",
    "jsonrpc":"2.0",
    "params":{}
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":{
        "contractsSymbols":[
            {
                "id":100105,
                "name":"BTC_USDT",
                "endTime":4083840000000,
                "openTime":1648818928000,
                "type":"PERPETUAL",
                "marginCurrency":"BTC",
                "inverse":false,
                "liquidateBy":"MARKET_PRICE",
                "multiplier":0.001,
                "minimumPriceIncrement":0.001,
                "priceStep":1,
                "priceScale":3,
                "quoteScale":2,
                "maximumQuantityPerOrder":10000,
                "riskLimit": {
                  "id":1,
                  "initialMarginRate":0.01,
                  "maintenanceMarginRateStep":0.005,
                  "maxLeverage":10,
                  "riskLimitBase":10,
                  "riskLimitStep":1,
                  "maxRiskLimitSteps":10,
                  "createdAt":1652266654968
                },
                "settlementFeeRate":0.001,
                "displayOrder":false,
                "hidden":false,
                "zone":"MAIN"
            }
        ],
        "contractsCurrencies":[
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
    "id":22,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| id      | Request id            | Integer      |
| result  | request func          | ResultObject |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field           | Description           | Data Type             |
| --------------- | --------------------- | --------------------- |
| contractsSymbols    | contracts symbols info    | Array < contractsSymbols> |
| contractsCurrencies | contracts currencies info | Array < String>       |
| currencies      | currencies info       | Array< Currencies>    |

**contractsSymbols**:

| Field                 | Description                       | Data Type  |
| --------------------- |-----------------------------------|------------|
| id                    | symbol id                         | Long       |
| name                  | symbol name                       | String     |
| endTime               | end time company：ms               | Long       |
| openTime              | open time company：ms              | Long       |
| type    | contracts type                    | String     |
| marginCurrency    | margin symbol                     | String     |
| inverse    | is Reverse contract               | Boolean    |
| liquidateBy    | forced Liquidation type           | String     |
| multiplier    | contract multiplier               | BigDecimal |
| minimumPriceIncrement    | minimum price increment           | BigDecimal |
| priceStep    | price step                        | Integer    |
| priceScale    | price scale                       | Integer    |
| quantityScale    | quantity scale                    | Integer    |
| maximumQuantityPerOrder    | maximum quantity per order        | Integer    |
| riskLimit    | risk limit                        | Object     |
| settlementFeeRate    | settlement fee rate               | BigDecimal |
| hidden                | true: disable, false: un disable  | Boolean    |
| displayOrder          | sort field index                  | Integer    |
| zone                  | What regions are allowed to trade | String     |

**riskLimit**:

| Field                 | Description                                                                                                                                     | Data Type  |
| --------------------- |-------------------------------------------------------------------------------------------------------------------------------------------------|------------|
| id                    | risk id                                                                                                                                         | Long       |
| initialMarginRate                    | initial margin rate                                                                                                                             | BigDecimal       |
| maintenanceMarginRateStep                    | maintenance margin rate                                                                                                                         | BigDecimal       |
| maxLeverage                    | Maximum leverage                                                                                                                                | Integer       |
| riskLimitBase                    | Base risk limit, the number of contracts under which the first leg of the initial maintenance margin/maintenance margin is calculated           | Long       |
| riskLimitStep                    | Incremental risk limit.  The maintenance margin rate will be automatically increased N times for every N increments beyond the basic risk limit | Long       |
| maxRiskLimitSteps                    | The maximum number of times to increase the risk limit                                                                                          | Integer       |
| createdAt                    | create time                                                                                                                                     | Long       |

**Currencies**:

| Field            | Description                      | Data Type |
| ---------------- | -------------------------------- | --------- |
| id               | currency id                      | Long      |
| name             | currency name                    | String    |
| hidden           | true: disable, false: un disable | Boolean   |
| depositOpenTime  | deposit open time                | Long      |
| displayOrder     | sort field index                 | Integer   |
| iconUrl          | icon url                         | String    |
| withdrawOpenTime | withdraw open time               | Long      |
| displayScale     | Display accuracy                 | Long      |



####  Contracts Order List

API Key Permission：Read

Get contracts trading pair info about price, volume, symbol name.

**Request Body**:

```json
{
    "id": 23,
    "method": "contractsKline:contractsList",
    "jsonrpc": "2.0",
    "params": {
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "volume":"0",
            "symbolId":100105,
            "price":23124.1,
            "name":"BTC_USDT",
            "type":"PERPETUAL",
            "marginCurrency":"BTC",
            "leverage":10,
            "changes":-13.23,
            "quantity":3,
            "multiplier":0.001,
            "priceScale":3,
            "quantityScale":3,
            "displayOrder":1
        },
        {
            "volume":"0",
            "symbolId":101105,
            "price":1552.20,
            "name":"ETH_USDT",
            "type":"PERPETUAL",
            "marginCurrency":"BTC",
            "leverage":10,
            "changes":-1.23,
            "quantity":2,
            "multiplier":0.01,
            "priceScale":2,
            "quantityScale":2,
            "displayOrder":1
        }
    ],
    "id":23,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | String               |

**ResultObject**:

| Field    | Description       | Data Type  |
| -------- |-------------------| ---------- |
| symbolId   | symbol id         | Long     |
| name     | symbol name       | String     |
| type     | contracts type    | String     |
| marginCurrency     | settlement symbol | String     |
| leverage     | leverage          | Integer     |
| price    | price             | BigDecimal     |
| changes  | 24h up and down   | BigDecimal |
| quantity     | quantity          | Long     |
| multiplier     | multiplier        | BigDecimal     |
| priceScale   | price scale       | Integer     |
| quantityScale  | quantity scale    | Integer |
| displayOrder  | display order     | Integer |


#### Ticker

API Key Permission：Read

Get kline info around recent 24h on the fixed symbol

**Request Body**:

```json
{
    "id":24,
    "method":"contractsKline:ticker",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"ETH_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | The request symbol    | String    |

**Response Body**:



```json
{
    "result":{
        "symbol":"ETH_USDT",
        "data":[
          1594973040000,		//timestamp
          9100.8,				//open
          9109.4,				//high
          9099.7,				//low
          9109.4,				//close
          0.2004,				//amount
          2.1					//volume
        ],
        "type":"TICKER",
        "sequenceId":"729912",
        "ts":"1652090424479"
    },
    "id":24,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

Format Explanation:

```json
[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]
```



#### All Ticker

API Key Permission：Read

The summary of the K-line info, and the frequency is less than 10op/s

**Request Body**:

```json
{
    "id":25,
    "method":"contractsKline:allTicker",
    "jsonrpc":"2.0",
    "params":{

    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |


**Response Body**:

```json
{
    "result":[
        {
            "symbol":"BTC_USDT",
        	"data":[
        	  1594973040000,		//timestamp
        	  9100.8,				//open
        	  9109.4,				//high
        	  9099.7,				//low
        	  9109.4,				//close
        	  0.2004,				//amount
        	  2.1					//volume
        	],
            "type":"TICKER",
            "sequenceId":"920557",
            "ts":"1652163323227"
        }
    ],
    "id":25,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type            |
| ------- | --------------------- | -------------------- |
| result  | Return result         | Array< ResultObject> |
| id      | The result id         | Integer              |
| jsonrpc | The json-rpc  version | string               |

**ResultObject**:

| Field      | Description                              | Data Type      |
| ---------- | ---------------------------------------- | -------------- |
| symbol     | symbol name                              | String         |
| data       | [ timestamp, open, high, low, close, amount, change ] | Array< String> |
| type       | type                                     | string         |
| sequenceId | sequence id                              | string         |
| ts         | time stamp ms                            | string         |

#### Order Book

API Key Permission：Read

Get the order book

**Request Body**:

```json
{
    "id":26,
    "method":"contractsKline:orderBook",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT"
    }
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Integer   |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol  | symbol name           | String    |


**Response Body**:

```json
{
    "result":{
        "price":19908.76,
        "sellOrders":[
            [
                19909.45,
                0.00017,
                0.00017
            ],
            [
                19914.62,
                0.18917,
                0.18934
            ]
        ],
        "buyOrders":[
            [
                19895.44,
                0.06636,
                0.06636
            ],
            [
                19894.23,
                0.23152,
                0.29788000000000003
            ],
            [
                19892.77,
                0.1756,
                0.47348
            ]
        ],
        "sequenceId":"28109890"
    },
    "id":26,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | number    |
| jsonrpc | The json-rpc  version | string    |

**Result**:

| Field       | Description                              | Data Type      |
| ----------- | ---------------------------------------- | -------------- |
| price       | The newest price                         | BigDecimal     |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String> |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String> |
| sequenceId  | sequence id                              | String         |

#### Bars

API Key Permission：Read

Get the newest bar info about the fixed trading pair, the interface has no difference between contracts and CFD

**Request Body**:

```json
{
    "id":27,
    "method":"contractsKline:bars",
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

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| id      | The result id                            | Integer   |
| Method  | request func                             | String    |
| jsonrpc | The json-rpc  version                    | string    |
| symbol  | name of the symbol                       | String    |
| type    | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0                 | Long      |
| end     | end timestamp ms eg: 0                   | Long      |
| limit   | The bar amount                           | Long      |

**Response Body**:

```json
{
    "result":[
        
        [
            1652089500000,	//timestamp
            36476.46,		//open
            36476.46,		//high
            36476.42,		//low
            36476.42,		//close
            0.00002			//amount
        ],
        [
            1652091300000,
            36667.42,
            36667.42,
            36667.42,
            36667.42,
            0.00002
        ]

    ],
    "id":27,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type               |
| ------- | --------------------- | ----------------------- |
| result  | Return result         | Array< Array < String>> |
| id      | The result id         | Integer                 |
| jsonrpc | The json-rpc  version | string                  |

**Abort result**:

[timestamp, open, high, low, close, amount]

#### Ticks

API Key Permission：Read

Get the recent ticks info

**Request Body**:

```json
{
    "id":28,
    "method":"contractsKline:ticks",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"BTC_USDT",
        "limit":1,
        "sequenceId":0
    }
}
```

| Field      | Description                       | Data Type |
| ---------- | --------------------------------- | --------- |
| id         | The result id                     | Integer   |
| Method     | request func                      | String    |
| jsonrpc    | The json-rpc  version             | string    |
| symbol     | name of the symbol                | String    |
| limit      | The bar amount 1-500，default: 200 | Long      |
| sequenceId | sequence id eg: 0                 | Long      |


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
    "id":28,
    "jsonrpc":"2.0"
}
```

| Field   | Description           | Data Type      |
| ------- | --------------------- | -------------- |
| result  | Return result         | Array< String> |
| id      | The result id         | Integer        |
| jsonrpc | The json-rpc  version | String         |

**Abort data**:

[timestamp, dir, price, amount, flag]



## Liquid Contract

###  K-Line WebSocket streams

#### History

API Key Permission：Read

This endpoint returns a list of K-lines history data for all public users.

**Request Path**: `POST /api/endpoint`

**Request Body**:

```json
{
    "id": 32,
    "method": "cfdKline:history",
    "jsonrpc": "2.0",
    "params":{
        "symbol": "btcusdt",
        "kType": 1,
        "size": 1
    }
}
```

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
    "id": 32,
    "jsonrpc": "2.0"
}
```

| Field   | Description           | Data Type    |
| ------- | --------------------- | ------------ |
| result  | Return result         | ResultObject |
| id      | The result id         | Integer      |
| jsonrpc | The json-rpc  version | String       |

**ResultObject**:

| Field   | Description                    | Data Type          |
| ------- | ------------------------------ | ------------------ |
| code    | Return code                    | Integer            |
| data    | Return data                    | Array< DataObject> |
| message | Return message                 | String             |
| time    | Return timestamp               | String             |
| tid     | tracer id usd for open tracing | String             |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                               |
| high   | High price       | BigDecimal                               |
| low    | Low price        | BigDecimal                               |
| open   | Open price       | BigDecimal                               |
| time   | Market Timestamp | long                                     |




#Rate Limits

## IP Rate Limit

<aside class="notice">
 If you receive a return message with code 000006, your IP has been either temporarily or permanently banned. You should immediately review the below guidelines to ensure your application does not continue to violate the limit. If you are still banned after 30 minutes, you likely have a permanent ban.
</aside>

MoonXBT has different IP frequency limits depending on the request method. We do not recommend running your application at the very edge of these limits in case abnormal network activity results in an unexpected violation.

- **All methods accept a maximum of 30 requests per second**

After violating the limit your IP will be banned for a set period of time (usually 30 minutes). Continually violating the limit will result in a permanent ban. We cannot undo permanent bans or shorten temporary bans.

>>>>>>> Stashed changes
