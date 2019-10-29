AEX RESTful API 协议说明文档 （V3）
---

# 目录
+ [公开数据接口](#请求应答)
   + [1. 获取交易对行情数据](#1-获取交易对行情数据), [\[错误码\]](#错误应答错误码)
   + [2. 获取交易对深度数据](#2-获取交易对深度数据), [\[错误码\]](#错误应答错误码-1)
   + [3. 获取交易对历史成交数据](#3-获取交易对历史成交数据), [\[错误码\]](#错误应答错误码-2)
+ [用户数据接口](#4-我的账户余额)
   + [4. 我的账户余额](#4-我的账户余额), [\[错误码\]](#错误应答错误码-3)
   + [5. 挂单](#5-挂单), [\[错误码\]](#错误应答错误码-4)
   + [6. 撤单](#6-撤单), [\[错误码\]](#错误应答错误码-5)
   + [7. 我的挂单](#7-我的挂单), [\[错误码\]](#错误应答错误码-6)
   + [8. 我的成交记录](#8-我的成交记录), [\[错误码\]](#错误应答错误码-7)
   + [9. 通过Tag查询我的挂单](#9-通过tag查询我的挂单), [\[错误码\]](#错误应答错误码-8)
   + [10. 通过Tag查询我的成交记录](#10-通过tag查询我的成交记录), [\[错误码\]](#错误应答错误码-9)
   + [11. 获取交易配置](#10-获取交易配置), [\[错误码\]](#错误应答错误码-10)
+ [FAQ](#faq)
   + [api版本说明](#api版本说明)
   + [请求与应答说明](#请求与应答说明)
   

# 请求/应答
## 1. 获取交易对行情数据   

  请求URL
  ```
  POST /v3/ticker.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  coinname| 币名，比如: coinname=btc, 获取btc/cnc交易对的行情数据; coinname=all, 获取cnc交易区下所有有效交易对的行情数据
  
  正常应答（json）
  ```
  {
        "code":20000,
        "msg":"",
        "data":
            btc/cnc 交易对行情数据：
            {
            "ticker":{
              "high":85701,
              "low":78527,
              "last":81538,
              "vol":4775.686371,
              "buy":81434,
              "sell":81537,
              "range":-0.017
            }
            }
            ```
            ```
            cnc 交易区下所有交易对行情数据:
            {
            "gat":{
                "ticker":{
                    "high":0.0158,
                    "low":0.015,
                    "last":0.01529,
                    "vol":352898167.69075,
                    "buy":0.01527,
                    "sell":0.01532,
                    "range":-0.0077
                }
            },
            "btc":{
                "ticker":{
                    "high":85701,
                    "low":78527,
                    "last":81445,
                    "vol":4775.959156,
                    "buy":81417,
                    "sell":81448,
                    "range":-0.0177
                }
            },
            ...
            }
  }
  
  ```
  ### 错误应答(错误码)
  参考请求和应答说明
  
  
## 2. 获取交易对深度数据   

  请求URL
  ```
  POST /v3/depth.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如: coinname=btc, 获取btc/cnc交易对的行情数据; coinname=all, 获取cnc交易区下所有有效交易对的行情数据
  
  正常应答（json）
  ```
  {
      "code":20000,
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
  ### 错误应答(错误码)
  参考请求和应答说明
  
  
## 3. 获取交易对历史成交数据   

  请求URL
  ```
  POST /v3/trades.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如: coinname =btc, 获取btc/cnc交易对的行情数据; coinname=all, 获取cnc交易区下所有有效交易对的行情数据
  fromid | 从哪个成交ID开始查询（可选参数，不带这个参数默认从第一条开始查询）
  limit | 查询返回条数
  
  正常应答（json）
  ```
   {
        "code":20000,
        "msg":"",
        "data":
         [
           {
               "date":1565160467,
               "price":81238,
               "amount":0.204957,
               "tid":13354117,
               "type":"sell"
           },
           ...
         ] 
    }
  ```   
  ### 错误应答(错误码)
  参考请求和应答说明
  
## 4. 我的账户余额   

  请求URL
  ```
  POST /v3/getMyBalance.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  
  
  正常应答（json）
  ```
   {
        "code":20000,
        "msg":"",
        "data":{
                   "btc_balance":"0.00022878",
                   "btc_balance_lock":"0.00000000",
                   "ltc_balance":"0",
                   "ltc_balance_lock":"0",
                   ...
                 }
    }
  
  ```    
  ### 错误应答(错误码)
  参考请求和应答说明
  
## 5. 挂单   

  请求URL
  ```
  POST /v3/submitOrder.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 btc
  type | 挂单类型: 1=挂买单，2=挂卖单
  price | 挂单价格
  amount | 挂单数量
  tag | 自定义标签（可选，默认为0），十进制，不超过10位正整数，可以用来关联挂单和成交记录
  
  
  正常应答: 下单时完全撮合成交（string）
  ```
  {
      "code":220010,
      "msg":"add order succ",
      "data":[]
  }
  ``` 
  ### 错误应答(错误码)
  参考请求和应答说明
  
  
## 6. 撤单   

  请求URL
  ```
  POST /v3/cancelOrder.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 btc
  order_id | 订ID
  
  
  正常应答（string）
  ```
  {
      "code":220020,
      "msg":"cancel order succ",
      "data":[]
  }
  ```      
  
  ### 错误应答(错误码)
  参考请求和应答说明
  
  
## 7. 我的挂单   

  请求URL
  ```
  POST /v3/getOrderList.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 gat
  
  
  应答, gat/cnc交易对（json）
  ```
  {
    "code":20000,
    "msg":"",
    "data":
    [
        {
          "id":"3189572",
          "coinname":"gat",
          "type":"1",
          "price":"0.01530000",
          "amount":"757.87633900",
          "time":"2019-08-07 15:27:11"
        }
    ]
  }
  ```      
  
  ### 错误应答(错误码)
  参考请求和应答说明

  
## 8. 我的成交记录   

  请求URL
  ```
  POST /v3/getMyTradeList.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 gat
  page | 获取第几页记录（可选参数）, 不带page参数，服务器默认page是0，即查询第1页的数据
  
  
  应答, gat/cnc交易对（json）
  ```
   {
      "code":20000,
      "msg":"",
      "data":
      [
          {
            "id":"3189572",
            "coinname":"gat",
            "type":"1",
            "price":"0.01530000",
            "amount":"757.87633900",
            "time":"2019-08-07 15:27:11"
          }
      ]
   }
  ```      
  
  ### 错误应答(错误码)
  参考请求和应答说明


## 9. 通过Tag查询我的挂单   

  请求URL
  ```
  POST /v3/getMyOrdersByTag.php
  ```
  
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 gat
  tag | 要查询什么tag的订单，要求tag>=1，这里的tag是submitOrder.php请求中的tag
  since_order_id | 从哪个订单ID开始查询（可选），要求since_order_id>=1，不带该参数默认从第一条订单开始查询
  
  
  应答, gat/cnc交易对（json）
  ```
    {
        "code":20000,
        "msg":"",
        "data":
        {
           "mk_type":"cnc",
           "coin":"gat",
           "tag":23,
           "orders":[
             {
               "id":3190716,
               "type":1,
               "price":0.01535,
               "amount":755.407687,
               "time":"2019-08-07 16:10:30"
             },
             ...
           ]
         }
    }
    
  ```      
  
  ### 错误应答(错误码)
  参考请求和应答说明

  
## 10. 通过Tag查询我的成交记录   

  请求URL
  ```
  POST /v3/getMyTradesByTag.php
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  key  | 公钥
  time | 发起请求时的Unix时间戳，单位秒，不是毫秒
  md5  | 鉴权md5，md5=md5("{key}\_{user_id}\_{skey}\_{time}"), user_id是用户登录后的数字ID，不是邮箱账号
  mk_type | 交易区，比如 cnc
  coinname | 币名，比如 gat
  tag | 要查询什么tag的订单，要求tag>=1，这里的tag是submitOrder.php请求中的tag
  since_trade_id | 从哪个成交ID开始查询（可选），since_trade_id>=1，不带该参数默认从第一条成交记录开始查询
  
  
  应答, gat/cnc交易对（json）
  ```
   {
      "code":20000,
      "msg":"",
      "data":  
      {
         "mk_type":"cnc",
         "coin":"gat",
         "tag":25,
         "trades":[
           {
             "id":1423223,
             "type":1,
             "price":0.01541,
             "volume":65.541856,
             "fee":0.06554185,
             "time":"2019-08-07 16:24:14"
           },
           {
             "id":1423224,
             "type":1,
             "price":0.01541,
             "volume":686.924594,
             "fee":0.61922016,
             "time":"2019-08-07 16:24:14"
           }
         ]
       }
   }
  ```      
  
  ## 11. 获取交易配置   
  
    请求URL
    ```
    GET /v3/tradeconf.php
    ```

  请求参数:   
  
  参数名  | 说明
  -----  | ---------

    
    应答（json）
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
  
  ### 错误应答(错误码)
    参考请求和应答说明

# FAQ
## api版本说明
  ```
  1）这版的改动统称为v3。如果在v3没有找到的接口， 请在老版本查找
  2）新版本基本兼容旧版本
  3）是否使用新版交易是有配置确定的，开发者不能自由切换新旧版本
  4）获取交易配置conf_service_id大于0时，为v3版本；反之，为旧版本
  5) 这个版本挂单和撤单都是异步返回的。挂单返回成功，只是代表进入撮合过程；撤单返回成功，只是代表撮合接受到了撤单请求，并不代表一定撤单。是否成交和是否撤单成功需要利用其它接口来判断（我的挂单和我的成交记录）。
  
  ```
## 请求与应答说明
  ```
    1）应答公共字段说明；code：程序代码；msg：返回错误的信息；data：成功返回的字段
    2）关系说明；code是一个整数，最高为是奇数时代表程序出错，反之最高位是偶数时，程序成功；当程序成功时msg一般为空，反之，程序失败时data一般为空。程序是否成功不能用msg和data为空来判定，这只是参考条件，决定条件是code的最高位的奇偶。
    3）code错误和msg说明：不同的code通常代表不同的意思，不过有些开发的报错用的是统一的报错code，这时需要参考一下msg或者data里面的信息
  ```
  
 code具体代表的意义:
 
    code |     说明   |msg(data)-zh |msg(data)-en
    -----| ------------|------------ |----- 
    20000  |     程序成功 |             |
    220010 |     下单成功 |             |
    220020 |     撤单成功 |             |
    50004  |     必填参数为空|             |
    10002  |     处理失败，默认的|             |
    110046 |     交易区不存在 |             |
    110047 |     订单类型错误 |             |
    110054 |     价格波动过大 |             |
    110055 |     下单金额不足 |             |
    110056 |     交易币不存在 |             |
    110057 |     精度不存在 |             |
    110058 |     未打开交易 |             |
    110059 |     挂单价格不能小于等于0 |             |
    110070 |     配置的精度不能超过8位 |             |
    110060 |     配置的精度不能小于0 |             |
    110061 |     价格精度不能超过8位 |             |
    110062 |     价格精度不能小于0 |             |
    110063 |     挂单数量精度不能超过配置的精度 |             |
    110064 |     数量不能超过配置的最大限制 |             |
    110065 |     订单数量不能小于配置的最小限制 |             |
    110067 |     交易价格精度不能超过配置精度 |             |
    110066 |     挂单价格不能超过配置的最大限制 |             |
    110075 |     交易总金额不能小于配置的金额 |             |
    120071 |     未找到最后价格 |             |
    110068 |     插入挂单日志失败 |             |
    110069 |     减少用户币失败 |             |
    110073 |     不存在订单 |             |
    110071 |     已经存在撤单 |             |
    110072 |     加入撤单日志失败 |             |
      
      