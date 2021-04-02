AEX RESTful API Protocol Documentation (V3)
---

[中文版](/中文.md)

# table of Contents
+ [public data interface](#request--response)
   + [1. Get transaction vs. market data](#1-get-transaction-to-market-data)
   + [2. Get Transaction vs. Depth Data](#2-get-transaction-versus-depth-data)
   + [3. Get transaction to historical transaction data](#3-get-the-transaction-to-historical-transaction-data)
   + [4. Get all market pair information](#4-get-all-market-pair-information)
   + [5. Get specified market pair information](#5-get-specified-market-pair-information)
   + [6. Get k Kline Data](#6-get-k-kline-data)
+ [User Data Interface](#user-data-nterface)
   + [7. My Account Balance](#7-my-account-balance)
   + [8. Pending Order](#8-pending-order)
   + [9. Withdrawal](#9-withdrawal)
   + [10. My Pending Order](#10-my-pending-order)
   + [11. My Transaction Record](#11-my-transaction-record)
   + [12. Query my pending order via Tag](#12-query-my-pending-order-via-tag)
   + [13. Query my pending order via tab_id](#13-query-my-pending-order-via-tab_id)
   + [14. Query my transaction record by Tag](#14-query-my-transaction-record-by-tag)
   + [15. Query my transaction record by tab_id (flow id)](#15-query-my-transaction-record-by-tab_id)
   + [16. Get Transaction Configuration](#16-get-transaction-configuration)
   + [17. Get my cancellation record](#17-get-my-cancellation-record)
   + [18. Get my order life cycle](#18-get-my-order-life-cycle)
+ [FAQ](#faq)
   + [api release notes](#api-release-notes)
   + [Request and Response Description](#request-and-answer-description)


# request / answer
## 1. Get transaction to market data

  Request URL
  ```
  POST /v3/ticker.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  mk_type | trading area, such as cnc
  coinname| coin name, for example: coinname=btc, get the market data of the btc/cnc transaction pair; coinname=all, get the market data of all valid transaction pairs under the cnc trading area

  Normal response (json)
  ```
  {
        "code": 20000,
        "msg":"",
        "data":
            Btc/cnc trading on market data:
            {
            "ticker":{
              "high": 85701,
              "low": 78527,
              "last": 81538,
              "last24": 81538, //The first price in the last 24 hours
              "vol": 4775.686371,
              "money": 444775.686371, //Last 24 hours turnover
              "buy": 81434,
              "sell": 81537,
              "range": -0.017
            }
            }
            ```
            ```
            All trading data for the market under the cnc trading area:
            {
            "gat":{
                "ticker":{
                    "high": 0.0158,
                    "low": 0.015,
                    "last": 0.01529,
                    "last24":0.01529,
                    "vol": 352898167.69075,
                    "money": 32898167.69075,
                    "buy": 0.01527,
                    "sell": 0.01532,
                    "range": -0.0077
                }
            },
            "btc":{
                "ticker":{
                    "high": 85701,
                    "low": 78527,
                    "last": 81445,
                    "last24": 81445,
                    "vol":4775.959156,
                    "money":34775.959156,
                    "buy": 81417,
                    "sell": 81448,
                    "range": -0.0177
                }
            },
            ...
            }
  }

  ```
  ### Error response (error code)
  Reference request and response instructions
## 2. Get transaction versus depth data

  Request URL
  ```
  POST /v3/depth.php
  ```
  Request parameters:

  parameter name | description
  ----- | ---------
  mk_type | trading area, such as cnc
  coinname | coin name, for example: coinname=btc, get the market data of the btc/cnc depth pair

  Normal response (json)
  ```
  {
      "code": 20000,
      "msg":"",
      "data":
         {
           "bids":[
             [
                 81289,
                 0.0256
             ],
             ...
           ],
           "asks":[
             [
                 81324,
                 0.019146
             ],
             ...
           ]
         }
  }
  ```
  ### Error response (error code)
  Reference request and response instructions
## 3. Get the transaction to historical transaction data

  Request URL
  ```
  POST /v3/trades.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  mk_type | trading area, such as cnc
  coinname | coin name, for example: coinname =btc, get the market data of the btc/cnc trade pair; coinname=all
  fromid | Start from which transaction ID query (optional parameter, without this parameter, the default query from the first)
  limit | the number of returns returned by the query
  
  Normal response (json)
  ```
   {
        "code": 20000,
        "msg":"",
        "data":
         [
           {
               "date": 1565160467,
               "price": 81238,
               "amount": 0.204957,
               "tradeid": 13354117,
               "type": "sell"
           },
           ...
         ]
    }
  ```
  ### Error response (error code)
  Reference request and response instructions
## 4. Get all market pair information

  Request URL
  ```
  GET /v3/allpair.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
 
  
  Normal response (json)
  ```
   {
     "eno": 0, 
     "emsg":"", 
     "data":[
       {
         "market":"usdt", 
         "coin": "btc", 
         "limits":{
           "PricePrecision": 0, 
           "AmtPrecision": 6, // the number of pending orders
           "AmtMax": "10000000.00000000", 
           "AmtMin": "0.00000100", 
           "PriceMax": "1000000.00000000", 
           "MoneyMin": "1.00000000" // Minimum pending order <= Pending order price* Pending order quantity
         }
       }
     ]
   }
  ```
  ### Error response (error code)
  Reference request and response instructions
## 5. Get specified market pair information

  Request URL
  ```
  GET /v3/pair.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  market | trading area, such as cnc
  coin | currency name, such as btc
  
  Normal response (json)
  ```
  {
    "eno": 0, 
    "emsg":"", 
    "data":[
      {
        "market":"usdt", 
        "coin": "btc", 
        "limits":{
          "PricePrecision": 0, 
          "AmtPrecision": 6, // the number of pending orders
          "AmtMax": "10000000.00000000", 
          "AmtMin": "0.00000100", 
          "PriceMax": "1000000.00000000", 
          "MoneyMin": "1.00000000" // Minimum pending order <= Pending order price* Pending order quantity
        }
      }
    ]
  }
  ```
  ### Error response (error code)
  Reference request and response instructions
## 6. Get k kline Data

  Request URL
  ```
  GET /v3/kLine.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  market | trading area, such as cnc
  coin | currency name, such as btc
  cycle| k kline period, such as 5min
  begin_time|start time, such as 1608789000(time stamp)
  end_time|end time, such as 1608790200 (time stamp)
  
  Optional time period
      ```
      '1min' => '1 min',
      '5min' => '5 min',
      '15min' => '15 min',
      '30min' => '30 min'
      '1hour' => '1 hour',
      '4hour' => '4 hour',
      '1day' => 'one day',
      '5day' => 'five day',
      '1wek' => 'one week',
      '1mon' => 'one month ',
      ```
  Normal response (json)
  ```
   {
        "code": 20000,
        "msg":"",
        "data":
         [
           {
               t: 1608789000,     /K-line opening time
               v: "0.25671818",   //Number of transactions
               o: "150191",       //Opening price
               h: "150191",       //High price
               l: "150122",       //Low price
               c: "150154"        //The latest price or closing price
           },
           ...
         ]
    }
  ```
  ### Error response (error code)
  Reference request and response instructions      
## 7. My account balance

  Request URL
  ```
  POST /v3/getMyBalance.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account


  Normal response (json)
  ```
   {
        "code": 20000,
        "msg":"",
        "data":{
                   "btc_balance":"0.00022878",
                   "btc_balance_lock": "0.00000000",
                   "ltc_balance": "0",
                   "ltc_balance_lock": "0",
                   ...
                 }
    }

  ```
  ### Error response (error code)
  Reference request and response instructions
## 8. Pending order

  Request URL
  ```
  POST /v3/submitOrder.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as btc
  type | Pending order type: 1=Pending order, 2=Pending order
  price | pending order price
  amount | number of pending orders
  tag | custom tag (optional, default is 0), decimal, no more than 9 positive integers, can be used to associate pending orders and transaction records
  time_in_force | Order timeliness ,int (optional, default is 1) 2:make only


  Normal response: When you place an order, you can complete the combination (string).
  ```
  {
      "code": 220010,
      "msg": "add order succ",
      "data":{"tab_id":24022502}
  }
  ```
  ### Error response (error code)
  Reference request and response instructions
## 9. Withdrawal

  Request URL
  ```
  POST /v3/cancelOrder.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as btc
  order_id | 
  tab_id | 
  tag | order tag // order_id, tab_id, tag can only pass one of them


  Normal response (string)
  ```
  {
      "code": 220020,
      "msg": "cancel order succ",
      "data":[]
  }
  ```

  ### Error response (error code)
  Reference request and response instructions
## 10. My pending order

  Request URL
  ```
  POST /v3/getOrderList.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat


  Reply, gat/cnc transaction pair (json)
  ```
  {
    "code": 20000,
    "msg":"",
    "data":
    [
        {
          "order_id": "3189572",
          "coinname": "gat",
          "tab_id":123 //flow id
          "type": "1",
          "price": "0.01530000",
          "amount": "757.87633900",
          "time": "2019-08-07 15:27:11"
        }
    ]
  }
  ```

  ### Error response (error code)
  Reference request and response instructions
## 11. My transaction record

  Request URL
  ```
  POST /v3/getMyTradeList.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  page | Get the first few pages of records (optional parameters), without the page parameter, the server default page is 0, that is, query the data of the first page


  Reply, gat/cnc transaction pair (json)
  ```
   {
      "code": 20000,
      "msg":"",
      "data":
      [
          {
            "trade_id": "3189572",
            "coinname": "gat",
            "type": "1",
            'tab_id': 100
            "price": "0.01530000",
            "amount": "757.87633900",
            "time": "2019-08-07 15:27:11"
          }
      ]
   }
  ```

  ### Error response (error code)
  Reference request and response instructions
## 12. Query my pending order via Tag

  Request URL
  ```
  POST /v3/getMyOrdersByTag.php
  ```

  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  tag | To query what tag order, request tag>=1, where the tag is the tag in the submitOrder.php request
  since_order_id | From which order ID to start querying (optional), require since_order_id>=1, without this parameter, start from the first order by default.


  Reply, gat/cnc transaction pair (json)
  ```
    {
        "code": 20000,
        "msg":"",
        "data":
        {
           "mk_type":"cnc",
           "coin": "gat",
           "tag": 23,
           "orders":[
             {
               "order_id": 3190716,
               "type": 1,
               "tab_id": 100
               "price": 0.01535,
               "amount": 755.407687,
               "time": "2019-08-07 16:10:30"
             },
             ...
           ]
         }
    }

  ```

  ### Error response (error code)
  Reference request and response instructions
## 13. Query my pending order via tab_id

  Request URL
  ```
  POST /v3/getMyOrdersByTabId.php
  ```

  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  tab_id | To query a pipelined order, require tab_id>=1, where tab_id is the tab_id returned in getOrderList.php


  Reply, gat/cnc transaction pair (json)
  ```
    {
        "code": 20000,
        "msg":"",
        "data":
        {
           "mk_type":"cnc",
           "coin": "gat",
           "tag": 23,
           "orders":[
             {
               "order_id": 3190716,
               "type": 1,
               "tab_id": 100
               "price": 0.01535,
               "amount": 755.407687,
               "time": "2019-08-07 16:10:30"
             },
             ...
           ]
         }
    }

  ```

  ### Error response (error code)
  Reference request and response instructions
## 14. Query my transaction record by tag

  Request URL
  ```
  POST /v3/getMyTradesByTag.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  tag | To query what tag order, request tag>=1, where the tag is the tag in the submitOrder.php request
  since_trade_id | From which transaction ID is started (optional), since_trade_id>=1, without this parameter, the query starts from the first transaction record by default.


  Reply, gat/cnc transaction pair (json)
  ```
   {
      "code": 20000,
      "msg":"",
      "data":
      {
         "mk_type":"cnc",
         "coin": "gat",
         "tag": 25,
         "trades":[
           {
             "trade_id": 1423223,
             "type": 1,
             "tab_id": 100,
             "price": 0.01541,
             "volume": 65.541856,
             "fee": 0.06554185,
             "time":"2019-08-07 16:24:14"
           },
           {
             "trade_id": 1423224,
             "type": 1,
             "tab_id": 100,
             "price": 0.01541,
             "volume": 686.924594,
             "fee": 0.61922016,
             "time":"2019-08-07 16:24:14"
           }
         ]
       }
   }
  ```
## 15. Query my transaction record by tab_id

  Request URL
  ```
  POST /v3/getMyTradesByTabId.php
  ```
  Request parameters:

  Parameter name | Description
  ----- | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  tab_id | To query a pipelined order, require tab_id>=1, where tab_id is the tab_id returned in getOrderList.php
  since_trade_id | From which transaction ID is started (optional), since_trade_id>=1, without this parameter, the query starts from the first transaction record by default.


  Reply, gat/cnc transaction pair (json)
  ```
   {
      "code": 20000,
      "msg":"",
      "data":
      {
         "mk_type":"cnc",
         "coin": "gat",
         "tab_id": 25,
         "trades":[
           {
             "trade_id": 1423223,
             "type": 1,
             "tab_id": 25
             "price": 0.01541,
             "volume": 65.541856,
             "fee": 0.06554185,
             "time":"2019-08-07 16:24:14"
           },
           {
             "trade_id": 1423224,
             "type": 1,
             "tab_id": 25
             "price": 0.01541,
             "volume": 686.924594,
             "fee": 0.61922016,
             "time":"2019-08-07 16:24:14"
           }
         ]
       }
   }
  ```
## 16. Get transaction configuration

    Request URL
    ```
    GET /v3/tradeconf.php
    ```

  Request parameters:

  Parameter name | Description
  ----- | ---------


    Answer (json)
    ```
     {
         "code": 20000,
         "msg": "",
         "data": {

             "cnc": {
                 "gat": {
                     "conf_service_id": 0
                 },
                 "btc": {
                     "conf_service_id": 1
                 },
                 "eth": {
                     "conf_service_id": 0
                 }
             }

         }
     }
    ```

  ### Error response (error code)
    Reference request and response instructions
## 17. Get my cancellation record
      
Request URL
```
POST /v3/getCancelOrderList.php
```
    
Parameter name | Description
-----  | ---------
key | public key
time | UNIX timestamp when the request is initiated, in seconds, not milliseconds
mD5 | authentication MD5, MD5 = MD5 ("{key} \ {user {ID} \ {skey} \ {time}"), user {ID is the number ID after the user logs in, not the email account
mk_type| trading area, such as CNC Coinname, such as gat
page | page number starts from 1 by default, page size is 10 by default, and cannot be changed
order_iD | order ID is not a required parameter
tab_iD | pipeline ID is not a required parameter
    
        
Answer（json）
```
{
"code": 20000,
"msg": "",
"data": {
      "res": [
               {
                  "id": 593,
                  "tab_id": 24022539,
                  "user_id": 387712,
                  "order_id": 25275,
                  "coin": "b360",
                  "market": "cnc",
                  "order_time": "2019-11-04 14:50:11",
                  "status": 0,
                  "order_type": 1,
                  "reset_amount": "1.49178000",
                  "reset_coin": "cnc"
                     }
                 ],
                 "current_page": 1,
                 "total_num": 0,
                 "is_more": 0,
                 "total_page": 1
             }
         }
``` 
## 18. Get my order life cycle 

  Request URL
  ```
    POST /v3/orderLifeCycle.php
  ```
  Request parameters:
  Parameter name | Description
  -----  | ---------
  key | public key
  time | Unix timestamp when the request was initiated, in seconds, not milliseconds
  md5 | authentication md5, md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id is the numeric ID of the user after login, not the email account
  mk_type | trading area, such as cnc
  coinname | currency name, such as gat
  tab_id | To query a pipelined order, require tab_id>=1, where tab_id is the tab_id returned in getOrderList.php
  
  Reply, gat/cnc transaction pair（json）
  ```
   {
   	"code": 20000,
   	"msg": "",
   	"data": {
   		"mk_type": "cnc",
   		"coin": "btc",
   		"tab_id": 38506249,
   		"order_log": [{
   			"price": "185.00000000",
   			"amount": "109.000000",
   			"type": "sell",
   			"time": "2020-05-18 16:29:19",
   			"tab_id": 38506249,
   			"status": 2,
   			"in_force": 1
   		}],
   		"order": [{
   			"order_id": 1598,
   			"type": "buy",
   			"price": "10.00000000",
   			"amount": "10.00000000",
   			"amount_origin": "10.00000000",
   			"tab_id": 38506259,
   			"time": "2020-05-18 20:10:45",
   			"tag": 0
   		}],
   		"trade": [{
   			"amount": "1.00000000",
   			"price": "185.00000000",
   			"time": "2020-05-18 16:29:19",
   			"fee": "6.86990504",
   			"fee_coin": "gat",
   			"tag": 0,
   			"exec_type": "t",
   			"trade_id": 1503,
   			"tab_id": 38506249,
   			"type": "sell",
   			"match": "other"
   		}],
   		"cancel_order": [{
   			"order_id": 1592,
   			"time": "2020-05-18 16:31:57",
   			"status": 0,
   			"tab_id": 38506249,
   			"type": "sell",
   			"re_amount": "108.00000000",
   			"re_coin": "btc"
   		}]
   	}
   }
    
  ```      

      
### Wrong answer(error code)
Refer to the request and response instructions
        
# FAQ
## api release notes
  ```
  1) The changes in this version are collectively referred to as v3. If the interface is not found in v3, please look in the old version
  2) The new version is basically compatible with the old version
  3) Whether to use the new version of the transaction is determined by the configuration, developers can not freely switch between the old and new versions
  4) When the transaction configuration conf_service_id is greater than 0, it is the v3 version; otherwise, it is the old version.
  5) This version of the pending order and the withdrawal order are returned asynchronously. The pending return is successful, but it represents the process of entering the match; the withdrawal of the order is successful, but the representative accepts the request to withdraw the order, and does not mean that the order must be withdrawn. Whether the transaction is successful or not, you need to use other interfaces to judge (my pending order and my transaction record).

  ```
## Request and Answer Description
  ```
    1) response public field description; code: program code; msg: return error information; data: successfully returned field
    2) Relationship description; code is an integer, the highest is an odd number represents a program error, and when the highest bit is an even number, the program succeeds; when the program succeeds, msg is generally empty, and vice versa, the data is generally empty when the program fails. Whether the program is successful cannot be judged by using msg and data as empty. This is only a reference condition, and the condition is the highest parity of the code.
    3) code error and msg description: different codes usually represent different meanings, but some development errors use a unified error code, then you need to refer to the information in msg or data.
  ```

 The meaning of the specific representation of code:

Code | Description |msg(data)-zh |msg(data)-en
:-----:| :------------:|:------------: |:-----:
20000 | Program Success | |
220010 | Order success | |
220020 | Successful withdrawal | |
50004 | Required parameters are empty | |
10002 | Processing failed, default | |
110046 | Trading area does not exist | |
110047 | Order Type Error | |
110054 | Price fluctuations are too large | |
110055 | Insufficient order amount | |
110056 | Trading currency does not exist | |
110057 | Accuracy does not exist | |
110058 | Unopened transaction | |
110059 | Pending order price can't be less than or equal to 0 |
110070 | The accuracy of the configuration cannot exceed 8 bits |
110060 | The accuracy of the configuration cannot be less than 0 |
110061 | Price accuracy can't exceed 8 digits |
110062 | Price accuracy can't be less than 0 | |
110063 | The quantity accuracy of the pending order cannot exceed the configured accuracy |
110064 | Quantity cannot exceed the configured maximum limit | |
110065 | The order quantity cannot be less than the configured minimum limit | |
110067 | Transaction price accuracy cannot exceed configuration accuracy | |
110066 | Pending order price cannot exceed the configured maximum limit | |
110075 | The total amount of the transaction cannot be less than the configured amount |
120071 | No last price found | |
110068 | Failed to insert pending order log |
110069 | Reduce User Currency Failure | |
110073 | No order exists | |
110071 | Already have been withdrawn | |
110072 | Failed to join the withdrawal log | |
110080 | This transaction can only be in V1 version ||
1008000 | This transaction can only be in V1 version ||
