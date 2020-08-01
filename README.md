# ispay就付
ispay.in支付系统,对接文档说明

---

### 订单提交


调用地址：http://payment.ispay.in/v2/gateway

请求方式：POST DATA

| 名称       | 类型    | 是否必须 | 描述                                                       |
| ---------- | :------ | :------- | :--------------------------------------------------------- |
| userid     | int     | 是       | 商户 ID                                                    |
| subject    | string  | 否       | 订单标题                                                   |
| order_sn   | string  | 是       | 订单号（唯一）列：20191118123424                           |
| order_type | string  | 是       | 订单类型 pdd,wepay,alipay            |
| amount     | float64 | 是       | 订单的金额,必须 300.00 保留 2 位小数点                     |
| notify_url | string  | 是       | 订单成功回调地址                                           |
| return_url | string  | 是       | 订单成功返回地址                                           |
| sign       | string  | 是       | 签名MD5 请查看签名说明                                          |

### 返回JSON 

| 名称       | 类型    | 是否必须 | 描述                                                       |
| ---------- | :------ | :------- | :--------------------------------------------------------- |
| success    | bool    | 是       | 返回true,false                                           |
| errorCode  | int     | 是       | 错误代码                                                  |
| order_id   | string  | 是       | 订单ID                          |
| data       | string  | 否       | SDK支付代码（SDK模式）            |
| url        | string  | 否       | URL跳转代码，                     |

SDK
```
{
    "success": true, 
    "errorCode": 0,
    "order_id": 10000, 
    "data":"code"
 }
 

```

 H5
```


 {
    "success": true, 
    "errorCode": 0,
    "order_id": 10000, 
    "url": "https://github.com/conku/ayang"
 }

```

---
### 订单号查询

请求方式：GET

调用地址：http://payment.ispay.in/order/{商户ID}/{订单号}/{md5}

请求方式：GET

返回类型：JSON

签名说明：md5({商户 ID}{订单号}{商户签名})

返回成功

```

{
    "success": true,
    "errorCode": 0,
    "errorMsg": nil,
    "order":{
         "sn":             "20191118123424",               //订单编号
         。。。。。。。。。。。
    }
}

```

---

### 回调通知接口 

订单回调通知地址： notify_url

请求方式：POST

签名说明：

| 名称       | 类型    | 是否必须 | 描述                                                       |
| ---------- | :------ | :------- | :--------------------------------------------------------- |
| order_no   | string  | 是       | 订单号（唯一）列：20191118123424                     |
| amount     | string  | 否       | 价格                                                |
| state      | int     | 是       | 订单状态 9 为支付成功                           |
| callbacks  | string  | 是       | 时间搓                                       |
| sign       | string  | 是       | 签名                                   |


返回 TXT 
成功

```

success

```
失败
```

fail

```

---

### 签名说明

第一步，设所有发送或者接收到的数据为集合M，将集合M内非空参数值的参数按照参数名ASCII码从小到大排序（字典序），使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串stringA

特别注意以下重要规则：
1、参数名ASCII码从小到大排序（字典序）
2、参数名区分大小写
3、如果参数的值为空也参与签名


第二步，在stringA最后拼接上
$stringSignTemp = $stringA ."&key=192006250b4c09247ec02edce69f6a2d"; // 拼接商户密钥
$sign = md5($stringSignTemp); // md5加密

