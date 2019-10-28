AEX RESTful API 协议说明文档 （V1）
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
+ [FAQ](#faq)
   + ["param time should be a timestamp"错误要怎么处理？](#param-time-should-be-a-timestamp错误要怎么处理)

# 请求/应答
## 1. 获取交易对行情数据   

  请求URL
  ```
  GET /ticker.php?mk_type={market}&c={coin}
  ```
  例如: https://api.aex.zone/ticker.php?mk_type=cnc&c=btc 
  
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  c       | 币名，比如: c=btc, 获取btc/cnc交易对的行情数据; c=all, 获取cnc交易区下所有有效交易对的行情数据
  
  正常应答（json）
  ```
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
  ```
  ### 错误应答(错误码)
  错误码 | 说明
  -----  | ---------
  "input_error1"        | 参数错误
  "invalid_market_name" | mk_type参数指定的市场无效
  "invalid_trade_coin" | 无效交易对
  "fail#1" | 系统错误
  
  
## 2. 获取交易对深度数据   

  请求URL
  ```
  GET /depth.php?mk_type={market}&c={coin}
  ```
  例如: https://api.aex.zone/depth.php?mk_type=cnc&c=btc 
  
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  c       | 币名，比如: c=btc, 获取btc/cnc交易对的行情数据; c=all, 获取cnc交易区下所有有效交易对的行情数据
  
  
  正常应答（json）
  ```
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
  ```  
  ### 错误应答(错误码)
  错误码 | 说明
  -----  | ---------
  "input_error1"        | 参数错误
  "fail#1" | mk_type参数指定的市场无效
  "no_order" | 无效交易对
  "fail#3" | 系统错误
  
  
## 3. 获取交易对历史成交数据   

  请求URL
  ```
  GET /v3/trades.php?mk_type={market}&c={coin}&tid={tradeId}
  ```
  例如: https://api.aex.zone/v3/trades.php?mk_type=cnc&c=btc 
  
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  mk_type | 交易区，比如 cnc
  c       | 币名，比如: c=btc, 获取btc/cnc交易对的行情数据; c=all, 获取cnc交易区下所有有效交易对的行情数据
  tid     | 从哪个成交ID开始查询（可选参数，不带这个参数默认从第一条开始查询）
  
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
  错误码 | 说明
  -----  | ---------
  "input_error1"        | 参数错误
  "[]" | mk_type参数指定的市场无效, 或者交易对无效，或者无成交数据
  
  
## 4. 我的账户余额   

  请求URL
  ```
  POST /getMyBalance.php
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
    "btc_balance":"0.00022878",
    "btc_balance_lock":"0.00000000",
    "ltc_balance":"0",
    "ltc_balance_lock":"0",
    ...
  }
  ```    
  ### 错误应答(错误码)
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "fail #4" | 系统错误
  
  
  
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
  正常应答: 下单时部分撮合成交（string）
  ```
  "succ|{orderId}"
  ``` 
  
  ### 错误应答(错误码)
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "system_busy"   | 系统错误
  "input_error1"  | 请求参数错误
  "noOrder"  | mk_type和c参数指定的交易对无效
  "invalid_market_name" | mk_type参数指定的市场无效
  "trade_conf_err" | 系统错误，交易参数配置无效
  "invalid_order_type" | type参数错误，只有1=买单，2=卖单
  "input_error2" |  挂单价格无效
  "sys_amt_precision_err#1" | 系统错误，挂单数量精度配置错误
  "sys_amt_precision_err#2" | 系统错误，挂单数量精度配置错误
  "input_error3" | 挂单数量精度超过配置
  "min" | 挂单数量超过最大限制
  "sml" | 挂单数量少于最小限制
  "lag" | 挂单价格超过最大限制
  "sys_price_precision_conf" | 系统错误，价格精度配置错误
  "sys_price_precision_err#1" | 挂单价格精度超过8位
  "sys_price_precision_err#2" | 挂单价格精度<0
  "deciError2" | 挂单价格精度超过系统配置
  "amountsmall" | 挂单金额少于最小限制: 挂单金额=挂单价格x挂单数量
  "succ" | 当前挂单直接撮合了，没有生成订单
  "succ\|{number}" | 当前挂单部分成交或者完全没有成交，剩余的部分生成了订单，订单ID是number
  "system_busy#8" | 可能已经有部分成交或者完全没成交，剩余的部分生成订单失败
  
  
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
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "input_error1"  | 请求参数错误
  "invalid_pricing_coin" | mk_type参数指定的市场无效
  "coinerr"  | mk_type和c参数指定的交易对无效
  "system_busy#1"   | 系统错误
  "system_err" | 系统错误
  "no_record" | 待撤销的订单不存在
  "failed" | 撤销订单失败
  
  
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
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "input_error1"  | 请求参数错误, 或者交易市场无效
  "coinerr"  | mk_type和c参数指定的交易对无效
  "no_record" | 没有挂单

  
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
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "input_error1"  | 请求参数错误, 或者交易市场无效
  "coinerr"  | mk_type和c参数指定的交易对无效
  "no_record" | 没有挂单


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
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "input_error1"  | 请求参数错误, 或者交易市场无效
  "noOrder"  | mk_type和c参数指定的交易对无效
  "no_record" | 没有挂单
  "system_busy" | 系统错误

  
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
  
  ### 错误应答(错误码)
  错误码 | 说明
  -----  | ---------
  "[]"      | 系统错误
  "param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
  "param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
  "public key error"   | 公钥格式错误
  "wrong public key"   | 公钥不存在
  "wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
  "{IP} is not allowed."   | IP不在白名单中
  "input_error1"  | 请求参数错误, 或者交易市场无效
  "noOrder"  | mk_type和c参数指定的交易对无效
  "system_busy" | 系统错误

# FAQ
## "param time should be a timestamp"错误要怎么处理？
  ```
  1）要使用http的post请求，不能是get请求，也不能是json格式的请求   
  2）设置Content-type: application/x-www-form-urlencoded
  ```
    
