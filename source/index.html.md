---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://v2.api.moonxbt.com'>SnapEx</a>

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
- The base endpoint is: `https://www.snapex.com`
- All endpoints return either a JSON object or array.
- Data is returned in ascending order. Oldest first, newest last.
- All time and timestamp related fields are in milliseconds.


## HTTP Return Codes

- HTTP 200 OK
- HTTP 4XX return codes are used for malformed requests; the issue is on the sender's side.
- HTTP 5XX return codes are used for internal errors; the issue is on SnapEx's side.

## General Information on Endpoints
- For GET endpoints, parameters must be sent as a query string.
- For POST, PUT, and DELETE endpoints, the parameters may be sent as a query string or in the request body with content type application/x-www-form-urlencoded. You may mix parameters between both the query string and request body if you wish to do so.
- Parameters may be sent in any order.
- If a parameter sent in both the query string and request body, the query string parameter will be used.

## Authentication

**Overview**

The API request may be tampered during internet, therefore all private API must be signed by your API Key (Secrete Key).

Each API Key has permission property, please check the API permission, and make sure your API key has proper permission.

A valid request consists of below parts:

- API Path: for example https://v2.api.moolecloud.com/api/endpoint
- API Access Key: The 'Access Key' in your API Key
- Signature Method: The Hash method that is used to sign, it uses HmacSHA256
- Signature Version: The version for the signature protocol, it uses 1
- Timestamp: The UTC time when the request is sent, e.g. 2017-05-11T16:22:06. It is useful to prevent the request to be intercepted by third-party.
- Parameters: Each API Method has a group of parameters, you can refer to detailed document for each of them.
- For POST request, the parameters needn't be signed and they should be put in request body.
- Signature: The value after signed, it is guarantee the signature is valid and the request is not be tempered.

**Signature Method**

The signature may be different if the request text is different, therefore the request should be normalized before signing. Below signing steps take the order query as an example:

This is a full URL to query one order:


`https://v2.api.moolecloud.com/api/endpoint?`

`AccessKey=73915b6e-e41d-43ff-ae18-0c29df541b43`

`&Signature=fN5ViyRfXY%2FlheM5rGmChIIsvzRD93hXYiUO6BFG8AY%3D`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=10`

`&Timestamp=2022-05-28T08:30:50`
** Http Request Body**:
```json
{ "id": 3, "method": "spotsKline:meta", "jsonrpc": "2.0", "params": { } }
```




# K-Line Spots Endpoints 
## spotsKline:meta
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

HTTP Response Body**:

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
## spotsKline:spotsList

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



## spotsKline:ticker

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

## spotsKline:ticker

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

| Field   | Description           | Data Type |
| ------- | --------------------- | --------- |
| Id      | Request id            | Number    |
| Method  | request func          | String    |
| jsonrpc | The json-rpc  version | String    |



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

## spotsKline:allTicker

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



## spotsKline:orderBook



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



## spotsKline:bars



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

## spotsKline:ticks



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



## spotsKline:orderChanges



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

## spotsKline:orderMatches



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



## spotsKline:ping



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





# K-Line CFD Endpoints 

## cfdKline:history

This endpoint returns a list of K-lines history data for all public users.

**HTTP Request**: `POST /api/kline`

**HTTP Request Body**:

```json
{
    "id":5,
    "method":"cfdKline:history",
    "jsonrpc":"2.0",
    "params":{
        "symbol":"btcusd",
        "kType":5,
        "size":200
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
	"code": 0,
	"data": [
		{
			"open": 9313.2881,
			"close": 9316.2478,
			"low": 9260.2442,
			"high": 9390.1536,
			"vol": 4190.569625,
			"time": 1589414400
		}
	],
	"message": "Success",
	"time": "2020-05-14 11:07:46"
}
```


| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Kline Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |


**Kline Entity**

| Field | Description      | Data Type |
| ----- | ---------------- | --------- |
| close | Close price      | number    |
| high  | High price       | number    |
| low   | Low price        | number    |
| open  | Open price       | number    |
| time  | Market Timestamp | long      |
| vol   | Volume           | number    |



# K-Line WebSocket streams

API Key Permission：No need.

This WebSocket returns a list of K-lines history data for all public users.

**WebSocket Path**: wss://www.snapex.com/ws/v1/kline

<aside class="notice">
The WebSocket server will check the idle connections every 60 seconds. If the WebSocket server does not receive any request frame from the connection within a 1 minute period, the connection will be disconnected. Unsolicited request frames are not allowed.
</aside>


**Reqeust Parameters**：

```json

{

  "kType": 1,
  "symbol": "btcusd",
  "size" 10
}


```

| Parameter         | Description     |  Mandatory      |  Data Type  |  Value Range
| ------------ | -------------------------------- |--------|----|---------------|
|kType| Data range  | true |integer  | 1: 1 day, 2: 1 min, 3: 5 mins, 4: 15 mins, 5: 1 hour, 7: 4 hours |
|size| Fetch size  | true |integer  | 1-200 |
|symbol| Trading symbol (wildcard inacceptable)  | true |string  | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |



**Response Content**:


<aside class="notice">
If you connect to the WebSocket successfully at the 1st time, it will return the response message `PONG`.
</aside>

```json
{
	"code": 0,
	"data": "PONG",
	"message": "Success",
	"time": "2020-05-14 11:07:46"
}
```

<aside class="notice">
After that, you can receive the actual K-line data as below:
</aside>

```json
{
	"code": 0,
	"data": [
		{
			"open": 9313.2881,
			"close": 9316.2478,
			"low": 9260.2442,
			"high": 9390.1536,
			"vol": 4190.569625,
			"time": 1589414400
		}
	],
	"message": "Success",
	"time": "2020-05-14 11:07:46"
}
```


| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Kline Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |


**Kline Entity**

| Field | Description      | Data Type |
| ----- | ---------------- | --------- |
| close | Close price      | number    |
| high  | High price       | number    |
| low   | Low price        | number    |
| open  | Open price       | number    |
| time  | Market Timestamp | long      |
| vol   | Volume           | number    |




# Trading Endpoints

## Position History

API Key Permission：Read

This endpoint returns a list of historical orders owned by this API user.

**HTTP Request**: POST /api/cfd

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
| currencyName | Currency Name     | false     | string    | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |


**Response Content**:


```json
{
	"message": "Success",
	"time": "2020-05-14 11:58:06",
	"code": 0,
	"data": [
			{
				"id": null,
				"code": "fd14628655274ae98b5e86741e1c1558",
				"cid": 2,
				"cusSuperior": null,
				"direction": 0,
				"amount": 150,
				"orderTime": "2020-02-22 18:50:44",
				"strikePrice": 8662.642,
				"settlement": 8662.642,
				"charge": 2.025,
				"profit": 0,
				"orderStatus": 1,
				"overtime": "2020-02-22 18:50:44",
				"state": 0,
				"balance": null,
				"stopLoss": 8000,
				"simulatedTrading": 0,
				"sign": 0,
				"holdPosition": null,
				"symbol": null,
				"type": 1,
				"targetProfit": 9000,
				"money": 152.025,
				"lever": 10,
				"bamoney": 0.6075,
				"interest": 14.58,
				"currencyName": "BSbtcusd",
				"nightCount": null,
				"cusAccount": "13810000011",
				"countryCode": null,
				"countryName": null,
				"pendingTime": null,
				"deviceType": "iOS",
				"profitRate": null,
				"holdingTime": null,
				"appendCharge": null,
				"positions": 0.173157,
				"currentPrice": null,
				"normal": true,
				"simuOrder": false
			}
		]
}
```

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Order Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |



## Position Holdings

API Key Permission：Read

This endpoint returns a list of holding orders owned by this API user.

**HTTP Request**: POST /api/cfd

**HTTP Request**:

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
| currencyName | Currency Name     | false     | string    | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |


**Response Content**:


```json
{
	"message": "Success",
	"time": "2020-05-14 11:58:06",
	"code": 0,
	"data": [
			{
				"id": null,
				"code": "fd14628655274ae98b5e86741e1c1558",
				"cid": 2,
				"cusSuperior": null,
				"direction": 0,
				"amount": 150,
				"orderTime": "2020-02-22 18:50:44",
				"strikePrice": 8662.642,
				"settlement": 8662.642,
				"charge": 2.025,
				"profit": 0,
				"orderStatus": 0,
				"overtime": "2020-02-22 18:50:44",
				"state": 0,
				"balance": null,
				"stopLoss": 8000,
				"simulatedTrading": 0,
				"sign": 0,
				"holdPosition": null,
				"symbol": null,
				"type": 1,
				"targetProfit": 9000,
				"money": 152.025,
				"lever": 10,
				"bamoney": 0.6075,
				"interest": 14.58,
				"currencyName": "BSbtcusd",
				"nightCount": null,
				"cusAccount": "13810000011",
				"countryCode": null,
				"countryName": null,
				"pendingTime": null,
				"deviceType": "iOS",
				"profitRate": null,
				"holdingTime": "13810000011",
				"appendCharge": null,
				"positions": 0.173157,
				"currentPrice": null,
				"normal": true,
				"simuOrder": false
			}
		]
}
```

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Order Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |



## Position Pendings

API Key Permission：Read

This endpoint returns a list of pending orders owned by this API user.

**HTTP Request**: ` Post /api/cfd`

**HTTP Request Body**: 

```json
{
    "id": 5,
    "method": "cfd:apendingList",
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
| currencyName | Currency Name     | false     | string    | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd |
| begin        | Filter start time | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |
| end          | Filter end time   | false     | string    | Format `yyyy-MM-dd HH:mm:ss`             |


**Response Content**:


```json
{
	"message": "Success",
	"time": "2020-05-14 11:58:06",
	"code": 0,
	"data": [
			{
				"id": null,
				"code": "fd14628655274ae98b5e86741e1c1558",
				"cid": 2,
				"cusSuperior": null,
				"direction": 0,
				"amount": 150,
				"orderTime": "2020-02-22 18:50:44",
				"strikePrice": 8662.642,
				"settlement": 8662.642,
				"charge": 2.025,
				"profit": 0,
				"orderStatus": 0,
				"overtime": "2020-02-22 18:50:44",
				"state": 0,
				"balance": null,
				"stopLoss": 8000,
				"simulatedTrading": 0,
				"sign": 0,
				"holdPosition": null,
				"symbol": null,
				"type": 1,
				"targetProfit": 9000,
				"money": 152.025,
				"lever": 10,
				"bamoney": 0.6075,
				"interest": 14.58,
				"currencyName": "BSbtcusd",
				"nightCount": null,
				"cusAccount": "13810000011",
				"countryCode": null,
				"countryName": null,
				"pendingTime": "2020-02-22 18:50:44",
				"deviceType": "iOS",
				"profitRate": null,
				"holdingTime": "13810000011",
				"appendCharge": null,
				"positions": 0.173157,
				"currentPrice": null,
				"normal": true,
				"simuOrder": false
			}
		]
}
```

| Field   | Description                              | Data Type |
| ------- | ---------------------------------------- | --------- |
| code    | Return code                              | integer   |
| data    | Return data (if avaliable, check below `Order Entity` ) | array     |
| message | Return message                           | string    |
| time    | Return timestamp                         | string    |



## Order Entity

| Field            | Description                              | Data Type |
| ---------------- | ---------------------------------------- | --------- |
| id               | Order Id                                 | integer   |
| code             | Order Code                               | string    |
| cid              | Customer Id                              | integer   |
| cusSuperior      | Customer Supervisor                      | integer   |
| direction        | Order Type                               | integer   |
| amount           | Trading Amount                           | number    |
| orderTime        | Order Time -  Format `yyyy-MM-dd HH:mm:ss` | datetime  |
| strikePrice      | Strike Price                             | number    |
| settlement       | Settle Price                             | number    |
| charge           | Service Charge                           | number    |
| profit           | Profit                                   | number    |
| orderStatus      | Order Status - 0: pending, 1: finished   | integer   |
| overtime         | Order Finish Time -  Format `yyyy-MM-dd HH:mm:ss` | datetime  |
| state            | Account Status - 0: pending, 1: forced liquidation, 2: non-overnight charge, 3: manual liquidation, 4: overnight charge due | integer   |
| stopLoss         | Stop-loss                                | number    |
| simulatedTrading | Flag of Simulated Trading - 0: no, 1: yes | integer   |
| sign             | Flag of Overnight Charge - 0: no, 1: yes | integer   |
| holdPosition     | Hold Position                            | string    |
| symbol           | Symbol                                   | string    |
| type             | Flag of Overnight - 0: no, 1: yes        | integer   |
| targetProfit     | Target Profit                            | number    |
| money            | Purchase Money                           | number    |
| lever            | Lever                                    | number    |
| bamoney          | Overnight Charge(per day)                | number    |
| interest         | Overnight Charge(total)                  | number    |
| currencyName     | Currency Name                            | string    |
| nightCount       | Days of Overnight                        | number    |
| cusAccount       | Customer Account                         | number    |
| countryCode      | Country Code                             | string    |
| countryName      | Country Name                             | string    |
| pendingTime      | Pending Time -  Format `yyyy-MM-dd HH:mm:ss` | datetime  |
| deviceType       | Device Type - Range: iOS, Android, Web   | string    |
| profitRate       | Profit Rate                              | number    |
| holdingTime      | Holding Time                             | long      |
| appendCharge     | Append Charge                            | number    |
| positions        | Positions                                | number    |
| currentPrice     | Current Price                            | number    |
| normal           | Order Category - true: normal, false: pending | boolean   |
| simuOrder        | Simulated Order - true: yes, false: no   | boolean   |


