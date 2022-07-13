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

The signature may be different if the request text is different, therefore the request should be normalized and URL encoded before signing. Below we have an example to show how to organize them，currently the first line should be GET, and each line has to be splitted by '\n':

GET
v2api.moonxbt.com
/api/endpoint
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | Request id            | Integer    |
| result  | request func          | ResultObject|
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| spotsSymbols      | spots symbols info            | Array < SpotsSymbols>   |
| spotsCurrencies  | spots currencies info         | Array < String>|
| currencies | currencies info | Array< Currencies>|

**SpotsSymbols**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | symbol id            | Long   |
| name    | symbol name            | String   |
| supportMarginTrade      | support margin trade            | Boolean   |
| hidden      | true: disable, false: un disable             | Boolean   |
| displayOrder      | sort field index           | Integer   |
| derivative      | derivative            | Boolean   |
| baseName      | base name          | String   |
| quoteName      | quote name            | String   |
| baseScale      | Base currency price precision decimal places | Integer   |
| baseMinimumIncrement      | The minimum change scale of the base currency price            | BigDecimal   |
| baseMaximumQuantity      | The maximum number of transactions in a single base currency | Integer   |
| baseMinimumQuantity    | The minimum number of transactions in a single base currency| BigDecimal   |
| quoteScale      | Denomination currency precision decimal places            | Integer   |
| quoteMinimumIncrement  | The minimum price change scale of the denominated currency            | BigDecimal   |
| orderBookAccuracy      | Order Book Accuracy  0,0.1,0.01           | String   |
| alwaysChargeQuote      | Fees are always charged in the currency of denomination           | Boolean   |
| zone      | What regions are allowed to trade           | String   |
| endTime      | end time company：ms            | Long   |
| openTime      | open time company：ms           | Long   |


**Currencies**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | currency id            | Long   |
| name      | currency name            | String   |
| hidden      | true: disable, false: un disable            | Boolean   |
| depositOpenTime      | deposit open time            | Long   |
| displayOrder      | sort field index           | Integer   |
| iconUrl      | icon url            | String   |
| withdrawOpenTime      | withdraw open time            | Long   |
| displayScale      | Display accuracy            | Long   |


#### SpotsList

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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**: 

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| volume  | volume         | String     |
| symbolId      | symbol id        | Integer    |
| price | price | String    |
| name | symbol name | String    |
| changes | 24h up and down | BigDecimal    |



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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| symbol  | symbol name         | String     |
| data      | [ timestamp, open, high, low, close, amount, change ]         | Array< String>    |
| type | type | string    |
| sequenceId | sequence id | string    |
| ts | time stamp ms | string    |

#### AllTicker

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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| symbol  | symbol name         | String     |
| data      | [ timestamp, open, high, low, close, amount, change ]         | Array< String>    |
| type | type | string    |
| sequenceId | sequence id | string    |
| ts | time stamp ms | string    |



#### OrderBook

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
| Id      | Request id            | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol | symbol name | String    |

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

| Field       | Description                              | Data Type  |
| ----------- | ---------------------------------------- | ---------- |
| price       | The newest price                         | BigDecimal |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String>      |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String>      |
| sequenceId | sequence id | String      |

#### Bars

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| type   | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0 | Long      |
| end   | end timestamp ms eg: 0 | Long      |
| limit   | The bar amount | Long      |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< Array < String>>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**Abort result**: 

[timestamp, open, high, low, close, amount]


#### Ticks

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| limit   | The bar amount 1-500，default: 200 | Long      |
| sequenceId   | sequence id eg: 0 | Long      |



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< String>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**Abort data**:

[timestamp, dir, price, amount, flag]



#### OrderChanges
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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| updateAt   | latest timestamp | Long      |


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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | String    |
| jsonrpc | The json-rpc  version | String    |


**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| sequenceId  | sequence id         | Long    |
| symbolId | symbol id | Long    |
| type | order type limit: Limit order, market: market order| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled | BigDecimal |
| makerFeeRate   | Rate as Maker | BigDecimal |
| takerFeeRate   | Rate as Taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| triggerOn   | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice   | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique | String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |


[Abort status](#orderStatus)

#### OrderMatches



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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol   | String    |
| updateAt   | latest timestamp | Long      |



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| sequenceId  | sequence id         | Long    |
| symbolId | symbol id | Long    |
| orderId | order id | String    |
| direction   | LONG:buy,SHORT:sell | String |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| createAt |create time stamp ms| Long |


### Spots Trade Endpoint

####  Create Open Order

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


| Field                 | Type        | Description                              |
| --------------------- | ----------- | ---------------------------------------- |
| symbol           | string  | Required , exchage pair to trade, such as `BTC_USDT` |
| type              | enum    | Required order type：limit order="LIMIT"，market order="MARKET" |
| direction         | enum    | Required  order direction：buy="LONG"，sell="SHORT" |
| price             | decimal | Only for Limited Order  order price ，such as`7123.5` |
| quantity          | decimal | Required order amount ,such as`1.02` |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| symbol | symbol id | Long    |
| triggerOn   | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type | order type limit: Limit order, market: market order| String    |
| marginTrade | Order feature code| Long    |
| features | Whether it is leveraged trading (currently not open)| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | order quantity | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled | BigDecimal |
| makerFeeRate   | Rate as Maker | BigDecimal |
| takerFeeRate   | Rate as Taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| trailingBasePrice   | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique | String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

[Abort status](#orderStatus)


####  Batch Cancel

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| orderIds | cancel order id   | Array< String>    |

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
| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |
| code | code 0 success other failure | Integer |

#### Get Open Orders



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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| marginTrade | margin trade only false  | Boolean    |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| account  | Return result         | AccountObject    |
| order      | The result id         | Array< OrderObject>    |


**AccountObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| key  | BTC,USDT..... is currency name       | String  |
| usdtPrice  | The price of usdt corresponding to the currency         | BigDecimal|
| available  | Available Balance         | BigDecimal|
| frozen  | freeze         | BigDecimal|

**OrderObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| symbol | symbol id | Long    |
| triggerOn   | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| type | order type limit: Limit order, market: market order| String    |
| marginTrade | Order feature code| Long    |
| features | Whether it is leveraged trading (currently not open)| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | order quantity | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled | BigDecimal |
| makerFeeRate   | Rate as Maker | BigDecimal |
| takerFeeRate   | Rate as Taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| trailingBasePrice   | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique | String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

[Abort status](#orderStatus)

#### Open Symbol 

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| marginTrade | margin trade only false  | Boolean    |
| symbolName | symbol name  | String    |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| account  | Return result         | AccountObject    |
| order      | The result id         | Array< OrderObject>    |

**AccountObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| key  | BTC,USDT..... is currency name       | String  |
| usdtPrice  | The price of usdt corresponding to the currency         | BigDecimal|
| available  | Available Balance         | BigDecimal|
| frozen  | freeze         | BigDecimal|

**OrderObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| symbol | symbol id | Long    |
| triggerOn   | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0| BigDecimal |
| type | order type limit: Limit order, market: market order| String    |
| marginTrade | Order feature code| Long    |
| features | Whether it is leveraged trading (currently not open)| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled  | BigDecimal |
| makerFeeRate   | rate as maker | BigDecimal |
| takerFeeRate   | rate as taker | BigDecimal |
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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| key  | BTC,USDT..... is currency name       | String  |
| usdtPrice  | The price of usdt corresponding to the currency         | BigDecimal|
| available  | Available Balance         | BigDecimal|
| frozen  | freeze         | BigDecimal|

#### Closed



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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| range | year month eg: 202207| String |
| symbolName | symbol name  version | String    |
| offsetId | last order id | String    |
| limit | The json-rpc  version | Integer    |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |


**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| symbol | symbol id | Long    |
| triggerOn   | The trigger price for stop orders, the trigger price for non-stop orders is always 0 | BigDecimal |
| type | order type limit: Limit order, market: market order| String    |
| marginTrade | Order feature code| Long    |
| features | Whether it is leveraged trading (currently not open)| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   |  quantity of order | BigDecimal |
| unfilledQuantity   |  Number of orders not yet filled | BigDecimal |
| makerFeeRate   | rate as maker | BigDecimal |
| takerFeeRate   | rate as taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| trailingBasePrice   | The base price for trailing stop orders, otherwise it will always be 0 | BigDecimal |
| trailingDistance | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique | String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |

[Abort status](#orderStatus)

#### GetOrder



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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| orderId | order id| String |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| symbol | symbol id | Long    |
| triggerOn   |The custom id is globally unique | BigDecimal |
| type | order type limit: Limit order, market: market order| String    |
| marginTrade | Order feature code| Long    |
| features | Whether it is leveraged trading (currently not open)| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled | BigDecimal |
| makerFeeRate   | rate as maker | BigDecimal |
| takerFeeRate   | rate as taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| trailingBasePrice   | The base price for trailing stop orders, otherwise it will always be 0| BigDecimal |
| trailingDistance | The trigger price distance for Trailing stop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique
| String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |


### Order Status
<a id = "orderStatus">Abort status</a>
OrderStatus：Order Status Description

| Field  | Description                              |
| ------ | ---------------------------------------- |
| STOP_PENDING | Waiting for the triggered stop order;|
| PENDING | Waiting for the triggered stop order; |
| FAILED | Order execution failed (insufficient margin, etc.), the final status; |
| STOP_FAILED | After the Stop order is triggered, the execution fails (for reasons such as insufficient margin), and the final state; |
| FULLY_FILLED | All transactions, final status; |
| PARTIAL_FILLED | Partially sold; |
| PARTIAL_CANCELLED | Waiting for a triggered stop order; |
| STOP_CANCELLED | The Stop order is canceled by the user before it is triggered, and the final state; |
| FULLY_CANCELLED | The order is canceled by the user before it is completed, and the final status; |
| STOP_PENDING | Waiting for a triggered stop order;|
| STOP_PENDING | Waiting for a triggered stop order; |




## Liquid Contract

### K-line Liquid Contract Endpoint

#### 

####  History

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | Integer   |
| data    | Return data | Array< DataObject>     |
| message | Return message                           | String    |
| time    | Return timestamp                         | String    |
| tid     | tracer id usd for open tracing           | String    |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                                   |
| high   | High price       | BigDecimal                                   |
| low    | Low price        | BigDecimal                                   |
| open   | Open price       | BigDecimal                                   |
| time   | Market Timestamp | long                                     |



### Liquid Contract Endpoint

#### 

#### Position History



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

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



#### Position Holdings

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

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



#### Position Pendings



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

ResultObject: 

| Field   | Description | Data Type  |
| ------ | ---------- | ---------- |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | Request id            | Integer    |
| result  | request func          | ResultObject|
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| spotsSymbols      | spots symbols info            | Array < SpotsSymbols>   |
| spotsCurrencies  | spots currencies info         | Array < String>|
| currencies | currencies info | Array< Currencies>|

**SpotsSymbols**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | symbol id            | Long   |
| name    | symbol name            | String   |
| supportMarginTrade      | support margin trade            | Boolean   |
| hidden      | true: disable, false: un disable             | Boolean   |
| displayOrder      | sort field index           | Integer   |
| derivative      | derivative            | Boolean   |
| baseName      | base name          | String   |
| quoteName      | quote name            | String   |
| baseScale      | Base currency price precision decimal places | Integer   |
| baseMinimumIncrement      | The minimum change scale of the base currency price            | BigDecimal   |
| baseMaximumQuantity      | The maximum number of transactions in a single base currency | Integer   |
| baseMinimumQuantity    | The minimum number of transactions in a single base currency| BigDecimal   |
| quoteScale      | Denomination currency precision decimal places            | Integer   |
| quoteMinimumIncrement  | The minimum price change scale of the denominated currency            | BigDecimal   |
| orderBookAccuracy      | Order Book Accuracy  0,0.1,0.01           | String   |
| alwaysChargeQuote      | Fees are always charged in the currency of denomination           | Boolean   |
| zone      | What regions are allowed to trade           | String   |
| endTime      | end time company：ms            | Long   |
| openTime      | open time company：ms           | Long   |


**Currencies**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| id      | currency id            | Long   |
| name      | currency name            | String   |
| hidden      | true: disable, false: un disable            | Boolean   |
| depositOpenTime      | deposit open time            | Long   |
| displayOrder      | sort field index           | Integer   |
| iconUrl      | icon url            | String   |
| withdrawOpenTime      | withdraw open time            | Long   |
| displayScale      | Display accuracy            | Long   |


####  SpotsList

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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**: 

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| volume  | volume         | String     |
| symbolId      | symbol id        | Integer    |
| price | price | String    |
| name | symbol name | String    |
| changes | 24h up and down | BigDecimal    |


#### Ticker

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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| symbol  | symbol name         | String     |
| data      | [ timestamp, open, high, low, close, amount, change ]         | Array< String>    |
| type | type | string    |
| sequenceId | sequence id | string    |
| ts | time stamp ms | string    |

Format Explanation:

```json
[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, amount,volume]
```



#### AllTicker

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
| Id      | Request id            | Integer    |
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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**ResultObject**:

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| symbol  | symbol name         | String     |
| data      | [ timestamp, open, high, low, close, amount, change ]         | Array< String>    |
| type | type | string    |
| sequenceId | sequence id | string    |
| ts | time stamp ms | string    |

#### OrderBook

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
| Id      | Request id            | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |
| symbol | symbol name | String    |


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

| Field       | Description                              | Data Type  |
| ----------- | ---------------------------------------- | ---------- |
| price       | The newest price                         | BigDecimal |
| Buy orders  | The buy order book: [ price, amount, total ] | Array< String>      |
| Sell orders | The buy order book:  [ price, amount, total ] | Array< String>      |
| sequenceId | sequence id | String      |

#### Bars

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| type   | `MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH` | Enum      |
| start   | start timestamp ms eg: 0 | Long      |
| end   | end timestamp ms eg: 0 | Long      |
| limit   | The bar amount | Long      |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< Array < String>>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | string    |

**Abort result**:

[timestamp, open, high, low, close, amount]

#### Ticks

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| limit   | The bar amount 1-500，default: 200 | Long      |
| sequenceId   | sequence id eg: 0 | Long      |


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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< String>     |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**Abort data**:

[timestamp, dir, price, amount, flag]


#### OrderChanges

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol                       | String    |
| updateAt   | latest timestamp | Long      |



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | String    |
| jsonrpc | The json-rpc  version | String    |


**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| sequenceId  | sequence id         | Long    |
| symbolId | symbol id | Long    |
| type | order type limit: Limit order, market: market order| String    |
| status   | order status | String |
| direction   | LONG:buy,SHORT:sell | String |
| fillPrice   | Average order price | BigDecimal |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| unfilledQuantity   | Number of orders not yet filled | BigDecimal |
| makerFeeRate   | Rate as Maker | BigDecimal |
| takerFeeRate   | Rate as Taker | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| triggerOn   | The trigger price for Stop orders, the trigger price for non-Stop orders is always 0 | BigDecimal |
| trailingBasePrice   | Base price for TrailingStop orders, always 0 for orders other than this type | BigDecimal |
| trailingDistance | The trigger price distance for TrailingStop orders, otherwise it is always 0 | BigDecimal |
| clientOrderId |The custom id is globally unique | String |
| createAt |create time stamp | Long |
| updateAt |update time stamp | Long |


[Abort status](#orderStatus)



#### OrderMatches



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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| symbol | name of the symbol   | String    |
| updateAt   | latest timestamp | Long      |



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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Array< ResultObject>    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | order id         | Long    |
| sequenceId  | sequence id         | Long    |
| symbolId | symbol id | Long    |
| orderId | order id | String    |
| direction   | LONG:buy,SHORT:sell | String |
| price   | order limit | BigDecimal |
| quantity   | quantity of order | BigDecimal |
| fee   | Total accumulated handling fee | BigDecimal |
| createAt |create time stamp ms| Long |



#### Ping

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

| Field  | Description                              | Data Type |
| ------ | ---------------------------------------- | --------- |
| id      | The result id         | Integer    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | string    |
| ts | now time stamp ms   | Long    |

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | Struct    |
| id      | The result id         | Number    |
| jsonrpc | The json-rpc  version | String    |
| ts | server time stamp ms | Long    |
| type | service type PONG| String     |
| gap | abs(server ts - client ts) | Long    |








## Liquid Contract

###  K-Line WebSocket streams

#### History

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| result  | Return result         | ResultObject    |
| id      | The result id         | Integer    |
| jsonrpc | The json-rpc  version | String    |

**ResultObject**:

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | Integer   |
| data    | Return data | Array< DataObject>     |
| message | Return message                           | String    |
| time    | Return timestamp                         | String    |
| tid     | tracer id usd for open tracing           | String    |

**DataObject**:

| Field  | Description      | Data Type                                |
| ------ | ---------------- | ---------------------------------------- |
| volume | Volume           | caculated by base token, for instance USD |
| amount | Volume           | caculated by quote token, for instance BTC |
| close  | Close price      | BigDecimal                                   |
| high   | High price       | BigDecimal                                   |
| low    | Low price        | BigDecimal                                   |
| open   | Open price       | BigDecimal                                   |
| time   | Market Timestamp | long                                     |


