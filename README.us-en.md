# BiKi Open API

English | [简体中文](./README.md)

- [**Getting Started Guide**](#Getting-Started-Guide)

  - [**Creating API Key**](#Creating-api-key)
  - [**Description on the Interface Methods**](#Description-on-the-Interface-Methods)
  - [**The Server**](#The-Server)
  - [**Contact Us**](#Contact-Us)

- [**Spot REST API**](#rest-api)

  - [**URL Access**](#URL-Access)
  - [**Request**](#Request)
  - [**Authentication of Signature**](#Authentication-of-Signature)
  - [**List of REST API**](#List-of-REST-API)
  - [**Get Symbols List**](#Get-Symbols-List)
  - [**Get All Tickers**](#Get-All-Tickers)
  - [**Get Recent Fills**](#Get-Recent-Fills)
  - [**Get Ticker**](#Get-Ticker)
  - [**Get Klines**](#Get-Klines)
  - [**Get Order Book**](#Get-Order-Book)
  - [**Get Account Balances**](#Get-Account-Balances)
  - [**Get Order**](#Get-Order)
  - [**Get All Orders**](#Get-All-Orders)
  - [**Get Recent Trades**](#Get-Recent-Trades)
  - [**Get Order Details**](#Get-Order-Details)
  - [**Creating Order**](#Creating-Order)
  - [**Cancel an Order**](#Cancel-Order)
  - [**Cancel All Orders**](#Cancel-All-Orders)
  - [**Batch creating & cancel orders**](#Batch-creating-and-cancel-orders)

- [**Spot Websocket API**](#websocket-api)

  - [**URL Access**](#host-url)
  - [**Request**](#Request)
  - [**Subscribe for Real-Time Trade Information**](#Subscribe-for-Real-Time-Trade-Information)
  - [**Subscribe for order book**](#Subscribe-for-order-book)
  - [**Subscribe for Klines Data**](#Subscribe-for-Klines-Data)

- [**Contract REST API**](#contract-rest-api)

  - [**URL Access**](#contract-url-access)
  - [**Get Contract List**](#get-contract-list)
  - [**Get Contract Tickers**](#get-contract-tickers)
  - [**Get Contract Trades**](#get-contract-trades)
  - [**Get Contract Order Book**](#get-contract-order-book)
  - [**Get Contract Market Data**](#get-contract-market-data)

## Getting Started Guide

**Welcome to the User and Developer Documentation. BiKi.com offers simple and easy-to-use API Interface to provide account and order management as well as market data.**

### Creating API Key

After the user register an account on **[BiKi](https://www.biki.cc)**，they have to proceed to **[[User Center](https://www.biki.cc/personal/userManagement)] - [[API Management](https://www.biki.cc/personal/apiManagement)]** to create an API key. After complete creating API key, the system will automatically generate a random API Key and Secret Key which can be used to perform programmed transactions. Each account can create a maximum of 5 API Key.

> **Please do not disclose information about API Key and Secret Key to any other person and to protect loss of funds, it is highly recommended that users bind IP address to API Key. Each API Key can bind a maximum of 5 IP addresses. Please use comma to separate the IP address if binding more than 1 IP address.**

### Description on the Interface Methods

**BiKi.com provides 2 types of interface and users may choose the type to suit their usage and preference. You may [refer SDK(click here for SDK page)](/sdk/)**

- REST API

  Provides account and order management as well as market data query and we recommend users to use REST API to perform function e.g. account balance query, trading, order management etc.

- Websocket API

  Provides depth, real-time transaction information as well as market conditions and we recommend users to use Websocket to get real-time data.

### The Server

BiKi.com server is based in Tokyo and to minimize delays on API access, we recommend using compatible servers that can communicate smoothly with our servers in Tokyo.

<br>

## Spot REST API

### URL Access

- **[https://openapi.biki.cc](https://www.biki.cc) [Recommend]**

- **[https://openapi.biki.com](https://www.biki.com)**

### Request

#### Introduction

REST API provides account and order management as well as market data query.

All requests are based on HTTPS protocol and the content-type in the request header must follow the stipulated format:

- **content-type:application/x-www-form-urlencoded**

#### Status Code

Status Code | Description                                         | Remarks
----------- | --------------------------------------------------- | -----------------------------------------------------
0           | Success                                             | code=0 Success code >0 Failed
5           | Failed to place order                               | Please check the accuracy of order price and quantity
6           | Quantity is less than minimum value                 |
7           | Quantity is more than maximum value                 |
8           | Failed to cancel order                              |
9           | Transactions are frozen                             |
13          | System error                                        |
19          | Insufficient Available Balance                      |
22          | Order does not exis                                 |
23          | Trade quantity is missing                           |
24          | Trade price is missing                              |
100001      | Abnormal System                                     |
100002      | System Upgrade                                      |
100004      | Request parameters are not legal                    |
100005      | Error in signature parameters                       |
100007      | Illegal IP                                          | Server IP is not listed in the API-Bound IP List
110004      | Withdrawals are frozen                              |
110025      | Account is being locked by the system administrator |
110041      | Frequency on URL access is too high                 |

### Authentication of Signature

#### Description of Signature

API requests may likely be altered during network transmission and to ensure API is not altered, other than public interface (basic information, market data), API requests via private interface must be signed with API Key in order to verify the authenticity of the parameters or if parameters have been altered during transmission.

#### Steps on Creating API Signature

**Using obtaining asset balance as an example**

- Interface

  - GET /open/api/user/account

- Examples of API Key

  - api_key = 0816016bb06417f50327e2b557d39aaa

  - secret_key = ab5bba291b8e1cabd8009c2ce6aabdb3

**1\. Sort the parameters according to the order of ASCII Codes**

- The order of the original parameters is:

  - time = 156200607

  - api_key = 0816016bb06417f50327e2b557d39aaa

- Sort the parameters according to the order of ASCII Codes

  - api_key = 0816016bb06417f50327e2b557d39aaa

  - time = 156200607

**2\. All parameters are placed together in the format of "parameter name, parameter value" to form a string for signing.**

- api_key0816016bb06417f50327e2b557d39aaatime156200607

**3\. Using the 32-bit MD5 algorithm, place the Secret Key next to the signing string that you have just created to form the final string for signing.**

- MD5(api_key0816016bb06417f50327e2b557d39aaatime156200607ab5bba291b8e1cabd8009c2ce6aabdb3)

- All the letters used in the string for signing shall be in lowercase

  - sign = 5fcf02e226a4bb2fb180be2aaa6fe541

**4\. Add the final string for signing to the path of the API request**

- The final API request to be sent to the Server will be:

  - <https://openapi.biki.cc/open/api/user/account?api_key=0816016bb06417f50327e2b557d39aaa&time=156200607&sign=5fcf02e226a4bb2fb180be2aaa6fe541>

### List of Spot REST API

API                                                                | Interface Type    | Signature | Frequency Limit | Description
------------------------------------------------------------------ | ----------------- | --------- | --------------- | ------------------------------
[GET /open/api/common/symbols](#Get-Symbols-List)                  | Public interfac   | X         | 10 times/sec    | Get Symbols List
[GET /open/api/get_allticker](#Get-All-Tickers)                    | Public interfac   | X         | 10 times/sec    | Get All Tickers
[GET /open/api/market](#Get-Recent-Fills)                          | Public interfac   | X         | 10 times/sec    | Get Recent Fills
[GET /open/api/get_ticker](#Get-Ticker)                            | Public interfac   | X         | 10 times/sec    | Get Ticker
[GET /open/api/get_trades](#Get-Trade-Histories)                   | Public interfac   | X         | 10 times/sec    | Get Trade Histories
[GET /open/api/get_records](#Get-Klines)                           | Public interfac   | X         | 10 times/sec    | Get Klines
[GET /open/api/market_dept](#Get-Order-Book)                       | Public interfac   | X         | 10 times/sec    | Get Order Book
[GET /open/api/user/account](#Get-Account-Balances)                | Private interface | V         | 10 times/sec    | Get Account Balances
[GET /open/api/v2/new_order](#Get-Order)                           | Private interface | V         | 10 times/sec    | Get Order
[GET /open/api/v2/all_order](#Get-All-Orders)                      | Private interface | V         | 10 times/sec    | Get All Orders
[GET /open/api/all_trade](#Get-Recent-Trades)                      | Private interface | V         | 10 times/sec    | Get Recent Trades
[GET /open/api/order_info](#Get-Order-Details)                     | Private interface | V         | 10 times/sec    | Get Order Details
[POST /open/api/create_order](#Creating-Order)                     | Private interface | V         | 100 times/10sec | Creating Order
[POST /open/api/cancel_order](#Cancel-Order)                       | Private interface | V         | 100 times/10sec | Cancel Order
[POST /open/api/cancel_order_all](#Cancel-All-Orders)              | Private interface | V         | 100 times/10sec | Cancel All Orders
[POST /open/api/mass_replaceV2](#Batch-creating-and-cancel-orders) | Private interface | V         | 100 times/10sec | Batch creating & cancel orders

### Get Spot Symbols List

#### GET [/open/api/common/symbols](https://openapi.biki.cc/open/api/common/symbols)

#### Entry Parameters: No

#### Return Parameters:

Parameters       | Data Typ | Description
---------------- | -------- | ----------------------------------
code             | string   | code=0 Success， code >0 Failed
symbol           | string   | symbol
count_coin       | string   | Quote Currency
base_coin        | string   | Base Currency
amount_precision | number   | Quantity precision (0 is a number)
price_precision  | number   | Price precision (0 is a number)

#### Return to example::

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "symbol": "bikiusdt",
            "count_coin": "USDT",
            "amount_precision": 4,
            "base_coin": "BIKI",
            "price_precision": 6
        },
        {
            "symbol": "vdsusdt",
            "count_coin": "USDT",
            "amount_precision": 2,
            "base_coin": "BTC",
            "price_precision": 4
        },
        ...
    ]
}
```

### Get All Spot Tickers

#### GET [/open/api/get_allticker](https://openapi.biki.cc/open/api/get_allticker)

#### Entry Parameters: No

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success, code >0 Failed
symbol     | string    | symbol
vol        | string    | 24H Volume
high       | string    | 24H High
last       | number    | Last
low        | string    | 24H Low
buy        | number    | Buy
sell       | number    | Sell
change     | string    | 24H Price change
rose       | string    | 24H Change rate

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "ticker": [
            {
                "symbol": "bikiusdt",
                "high": "0.1235",
                "vol": "31753853.80270792",
                "last": 0.114906,
                "low": "0.1111",
                "buy": 0.114887,
                "sell": 0.114967,
                "change": "0.0085224",
                "rose": "0.0085224"
            },
            {
                "symbol": "vdsusdt",
                "high": "3.39",
                "vol": "532061.01067007",
                "last": 3.1459,
                "low": "3.1",
                "buy": 3.14,
                "sell": 3.1541,
                "change": "-0.00427296",
                "rose": "-0.00427296"
            },
            {
                "symbol": "btcusdt",
                "high": "10716.3335",
                "vol": "20433.12745191",
                "last": 10521.9785,
                "low": "9864.9351",
                "buy": 10515.7454,
                "sell": 10527.1895,
                "change": "-0.00423288",
                "rose": "-0.00423288"
            },
            ...
        ],
        "date": 1563207200947
    }
}
```

### Get Recent Fills

#### GET [/open/api/market](https://openapi.biki.cc/open/api/market)

#### Entry Parameters: No

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | List Recent Fills

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "eosbtc": 0.00038891,
        "attusdt": 0.000041,
        "heyusdt": 0.021607,
        "ontbtc": 0.00008929,
    ...
    }
}
```

### Get Ticker

#### GET [/open/api/get_ticker](https://openapi.biki.cc/open/api/get_ticker?symbol=btcusdt)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description | Value Range
---------- | --------- | --------- | ----------- | -----------------------------
symbol     | true      | string    | symbol      | btcusdt, ltcusdt, ethusdt ...

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
symbol     | string    | symbol
vol        | string    | Volume
high       | string    | High
last       | number    | Last
low        | string    | Low
buy        | number    | Buy
sell       | number    | Sell
change     | string    | Price change
rose       | string    | Change rate

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "high": "10753.6563",
        "vol": "20193.35399854",
        "last": 10335.8936,
        "low": "9287.7207",
        "buy": 10328.6517,
        "sell": 10340.1291,
        "rose": "-0.00963801",
        "time": 1563530414000
    }
}
```

### Get Trade Histories

#### GET [/open/api/get_trades](https://openapi.biki.cc/open/api/get_trades?symbol=btcusdt)

#### Entry Parameters::

Parameters | Necessary | Data Type | Description | Value Range
---------- | --------- | --------- | ----------- | -----------------------------
symbol     | true      | string    | Symbol      | btcusdt, ltcusdt, ethusdt ...

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | Trade Record

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "amount": 0.55,               // Trade volume
            "price": 0.18519949,          // Trade price
            "id": 447121,
            "type": "buy"                 // type，buy or sell
            "ts":1553690617000            // timestamp
            "ds":2019-3-27 20:43:37       // Trade Time Format Display
        },
        ...
    ]
}
```

### Get Klines

#### GET [/open/api/get_records](https://openapi.biki.cc/open/api/get_records?symbol=btcusdt&period=1)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                                                                   | Value Range
---------- | --------- | --------- | --------------------------------------------------------------------------------------------- | -----------------------------
symbol     | true      | string    | symbol                                                                                        | btcusdt, ltcusdt, ethusdt ...
period     | true      | number    | K Line Cycle in Minutes, 1 for 1 minute, 1 Day for 1440 minutes 1/5/15/30/60/1440/10080/43200

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | Kline Data

##### data description:

```python
"data": [
  [
    1558586460,   // Kline opening timestamp
    7654.7866,    // Opening price
    7654.7866,    // High
    7654.0322,    // Low
    7654.0322,    // Closing price
    26.9234       // Trade volume
  ],
  ...
]
```

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        [
            1558586460,
            7654.7866,
            7654.7866,
            7654.0322,
            7654.0322,
            26.9234
        ],
        [
            1558586520,
            7654.0322,
            7654.0322,
            7654.0322,
            7654.0322,
            0.0
        ],
        ...
    ]
}
```

### Get Order Book

#### GET [/open/api/market_dept](https://openapi.biki.cc/open/api/market_dept?symbol=btcusdt&type=step0)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                            | Value Range
---------- | --------- | --------- | ------------------------------------------------------ | -----------------------------
symbol     | true      | string    | symbol                                                 | btcusdt, ltcusdt, ethusdt ...
type       | true      | string    | Depth Type (Combined Depth 0-2) Step0 as Highest Depth | step0/step1/step2

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
tick       | object    | Order Book

##### description :

```python
"tick": {
    "asks": [             // Sell
        [
            10352.1109,
            0.1959
        ],
        [
            10352.1315,
            0.2393
        ],
        ...
    ],
    "bids": [             // Buy
        [
            10336.1313,
            0.8707
        ],
        [
            10334.3287,
            0.1721
        ],
        ...
    ],
    "time": null
}
```

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "tick": {
            "asks": [
                [
                    10352.1109,
                    0.1959
                ],
                [
                    10352.1315,
                    0.2393
                ],
                ...
            ],
            "bids": [
                [
                    10336.1313,
                    0.8707
                ],
                [
                    10334.3287,
                    0.1721
                ],
                ...
            ],
            "time": null
        }
    }
}
```

### Get Account Balances

#### GET [/open/api/user/account](https://openapi.biki.cc/open/api/user/account?api_key=0816016bb06417f50327e2b557d39aaa&time=156200607&sign=3cdbe8034f7abf2820fc1bbe721e5692)

#### Example of Signing Request

api_key0816016bb06417f50327e2b557d39aaatime156200607

#### Entry Parameters:

Parameters | Necessary | Data Type | Description  | Value Range
---------- | --------- | --------- | ------------ | -----------
api_key    | true      | string    | user api_key |
time       | true      | string    | timestamp    |
sign       | true      | string    | signature    |

#### Return Parameters:

Parameters  | Data Type | Description
----------- | --------- | -------------------------------
code        | string    | code=0 Success， code >0 Failed
total_asset | string    | Total assets
btcValuatin | string    | Total assets in BTC
normal      | number    | Available Balance
locked      | string    | Available Balances after Frozen
coin        | string    | Holding Assets

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "total_asset": "10.79548765",
        "coin_list": [
            {
                "normal": "27599.42",
                "btcValuatin": "2.73373997",
                "locked": "702.40",
                "coin": "usdt"
            },
            {
                "normal": "0.00000000",
                "btcValuatin": "0.00000000",
                "locked": "0.00000000",
                "coin": "eusdt"
            },
            ...
        ]
    }
}
```

### Get Order

#### GET [/open/api/v2/new_order](https://openapi.biki.cc/open/api/v2/new_order)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632symbolbtcusdttime1564132794

#### Entry Parameters:

Parameters | Necessary | Data Type | Description   | Value Range
---------- | --------- | --------- | ------------- | -----------
api_key    | true      | string    | User api_key  |
pageSize   | false     | string    | Data per page |
page       | false     | string    | Page          |
sign       | true      | string    | Signature     |
symbol     | true      | string    | Symbol        |
time       | true      | string    | Timestamp     |

#### Return Parameters:

Parameters    | Data Type | Description
------------- | --------- | --------------------------------------------------------------------------------
code          | string    | code=0 Success， code >0 Failed
count         | number    | count
side          | string    | BUY/SELL
side_msg      | string    | description
status        | number    | Order Status 1 New 2 Done 3 Partial 4 Cancelled 5 Pending Cancel 6 Unusual Order
status_msg    | string    | Order Status in Chinese
type          | number    | 1 Limit Order 2 Market Order
baseCoin      | string    | Base Currency
countCoin     | string    | Quote Currency
price         | string    | Order price
volume        | string    | Order quantity
avg_price     | string    | Average price
deal_volume   | string    | Deal volume
remain_volume | string    | Open volume
deal_price    | string    | Deal Money
created_at    | string    | Create time
tradeList     | object    | Return to example:

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "count": 1000,
        "resultList": [
            {
                "side": "SELL",
                "total_price": "4.18993396",
                "fee": 0.0,
                "created_at": 1563541098939,
                "deal_price": 0.0,
                "avg_price": "0.00000000",
                "countCoin": "USDT",
                "source": 3,
                "type": 1,
                "side_msg": "卖出",
                "volume": "0.00040000",
                "price": "10474.83490000",
                "source_msg": "WEB",
                "status_msg": "未成交",
                "deal_volume": "0.00000000",
                "fee_coin": "USDT",
                "id": 90419538,
                "remain_volume": "0.00040000",
                "baseCoin": "BTC",
                "tradeList": [],
                "status": 1
            },
            ...
        ]
    }
}
```

### Get All Orders

#### GET [/open/api/v2/all_order](https://openapi.biki.cc/open/api/v2/all_order)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632symbolbtcusdttime1564132947

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                                    | Value range
---------- | --------- | --------- | -------------------------------------------------------------- | -----------
api_key    | true      | string    | User api_key                                                   |
startDate  | false     | string    | Start time to be accurate to the seconds "yyyy-MM-dd mm:hh:ss" |
endDate    | false     | string    | End time to be accurate to the seconds "yyyy-MM-dd mm:hh:ss"   |
pageSize   | false     | string    | Data per page                                                  |
page       | false     | string    | Page                                                           |
sign       | true      | string    | Signature                                                      |
symbol     | true      | string    | Symbol                                                         |
time       | true      | string    | Timestamp                                                      |

#### Return Parameters:

Parameters    | Data Type | Description
------------- | --------- | -----------------------------------------------------------------------------------
code          | string    | code=0 Success, code >0 Failed
count         | number    | max count
side          | string    | BUY/SELL
side_msg      | string    | Trade direction in Chines
status        | number    | status 1 New , 2 Done , 3 Partial , 4 Cancelled , 5 Pending Cancel, 6 Unusual Order
status_msg    | string    | Order Status in Chinese
type          | number    | 1 Limit Order, 2 Market Order
baseCoin      | string    | Base Currency
countCoin     | string    | Quote Currency
price         | string    | Order price
volume        | string    | Order quantity
avg_price     | string    | Average price
deal_volume   | string    | Deal volum
remain_volume | string    | Open volume
deal_price    | string    | Deal money
created_at    | string    | Create time
tradeList     | object    | Trade Record

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "count": 1000,
        "resultList": [
            {
                "side": "SELL",
                "total_price": "4.18993396",
                "fee": 0.0,
                "created_at": 1563541098939,
                "deal_price": 0.0,
                "avg_price": "0.00000000",
                "countCoin": "USDT",
                "source": 3,
                "type": 1,
                "side_msg": "卖出",
                "volume": "0.00040000",
                "price": "10474.83490000",
                "source_msg": "WEB",
                "status_msg": "未成交",
                "deal_volume": "0.00000000",
                "fee_coin": "USDT",
                "id": 90419538,
                "remain_volume": "0.00040000",
                "baseCoin": "BTC",
                "tradeList": [],
                "status": 1
            },
            ...
        ]
    }
}
```

### Get Recent Trades

#### GET [/open/api/all_trade](https://openapi.biki.cc/open/api/all_trade)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632symbolbtcusdttime1564133020

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                                    | Value Range
---------- | --------- | --------- | -------------------------------------------------------------- | -----------
api_key    | true      | string    | User api_key                                                   |
startDate  | false     | string    | Start time to be accurate to the seconds "yyyy-MM-dd mm:hh:ss" |
endDate    | false     | string    | End time to be accurate to the seconds "yyyy-MM-dd mm:hh:ss"   |
pageSize   | false     | string    | Data per page                                                  |
page       | false     | string    | Page                                                           |
sort       | false     | string    | 1 Represented inverted order                                   |
symbol     | true      | string    | Symbol                                                         |
sign       | true      | string    | Signature                                                      |
time       | true      | string    | Timestamp                                                      |

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | -----------------------------
code       | string    | code=0 Success code >0 Failed
count      | number    | max count
side       | string    | BUY/SELL
type       | number    |
price      | string    | deal price
volume     | string    | Deal volume
deal_price | string    | Deal money
ctime      | string    | create time

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "count":22,
        "resultList":[
            {
                "volume":"1.000",
                "side":"BUY",
                "feeCoin":"YLB",
                "price":"0.10000000",
                "fee":"0.16431104",
                "ctime":1510996571195,
                "deal_price":"0.10000000",
                "id":306,
                "type":"买入"
            },
            {
                "volume":"0.850",
                "side":"BUY",
                "feeCoin":"YLB",
                "price":"0.10000000",
                "fee":"0.13966438",
                "ctime":1510996571190,
                "deal_price":"0.08500000",
                "id":305,
                "type":"买入"
            },
            ...
        ]
    }
}
```

### Get Order Details

#### GET [/open/api/order_info](https://openapi.biki.cc/open/api/order_info)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632order_id30symbolbtcusdttime1564133078

#### Entry Parameters:

Parameters | Necessary | Data Typ | Description  | Value Range
---------- | --------- | -------- | ------------ | -----------
api_key    | true      | string   | User api_key |
symbol     | true      | string   | Symbol       |
order_id   | true      | string   | Order ID     |
sign       | true      | string   | Signature    |
time       | true      | string   | Timestamp    |

#### Return to example:

```python
{
  code:0,
  msg:"suc",
  data:{
      "order_info":{
            "side": "SELL",
              "total_price": "3.57000000",   // Order Money
              "fee": 0,
              "created_at": 1546759419493,
              "deal_price": 0,
              "avg_price": "0.00000000",
              "countCoin": "USDT",
              "source": 1,
              "type": 1,
              "side_msg": "卖出",
              "volume": "1.00000000",        // Order volume
              "price": "3.57000000",         // Order price
              "source_msg": "WEB",
              "status_msg": "未成交",
              "deal_volume": "0.00000000",   // Deal volume
              "fee_coin": "USDT",
              "id": 16,
              "remain_volume": "1.00000000",
              "baseCoin": "ETC",
              "tradeList": [],
              "status": 0                    // Order status：
                                             // 0 Initial Order, 1 New Order, 2 Done, 3 Partial, 4 Cancelled, 5 Unusual
      }
      "trade_list":[                         //  List of Order Details
          {
              "id":343,
              "created_at":"09-22 12:22",
              "price":222.33,               // Deal price
              "volume":222.33,              // Deal volume
              "deal_price":222.33,          //  Total deal amount
              "fee":222.33                  // Fee, the charge fee currency to deduct will corresponds to the volume
          },
          {
              "id":345,
              "created_at":"09-22 12:22",
              "price":222.33,
              "volume":222.33,
              "deal_price":222.33,
              "fee":222.33
          }
      ]
  }
}
```

### Creating Order

#### POST [/open/api/create_order](https://openapi.biki.cc/open/api/create_order)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632fee_is_user_exchange_coin0price9000sideBUYsymbolbtcusdttime1564133147type1volume1

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                  | Value Range
---------- | --------- | --------- | ---------------------------- | -----------------------------------------------------------------------------------------------------
api_key    | true      | string    | User api_key                 |
side       | true      | string    | BUY/SELL                     | BUY/SELL
type       | true      | string    | Order Type                   | 1 Limit Order, 2 Market Order
volume     | true      | string    | Purchase Quantity(Multiplex) | type=1: represents BUY/SELL volume, type=2: represents buy means total price, sell means total number
price      | false     | string    | Order price                  | type=2,ignore this parameter
symbol     | true      | string    | Symbol                       |
time       | true      | string    | Timestamp                    |
sign       | true      | string    | Signature                    |

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | -------------------------------
code       | string    | code=0 Success , code >0 Failed
order_id   | number    | Order ID

#### Return to example::

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "order_id":34343
    }
}
```

### Cancel Order

#### POST [/open/api/cancel_order](https://openapi.biki.cc/open/api/cancel_order)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632order_id39symbolbtcusdttime1564133229

#### Entry Parameters:

Parameters | Necessary | Data Typ | Description  | Value Range
---------- | --------- | -------- | ------------ | -----------
api_key    | true      | string   | User api_key |
order_id   | true      | number   | Order ID     |
symbol     | true      | string   | Symbol       |
time       | true      | string   | Timestamp    |
sign       | true      | string   | Signature    |

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": ""
}
```

### Cancel All Orders

#### POST [/open/api/cancel_order_all](https://openapi.biki.cc/open/api/cancel_order_all)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632symbolbtcusdttime1564133267

#### Entry Parameters:

Parameters | Necessary | Date Type | Description  | Value Range
---------- | --------- | --------- | ------------ | -----------
api_key    | true      | string    | User api_key |
symbol     | true      | string    | Symbol       |
time       | true      | string    | Timestamp    |
sign       | true      | string    | Signature    |

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": ""
}
```

### Batch Creating And Cancel Orders

#### POST [/open/api/mass_replaceV2](https://openapi.biki.cc/open/api/mass_replaceV2)

#### Example of Signing Request

api_keyd7e4d03168ac95fca79ffd60a9dc3632mass_cancel[24, 25]mass_place[{"side": "BUY", "type": "2", "volume": 1000}, {"side": "BUY", "type": "1", "volume": 1.5, "price": 10000}]symbolbtcusdttime1564133356

#### Entry Parameters:

Parameters  | Necessary | Data Type | Description
----------- | --------- | --------- | --------------------------------------------------------------------------------------------
api_key     | true      | string    | User api_key
symbol      | true      | string    | Symbol
mass_cancel | false     | string    | [1234,1235,...]
mass_place  | false     | string    | [{side:"BUY",type:"1",volume:"0.01",price:"6400",fee_is_user_exchange_coin:"0"}, {...}, ...]
time        | true      | string    | Timestamp
sign        | true      | string    | Signature

> mass_place Parameters<br>
> side：Direction（Buy/Sell Direction）<br>
> type：Type（1: Limit Order, 2: Market Order）<br>
> volume：Purchase Volume （Multiplex） type=1: represents BUY/SELL volume, type=2 represents buy means total price, sell means total number of<br>
> price：Order price：type=2：This parameter is not required

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
      "mass_place":[{"order_id":"1234","code":"0", "msg":"suc"},...],
      "mass_cancel":[{"order_id":"1234","code":"0", "msg":"suc"},...]
    }
}
```

<br>

## Spot Websocket API

### URL Access URL

#### [wss://ws.biki.cc/kline-api/ws]((https://www.biki.cc)) [Recommend]

#### [wss://ws.bikicoin.pro/kline-api/ws](https://www.biki.cc)

### Connection Request

#### Compressed Data

- All data returned by WebSocket API are compressed by GZIP so clients are required to unzip upon receipt of the data

#### Heartbeat Message

- When user's Websocket is connected to the Websocket Server, the Server will send periodic ping messages with a string of numbers as follows： {"ping": 156200607000}

- When user's Websocket receive such heartbeat message, please return a pong message with the same string of numbers： {"pong": 156200607000}

> If the Websocket Server does not receive any `pong` message after sending many `ping` messages, the Server will automatically disconnects from the user's Websocket

### Subscribe for Real-Time Trade Information

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_trade_ticker",
        "cb_id": "Customer"
    }
}
```

> Where $base$quote for trading information其中$base$quote<br>
> channel example "channel": "market_btcusdt_trade_ticker"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_trade_ticker",
    "cb_id": "Customer",
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new trading information available, the Server will auto send push messages as follows:

```python
{
    "channel":"market_$base$quote_trade_ticker", // Subscribed trading information channel
    "ts":1506584998239,                         // Request time
    "tick":{
        "id":12121,                             // the largest order ID in the data
        "ts":1506584998239,                     // the maximum time in the data
        "data":[
            {
                "id":12121,                     // trade ID
                "side":"buy",                   // Buy/Sell Direction
                "price":32.233,                 // Price
                "vol":232,                      // Volume
                "amount":323,                   // Total Amount
                "ts":1506584998239,             // Data create time
                "ds":'2017-09-10 23:12:21'
            },
            {
                "id":12120,                     // trade ID
                "side":"buy",                   // Buy/Sell Direction
                "price":32.233,                 // Price
                "vol":232,                      // Volume
                "amount":323,                   // Total Amount
                "ts":1506584998239,             // Data create time
                "ds":'2017-09-10 23:12:21'
            }
        ]
    }
}
```

### Subscribe for Depth Data

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_depth_step[0-2]",
        "cb_id": "Customer",
        "asks": 150,
        "bids": 150
    }
}
```

> $base$quote represents btcusdt and other trade pairs,step[0-2] represents depth with 3 dimension 0、1、2<br>
> channel example "channel": "market_btcusdt_depth_step0"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_depth_step[0-2]",
    "cb_id": "Customer",
    "asks": 150,
    "bids": 150,
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new information on depth data, the Server will auto send push messages:

```python
{
    "channel": "market_$base$quote_depth_step[0-2]",
    "ts": 1506584998239,
    "tick": {
        "asks": [
            [22112.22,0.9332],      // Price,Volume
            [22112.21,0.2]
        ],
        "buys": [
            [22111.22,0.9332],
            [22111.21,0.2]
        ]
    }
}
```

### Subscribe for Klines Date

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
        "cb_id": "Customer"
    }
}
```

> channel example "channel": "market_btcusdt_kline_1min"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
    "cb_id": "Customer",
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new information on Kline data, the Server will auto send push messages as follows:

```python
{
    "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
    "ts": 1506584998239,
    "tick": {
        "id": 1506602880,
        "amount": 123.1221,
        "vol": 1212.12211,
        "open": 2233.22,
        "close": 1221.11,
        "high": 22322.22,
        "low": 2321.22
    }
}
```

## Contract REST API

### Contract URL Access

- **<https://coapi.biki.cc> [Recommend]**

- **<https://coapi.biki.cc>**

### Get Contract List

#### GET [/swap/instruments](https://coapi.biki.cc/swap/instruments)

#### Entry Parameters: No

#### Return Parameters:

Parameters    | Data Typ | Description
------------- | -------- | ---------------------------------------------
errno         | string   |
message       | string   |
instrument_id | number   | instrument id
symbol        | string   | symbol
face_value    | string   | face value eg. BTCUSDT 0.0001BTC BTCUSD 1USDT
min_leverage  | string   | min leverage
max_leverage  | string   | max leverage
px_unit       | string   | Order price accuracy
qty_unit      | string   | Order quantity accuracy
margin_coin   | string   | Settlement currency

#### Return to example::

```python
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "instruments": [
            {
                "instrument_id": 1,
                "index_id": 1,
                "symbol": "BTCUSDT",
                "name_zh": "BTCUSDT永续合约",
                "name_en": "BTCUSDT Swap",
                "base_coin": "BTC",
                "quote_coin": "USDT",
                "margin_coin": "USDT",
                "face_value": "0.0001",
                "min_leverage": "1",
                "max_leverage": "125",
                "default_leverage": "0",
                "px_unit": "0.01",
                "qty_unit": "1",
                "value_unit": "0.0001",
                "min_qty": "1",
                "max_qty": "1000000",
                "maker_fee_ratio": "-0.0004",
                "taker_fee_ratio": "0.0006"
            },
            ...
        ]
    }
}
```

### Get Contract Tickers

#### GET [/swap/tickers](https://coapi.biki.cc/swap/tickers)

#### Entry Parameters: No

#### Return Parameters:

Parameters        | Data Type | Description
----------------- | --------- | ------------------------------------------------------------
instrument_id     | number    | instrument_id
symbol            | string    | symbol
last_px           | string    | last price
open              | string    | open price
high              | string    | Highest price
close             | string    | close price
low               | string    | 24H Low
change_value      | string    | change_value
change_rate       | string    | Change rate
position_size     | string    | Total open Stringerest
fair_px           | string    | fair price
amount24          | string    | 24H trade volume
base_coin_qty     | string    | trade volume on base coin BTCUSDT mean trade volume on BTC
quote_coin_qty    | string    | trade volume on quote coin BTCUSDT mean trade volume on USDT
timestamp         | number    | timestamp eg. 1534315695
next_funding_rate | string    |
next_funding_at   | string    |

#### Return to example:

```python
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "tickers": [
            {
                "symbol": "LTC/USDT",
                "last_px": "42.78",
                "open": "42.52",
                "close": "42.78",
                "low": "41.31",
                "high": "42.97",
                "avg_px": "42.3160293465642988",
                "last_qty": "11590",
                "qty24": "180609416",
                "timestamp": 1589377218,
                "change_rate": "0.0061147695202258",
                "change_value": "0.26",
                "qty_day": "165679224",
                "amount24": "76460525.17519999992197995",
                "instrument_id": 10,
                "position_size": "345121",
                "base_coin_qty": "1806094.16",
                "quote_coin_qty": "76426733.477218396127175008",
                "pps": "89098",
                "index_px": "42.786666666666",
                "fair_px": "42.802536113866",
                "depth_px": {
                    "bid_px": "42.76",
                    "ask_px": "42.8",
                    "mid_px": "42.7825"
                },
                "fair_basis": "0",
                "fair_value": "0",
                "madb": "0.0158694472",
                "rate": {
                    "quote_rate": "0.0006",
                    "base_rate": "0.0003",
                    "interest_rate": "0.000099999999"
                },
                "premium_index": "-0.000003855618",
                "funding_rate": "0.000093526743",
                "next_funding_at": "2020-05-13T16:00:00Z"
            },
            {
                "last_px": "188.29",
                "open": "187.47",
                "close": "188.29",
                "low": "184",
                "high": "190.76",
                "avg_px": "187.7633920084560113",
                "last_qty": "4000",
                "qty24": "194194490",
                "timestamp": 1589377221,
                "change_rate": "0.0043740331786419",
                "change_value": "0.82",
                "qty_day": "179246664",
                "amount24": "36459911.6556200000053700714",
                "symbol": "BSV/USDT",
                "instrument_id": 9,
                "position_size": "685623",
                "base_coin_qty": "194194.49",
                "quote_coin_qty": "36462616.151752190801837737",
                "pps": "96563",
                "index_px": "188.337277777777",
                "fair_px": "188.331303030303",
                "depth_px": {
                    "bid_px": "188.17",
                    "ask_px": "188.47",
                    "mid_px": "188.345"
                },
                "fair_basis": "0",
                "fair_value": "0",
                "madb": "-0.005974747474",
                "rate": {
                    "quote_rate": "0.0006",
                    "base_rate": "0.0003",
                    "interest_rate": "0.000099999999"
                },
                "premium_index": "0.000099916176",
                "funding_rate": "0.000099916176",
                "next_funding_at": "2020-05-13T16:00:00Z"
            },
            ...
        ]
    }
}
```

### Get Contract Trades

#### GET [/swap/trades](https://coapi.biki.cc/swap/trades?instrumentID=1)

#### Entry Parameters:

Parameters    | Data Type | Description
------------- | --------- | -------------
instrument_id | number    | instrument_id

#### Return Parameters:

Parameters    | Data Type | Description
------------- | --------- | --------------------------------------
instrument_id | string    | instrument_id
oid           | string    | taker order id
tid           | string    | trade id
px            | string    | trade price
qty           | string    | trade quantity
make_fee      | string    | make_fee
take_fee      | string    | take_fee
side          | string    | 1~4 buyer as taker 5~8 seller as taker

#### Return to example:

```python
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "trades": [
            {
                "tid": 2119605783,
                "oid": 2119605772,
                "instrument_id": 1,
                "px": "9128",
                "qty": "3900",
                "make_fee": "-1.423968",
                "take_fee": "2.135952",
                "created_at": "2020-05-13T14:17:37.524095113Z",
                "side": 4,
                "change": "0"
            },
            {
                "tid": 2119603181,
                "oid": 2119603178,
                "instrument_id": 1,
                "px": "9130.6",
                "qty": "2500",
                "make_fee": "-0.91306",
                "take_fee": "1.36959",
                "created_at": "2020-05-13T14:17:31.629972954Z",
                "side": 5,
                "change": "0"
            }
        ]
    }
}
```

### Get Contract Order book

#### GET [/swap/depth](https://coapi.biki.cc/swap/depth?instrumentID=1&count=10)

#### Entry Parameters:

Parameters    | Data Type | Description
------------- | --------- | -------------
instrument_id | number    | instrument_id
count         | number    | count

#### Return to example:

```python
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "asks": [
            [
                911400,
                "9114",   # price
                "134716", # contract size at the price
                0
            ],
            [
                911430,
                "9114.3",
                "36647",
                0
            ]
        ],
        "bids": [
            [
                911350,
                "9113.5",
                "41906",
                0
            ],
            [
                911340,
                "9113.4",
                "33348",
                0
            ]
        ]
    }
}
```

### Get Contract Market Data

#### GET [/swap/kline](https://coapi.biki.cc/swap/kline?instrumentID=1&startTime=1583300827&endTime=1583325827656&unit=5&resolution=M)

#### Entry Parameters:

Parameters    | Data Type | Description
------------- | --------- | ----------------------
instrument_id | number    | instrument_id
startDate     | number    | startDate
endDate       | number    | endDate
unit          | number    | granularity
resolution    | number    | granularity Unit M/H/D

#### Return to example:

```python
{
    "errno": "OK",
    "message": "Success",
    "data": [
        {
            "low": "6822.8",
            "high": "6911.7",
            "open": "6911.5",
            "close": "6835.9",
            "last_px": "6835.9",
            "avg_px": "6863.2959679229457036",
            "qty": "769480",
            "timestamp": 1586736001,
            "change_rate": "-0.0109382912537076",
            "change_value": "-75.6",
            "base_coin_qty": "76.948",
            "quote_coin_qty": "528116.898139734826"
        },
        {
            "low": "6835.6",
            "high": "6862.7",
            "open": "6835.9",
            "close": "6862.6",
            "last_px": "6862.6",
            "avg_px": "6850.140409757800756",
            "qty": "728032",
            "timestamp": 1586736301,
            "change_rate": "0.0039058499978057",
            "change_value": "26.7",
            "base_coin_qty": "72.8032",
            "quote_coin_qty": "498712.14227967912"
        }
    ]
}
```

[^_^]: aadf4
