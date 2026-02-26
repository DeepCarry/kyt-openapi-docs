# 3、实时交易风险告警接口

说明：支持对最新实时交易（最近24小时内）的风险筛查。用于自己作为资金接收方，链上交易发生后进行风险筛查，聚焦交易资金来源的风险。此接口会执行规则引擎配置的策略。

#### 1. 接口URL

GET    /openapi/v3/risk/rule/tx/screening

#### 2. 请求参数

<table data-header-hidden><thead><tr><th width="145.33984375"></th><th width="137.67578125"></th><th width="139.98828125"></th><th></th></tr></thead><tbody><tr><td><strong>参数名</strong></td><td><strong>数据类型</strong></td><td><strong>是否必传</strong></td><td><strong>描述</strong></td></tr><tr><td>apikey</td><td>string</td><td>true</td><td>apikey</td></tr><tr><td>tx</td><td>string</td><td>true</td><td>交易hash</td></tr><tr><td>chain</td><td>string</td><td>true</td><td>主链名称</td></tr><tr><td>to_address</td><td>string</td><td>true</td><td>代币接收地址</td></tr><tr><td>coin</td><td>string</td><td>true</td><td>代币名称</td></tr><tr><td>app_id</td><td>string</td><td>true</td><td>应用id</td></tr></tbody></table>

#### 3. 返回参数

```json
{
    "code": 200,
    "message": "success",
    "data": { 
        "unique_id": "2541......c245", 
        "risk_level": "severe", 
        "risk_types": [
            "Blackmail Scam"         
        ],
        "risk_tags":[
            "phishing",
            "Blackmail Scam"
        ],
        "risk_code": 4444, 
        "risk_detail": {      
            "private_data": { 
                "hit_private_whitelist": false, 
                "hit_private_blacklist": false, 
                "private_blacklist_name":"私有黑名单名称",
            },
            "hit_blacklist_address":true,
            "tx_blacklist_address": [
                {
                    "blacklist_address": "0xE0......D015",
                    "risk_types":"Blackmail Scam",
                    "risk_tags":[ 
                        "phishing"
                    ],
                },
            ], 
            "hit_direct_risk_review": false, 
            "hit_indirect_risk_review": true,
            "hit_aml_review": true,
        }
    }
}
```



#### 返回字段说明

<table><thead><tr><th width="132.46875">参数名</th><th width="145.55859375">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>code</td><td>int</td><td>业务状态码。200<strong>：</strong>请求成功；其他值表示请求失败（具体错误码以实际返回为准）。</td></tr><tr><td>message</td><td>string</td><td>接口请求状态描述，例如：<code>success</code>。</td></tr><tr><td>data</td><td>object</td><td>返回数据对象。</td></tr></tbody></table>

#### data 字段说明

<table><thead><tr><th width="133.43359375">参数名</th><th width="143.625">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>unique_id</td><td>string</td><td>唯一请求 ID（标识当前风险筛查请求，可用于日志追踪与问题排查）。</td></tr><tr><td>risk_level</td><td>string</td><td>风险等级：<code>severe</code>（严重）、<code>high</code>（高风险）、<code>medium</code>（中风险）、<code>low</code>（低风险）、<code>none</code>（未命中风险或未知）。</td></tr><tr><td>risk_types</td><td>string[]</td><td>风险类型数组（风险大类）。例如：<code>["Blackmail Scam"]</code>。</td></tr><tr><td>risk_tags</td><td>string[]</td><td>风险标签数组（更细粒度风险标签）。例如：<code>["phishing","Blackmail Scam"]</code>。</td></tr><tr><td>risk_code</td><td>int</td><td>风险 code：<code>0</code>（未筛查到风险）、<code>-1</code>（未查询到）、<code>4444</code>（命中黑名单）、其他 code 详见《risk_code说明书》。</td></tr><tr><td>risk_detail</td><td>object</td><td>风险明细信息（策略命中情况、黑名单命中信息等）。</td></tr></tbody></table>

***

#### risk\_detail 字段说明

<table><thead><tr><th width="207.29296875">参数名</th><th width="141.48828125">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>private_data</td><td>object</td><td>私有名单命中信息。</td></tr><tr><td>hit_blacklist_address</td><td>boolean</td><td>交易中是否包含黑名单地址（from 或 to）。</td></tr><tr><td>tx_blacklist_address</td><td>array</td><td>交易中命中的黑名单地址列表（可能包含多个）。</td></tr><tr><td>hit_direct_risk_review</td><td>boolean</td><td>是否命中直接风险review。</td></tr><tr><td>hit_indirect_risk_review</td><td>boolean</td><td>是否命中间接风险review。</td></tr><tr><td>hit_aml_review</td><td>boolean</td><td>是否命中反洗钱review。</td></tr></tbody></table>

***

#### private\_data 字段说明

<table><thead><tr><th width="209.59375">参数名</th><th width="140.94140625">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>hit_private_whitelist</td><td>boolean</td><td>是否命中私有白名单。<code>true</code> 表示命中白名单。</td></tr><tr><td>hit_private_blacklist</td><td>boolean</td><td>是否命中私有黑名单。<code>true</code> 表示命中黑名单。</td></tr><tr><td>private_blacklist_name</td><td>string</td><td>命中的私有黑名单名称（未命中时可能为空或不返回）。</td></tr></tbody></table>

***

#### tx\_blacklist\_address 元素说明

<table><thead><tr><th width="205.953125">参数名</th><th width="147.12109375">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>blacklist_address</td><td>string</td><td>命中的黑名单地址。</td></tr><tr><td>risk_types</td><td>string</td><td>该地址对应的风险类型。</td></tr><tr><td>risk_tags</td><td>string[]</td><td>该地址对应的风险标签数组。</td></tr></tbody></table>

