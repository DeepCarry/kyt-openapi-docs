# 5、黑名单地址检测接口

说明：此接口只检测钱包地址是否是黑名单。此接口不执行规则引擎配置的策略，也不检测用户在KYT系统配置的私有数据。

#### 1. 接口URL

GET   /openapi/v3/risk/address/blacklist

#### 2. 请求参数

| **参数名** | **数据类型** | **是否必传** | **描述** |
| ------- | -------- | -------- | ------ |
| apikey  | string   | true     | apikey |
| address | string   | true     | 钱包地址   |

#### 3. 返回参数

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "unique_id": "0140......8be5", 
        "risk_level": "severe", 
        "risk_types": "Sanctions", 
        "risk_tags":[ 
            "chenzhi",
            "prince group",
            "Sanctions"
        ]
        "risk_detail":{
            "is_blacklist_address": true, 
        }
    }
}
```

#### 返回字段说明

<table><thead><tr><th width="107.89453125">参数名</th><th width="105.51953125">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>code</td><td>int</td><td>业务状态码。<strong>200</strong>：请求成功；其他值表示请求失败（具体错误码以实际返回为准）。</td></tr><tr><td>message</td><td>string</td><td>接口请求状态描述，例如：<code>success</code>。</td></tr><tr><td>data</td><td>object</td><td>返回数据对象。</td></tr></tbody></table>

***

#### data 字段说明

<table><thead><tr><th width="108.63671875">参数名</th><th width="107.43359375">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>unique_id</td><td>string</td><td>唯一请求 ID（标识当前黑名单检测请求，可用于日志追踪与问题排查）。</td></tr><tr><td>risk_level</td><td>string</td><td>风险等级：<code>severe</code>（严重）、<code>high</code>（高风险）、<code>medium</code>（中风险）、<code>low</code>（低风险）。说明：黑名单权重策略调整后，等级可能不固定为 severe。</td></tr><tr><td>risk_types</td><td>string</td><td>风险类型（黑名单所属风险类别），例如：<code>Sanctions</code>。</td></tr><tr><td>risk_tags</td><td>string[]</td><td>风险标签数组（更细粒度标签），例如：<code>["Sanctions","Prince Group"]</code>。</td></tr><tr><td>risk_detail</td><td>object</td><td>风险明细对象。</td></tr></tbody></table>

***

#### risk\_detail 字段说明

<table><thead><tr><th width="176.0546875">参数名</th><th width="107.2265625">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>is_blacklist_address</td><td>boolean</td><td>是否为黑名单地址。<code>true</code> 表示命中黑名单；<code>false</code> 表示未命中。</td></tr></tbody></table>

