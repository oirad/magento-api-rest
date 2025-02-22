# Magento API REST

A Node.js client wrapper to work with the Magento REST API.

[![npm version](https://badge.fury.io/js/magento-api-rest.svg)](https://www.npmjs.com/package/magento-api-rest)
[![dependencies Status](https://david-dm.org/aadityachakravarty/magento-api-rest/status.svg)](https://david-dm.org/aadityachakravarty/magento-api-rest)

## Installation

```
npm i magento-api-rest
```

## Getting started

Generate API credentials by following these instructions <https://devdocs.magento.com/guides/v2.3/get-started/create-integration.html>.

Make sure to check the resource access is as per your requirements to prevent misuse of the API Keys.

Check out the Magento API endpoints and data that can be manipulated in <https://devdocs.magento.com/redoc/2.3/index.html>.

* The above documentation is for Magento 2.3, latest at the time of making this package. Should be compatible with Magento 2.0 and above.

## Setup

Setup for the new Magento REST API integration (Magento 2.3 or later):

```js
var MagentoAPI = require('magento-api-rest');

var client = new MagentoAPI({
    'url': 'http://www.test.com/index.php/rest',
    'consumerKey': '<OAuth 1.0a consumer key>',
    'consumerSecret': '<OAuth 1.0a consumer secret>',
    'accessToken': '<OAuth 1.0a access token>',
    'tokenSecret': '<OAuth 1.0a access token secret>'
})
```

### Options

| Option              | Type      | Required | Description                                                |
|---------------------|-----------|----------| -----------------------------------------------------------|
| `url`               | `String`  | yes      | Your Store URL                                             |
| `consumerKey`       | `String`  | yes      | Your API consumer key                                      |
| `consumerSecret`    | `String`  | yes      | Your API consumer secret                                   |
| `accessToken`       | `String`  | yes      | Your API Access Token                                      |
| `tokenSecret`       | `String`  | yes      | Your API Access Token Secret                               |
| `version`           | `String`  | no       | Magento REST API version, default is `V1`                  |

## Methods

### GET

- `.get(endpoint)`
- `.get(endpoint, params)`

| Params     | Type     | Description                                                   |
|------------|----------|---------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `orders`                       |
| `params`   | `Object` | JSON object to be sent as params.                             |

### POST

- `.post(endpoint, data)`

| Params     | Type     | Description                                                 |
|------------|----------|-------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `shipments`                  |
| `data`     | `Object` | JSON object to be sent as body.                             |

### PUT

- `.put(endpoint, data)`

| Params     | Type     | Description                                                 |
|------------|----------|-------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `shipments/12`               |
| `data`     | `Object` | JSON object to be sent as body.                             |

### DELETE

- `.delete(endpoint)`

| Params     | Type     | Description                                                     |
|------------|----------|-----------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `orders/12`                      |

### OPTIONS

- `.options(endpoint)`

| Params     | Type     | Description                                                     |
|------------|----------|-----------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `customers/1` or `orders/14`     |


### API

Requests are made with [Axios library](https://github.com/axios/axios) with [support to promises](https://github.com/axios/axios#promises).

```js
var params = {
    "filter_groups": [
        {
            "filters": [
                {
                    "field": "created_at",
                    "value": "2019-08-03 11:22:47",
                    "condition_type": "from"
                }
            ],
            "filters": [
                {   
                    "field": "created_at",
                    "value": "2020-08-03 11:22:47",
                    "condition_type": "to"
                }
            ]
        }
    ],
    "sortOrders": [
        {
            "field": "created_at",
            "direction": "desc"
        }
    ],
    "pageSize": 200
}
```
Or, you can use the inbuilt parser to write the above query as:
```js
var params = {
    "$or": [
        { "created_at": "2019-08-03 11:22:47" },
        { "created_at": "2020-08-03 11:22:47" }
    ],
    "$sort": {
        "created_at": "desc"
    },
    "$page": 200
}
```
* Note: You cannot use both the param writing styles together or it will cause Error 500 on your store.

#### Parser Operators

| Operator | Description |
|---|---|
| $or | Execute OR queries. |
| $from | Starting point of order search via ISO date. |
| $to | Starting point of order search via ISO date. |
| $after | Search after a specific ISO date. |
| $before | Search before a specific ISO date. |
| $sort | Sort the orders, see docs for more. |
| $page | Specifies the per page orders. |

* By default { key: value } translates to an "eq" operation where key = value.

To get more information as to how to form queries natively, use the following reference,
<https://devdocs.magento.com/guides/v2.3/rest/performing-searches.html>.

If you want to use the above object in a request,
```js
function getOrders () {
   client.get('orders', params).then((response) => {
    //  Response Handling
   }).catch((error) => {
    //  Error Handling
   })
}
```
or by async await,

```js
async function getOrders () {
    try {
        let response = await client.get('orders', params);
        // Response Handling
    } catch (e) {
        // Error Handling
    }
}
```

## Build History

- 2019-09-03 - v1.0.0 - Added support for remaning REST functions, removed QUERY function, added parser support.
- 2019-08-22 - v1.0.0-6 - Fixed Post APIs body and partially added separate rest functions.
- 2019-08-10 - v1.0.0-5 - Added Experimental Parser
- 2019-08-10 - v1.0.0-4 - Added Test Cases and added information to README.
- 2019-08-10 - v1.0.0-3 - Miscellaneous Edits
- 2019-08-09 - v1.0.0-2 - Setup for axios and OAuth.
- 2019-08-08 - v1.0.0-1 - Initial API Setup.

## To Do

* Add test cases.