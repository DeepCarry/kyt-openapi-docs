---
description: 获取openapi接口支持的主链和代币名称
---

# 1、主链代币枚举值接口

说明：获取openapi接口支持的主链和代币名称

#### 1. 接口URL

GET    /openapi/v3/chains/enum

#### 2. 请求参数

| **参数名** | **数据类型** | **是否必传** | **描述** |
| ------- | -------- | -------- | ------ |
| apikey  | string   | true     | apikey |

#### 3. 返回参数

```json
{
    "code": 200,
    "msg": "success",
    "data": { 
        "list": [
            {
                "coin_list": [ 
                    "BTC"
                ],
                "chain": "Bitcoin" 
            },
            {
                "coin_list": [
                    "ETH",
                    "USDT",
                    "USDC"
                ],
                "chain": "Ethereum"
            },
            {
                "coin_list": [
                    "TRX",
                    "USDT",
                    "USDC"
                ],
                "chain": "TRON"
            },
            ......
        ]
    }
}
```

