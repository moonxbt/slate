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


## 2020-4-13

1st release of API document.

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

- API Path: for example https://www.spanex.com/api/v1/orders/history/get
- API Access Key: The 'Access Key' in your API Key
- Signature Method: The Hash method that is used to sign, it uses HmacSHA256
- Signature Version: The version for the signature protocol, it uses 1
- Timestamp: The UTC time when the request is sent, e.g. 2017-05-11T16:22:06. It is useful to prevent the request to be intercepted by third-party.
- Parameters: Each API Method has a group of parameters, you can refer to detailed document for each of them.
- For GET request, all the parameters must be signed.
- For POST request, the parameters needn't be signed and they should be put in request body.
- Signature: The value after signed, it is guarantee the signature is valid and the request is not be tempered.

**Signature Method**

The signature may be different if the request text is different, therefore the request should be normalized before signing. Below signing steps take the order query as an example:

This is a full URL to query one order:


`https://www.snapex.com/api/v1/orders/history/get`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=2`

`&Timestamp=2020-05-11T15:19:30`

`&code=1234567890`

1. The request Method (GET or POST, WebSocket use GET), append line break “\n”

GET\n

2. The host with lower case, append line break “\n”

Example: www.snapex.com\n

3. The path, append line break “\n”

For example, query orders:

`/api/v1/order/orders/history/get\n`

For example, WebSocket v1

`/ws/v1`

4. The parameters are URL encoded, and ordered based on ASCII

For example below is the original parameters:

`AccessKeyId=ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx`

`code=1234567890`

`SignatureMethod=HmacSHA256`

`SignatureVersion=1`

`Timestamp=2017-05-11T15%3A19%3A30`

 Use UTF-8 encoding and URL encoded, the hex must be upper case. For example, The semicolon ':' should be encoded as '%3A', The space should be encoded as '%20'.
 The 'timestamp' should be formated as 'YYYY-MM-DDThh:mm:ss' and URL encoded.
Then above parameter should be ordered like below:

`AccessKeyId=ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=1`

`Timestamp=2020-05-11T15%3A19%3A30`

`code=1234567890`

5. Use char “&” to concatenate all parameters

`AccessKeyId=ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx&SignatureMethod=HmacSHA256&SignatureVersion=1&Timestamp=2020-05-11T15%3A19%3A30&code=1234567890`

6. Assemble the pre-signed text

GET\n

www.snapex.com\n

/api/v1/order/orders/history/get\n

AccessKeyId=ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx&SignatureMethod=HmacSHA256&SignatureVersion=1&Timestamp=2020-05-11T15%3A19%3A30&code=1234567890

7. Use the pre-signed text and your Secret Key to generate a signature

Use the pre-signed text in above step and your API Secret Key to generate hash code by HmacSHA256 hash function.
Encode the hash code with base-64 to generate the signature.
Ef65x8IDLyMWVQj3Aqp+B4w+ivaA8d9Oi2SuYtCJ9o=

8. Put the signature into request URL

For Rest interface:

Put all the parameters in the URL
Append the signature in the URL, with parameter name “Signature”.
Finally, the request sent to API should be:

`https://www.snapex.com/api/v1/orders/history/get?AccessKeyId=ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx&code=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=1&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D`

For WebSocket interface:

Fill the value according to required JSON schema
The value in JSON doesn't require URL encode
For example:

```json

{
  "AccessKey":"ylxxxxxx-t9xxxxxx-uixxxxxx-9xxxxxx",
  "SignatureMethod":"HmacSHA256",
  "SignatureVersion":"1",
  "Timestamp":"2020-09-01T18:16:16",
  "Signature":"Ef65x8IDLyMWVQj3Aqp+B4w+ivaA8d9Oi2SuYtCJ9o="
}

```




# K-Line Endpoints 

API Key Permission：No need.

This endpoint returns a list of K-lines history data for all public users.

**HTTP Request**: `GET /api/v1/kline/{symbol}/{kType}/{size}`

<aside class="notice">
You should directly interpret the parameters inside `{}` with your actual parameters.
Example `GET /api/v1/kline/btcusd/1/10`
</aside>


**Reqeust Parameters**：

| Parameter         | Description     |  Mandatory      |  Data Type  |  Value Range
| ------------ | -------------------------------- |--------|----|---------------|
|kType| Data range  | true |integer  | 1: 1 day, 2: 1 min, 3: 5 mins, 4: 15 mins, 5: 1 hour, 7: 4 hours |
|size| Fetch size  | true |integer  | 1-200 |
|symbol| Trading symbol (wildcard inacceptable)  | true |string  | btcusd, ltcusd, xrpusd, eosusd, trxusd, adausd, bchusd, etcusd |

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


| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|code| Return code  |integer  |
|data| Return data (if avaliable, check below `Kline Entity` )  | array  |
|message| Return message |string  |
|time| Return timestamp  |string  |


**Kline Entity**

| Field         | Description                             |     Data Type |
| ------------ | ------------------|--------|
|close | Close price   |number  |
|high | High price   |number  |
|low |  Low price  |number  |
|open | Open price   |number  |
|time | Market Timestamp  |long  |
|vol | Volume   |number  |



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


| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|code| Return code  |integer  |
|data| Return data (if avaliable, check below `Kline Entity` )  | array  |
|message| Return message |string  |
|time| Return timestamp  |string  |


**Kline Entity**

| Field         | Description                             |     Data Type |
| ------------ | ------------------|--------|
|close | Close price   |number  |
|high | High price   |number  |
|low |  Low price  |number  |
|open | Open price   |number  |
|time | Market Timestamp  |long  |
|vol | Volume   |number  |




# Trading Endpoints

## Order History List

API Key Permission：Read

This endpoint returns a list of historical orders owned by this API user.

**HTTP Request**: `GET /api/v1/orders/history/get`

**Reqeust Parameters**：


| Parameter         | Description     |  Mandatory      |  Data Type  |  Value Range |
| ------------ | -------------------------------- |--------|----|---------------|
|code| Order Code  | false |string  | - |
|direction| Order Type  | false |integer  | 1: up, 2: down |
|deviceType| Device Type  | false |string  | iOS, Android, Web |
|currencyName| Currency Name  | false |string  | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd|
|begin| Filter start time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |
|end| Filter end time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |


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

| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|code| Return code  |integer  |
|data| Return data (if avaliable, check below `Order Entity` )  | array  |
|message| Return message |string  |
|time| Return timestamp  |string  |




## Order Holding List

API Key Permission：Read

This endpoint returns a list of holding orders owned by this API user.

**HTTP Request**: `GET /api/v1/orders/holding/get`

**Reqeust Parameters**：


| Parameter         | Description     |  Mandatory      |  Data Type  |  Value Range |
| ------------ | -------------------------------- |--------|----|---------------|
|code| Order Code  | false |string  | - |
|direction| Order Type  | false |integer  | 1: up, 2: down |
|deviceType| Device Type  | false |string  | iOS, Android, Web |
|currencyName| Currency Name  | false |string  | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd|
|begin| Filter start time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |
|end| Filter end time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |


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

| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|code| Return code  |integer  |
|data| Return data (if avaliable, check below `Order Entity` )  | array  |
|message| Return message |string  |
|time| Return timestamp  |string  |



## Order Pending List

API Key Permission：Read

This endpoint returns a list of pending orders owned by this API user.

**HTTP Request**: `GET /api/v1/orders/pending/get`

**Reqeust Parameters**：


| Parameter         | Description     |  Mandatory      |  Data Type  |  Value Range |
| ------------ | -------------------------------- |--------|----|---------------|
|code| Order Code  | false |string  | - |
|direction| Order Type  | false |integer  | 1: up, 2: down |
|deviceType| Device Type  | false |string  | iOS, Android, Web |
|currencyName| Currency Name  | false |string  | BSbtcusd, BSeosusd, BSltcusd, BStrxusd, BSadausd, BSxrpusd, BSethusd, BSetcusd, BSbchusd|
|begin| Filter start time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |
|end| Filter end time  | false |string  | Format `yyyy-MM-dd HH:mm:ss` |


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

| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|code| Return code  |integer  |
|data| Return data (if avaliable, check below `Order Entity` )  | array  |
|message| Return message |string  |
|time| Return timestamp  |string  |



## Order Entity

| Field         | Description                             |    Data Type |
| ------------ | -------------------|-------|
|id| Order Id  |integer  |
|code| Order Code  | string  |
|cid| Customer Id |integer  |
|cusSuperior| Customer Supervisor  |integer  |
|direction| Order Type   |integer  |
|amount| Trading Amount   |number  |
|orderTime| Order Time -  Format `yyyy-MM-dd HH:mm:ss`  |datetime  |
|strikePrice| Strike Price  |number  |
|settlement| Settle Price  |number  |
|charge| Service Charge  |number  |
|profit| Profit |number  |
|orderStatus| Order Status - 0: pending, 1: finished |integer  |
|overtime| Order Finish Time -  Format `yyyy-MM-dd HH:mm:ss`  |datetime  |
|state| Account Status - 0: pending, 1: forced liquidation, 2: non-overnight charge, 3: manual liquidation, 4: overnight charge due |integer  |
|stopLoss| Stop-loss  |number  |
|simulatedTrading| Flag of Simulated Trading - 0: no, 1: yes |integer  |
|sign| Flag of Overnight Charge - 0: no, 1: yes |integer  |
|holdPosition| Hold Position  |string  |
|symbol| Symbol  |string  |
|type| Flag of Overnight - 0: no, 1: yes |integer  |
|targetProfit| Target Profit |number  |
|money| Purchase Money |number  |
|lever| Lever |number  |
|bamoney| Overnight Charge(per day)  |number  |
|interest| Overnight Charge(total)  |number  |
|currencyName| Currency Name  |string  |
|nightCount| Days of Overnight  |number  |
|cusAccount| Customer Account |number  |
|countryCode| Country Code |string  |
|countryName| Country Name |string  |
|pendingTime| Pending Time -  Format `yyyy-MM-dd HH:mm:ss`  |datetime  |
|deviceType| Device Type - Range: iOS, Android, Web | string  |
|profitRate| Profit Rate | number  |
|holdingTime| Holding Time | long  |
|appendCharge| Append Charge | number  |
|positions| Positions | number  |
|currentPrice| Current Price | number  |
|normal| Order Category - true: normal, false: pending | boolean  |
|simuOrder| Simulated Order - true: yes, false: no | boolean  |


