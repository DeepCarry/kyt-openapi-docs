# 4、钱包地址风险筛查接口

说明：对钱包地址进行风险筛查。此接口不执行规则引擎配置的策略，也不检测用户在KYT系统配置的私有数据。

#### 1. 接口URL

GET   /openapi/v3/risk/address/screening

#### 2. 请求参数

<table data-header-hidden><thead><tr><th width="140.50390625"></th><th width="136.890625"></th><th width="126.6875"></th><th></th></tr></thead><tbody><tr><td><strong>参数名</strong></td><td><strong>数据类型</strong></td><td><strong>是否必传</strong></td><td><strong>描述</strong></td></tr><tr><td>apikey</td><td>string</td><td>true</td><td>apikey</td></tr><tr><td>chain</td><td>string</td><td>true</td><td>主链</td></tr><tr><td>address</td><td>string</td><td>true</td><td>钱包地址</td></tr></tbody></table>

#### 3. 返回参数

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "unique_id": "0140......8be5", 
        "tcr_score": 41, 
        "risk_level": "severe", 
        "risk_types": [ //风险类型
            "Stolen Crypto"
        ],
        "risk_tags":[ //风险标签
            "Theft",
            "Stolen Crypto"
        ],
        "risk_detail": {        
            "is_blacklist_address": true, 
            "hit_direct_risk_review": false, //命中直接风险审查
            "hit_indirect_risk_review": true, //命中间接风险审查
            "hit_aml_review": true,  //命中反洗钱审查
        }
    }
}
```



#### 返回字段说明

<table><thead><tr><th width="124.2109375">参数名</th><th width="125.46484375">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>code</td><td>int</td><td>业务状态码。<strong>200</strong>：请求成功；其他值表示请求失败（具体错误码以实际返回为准）。</td></tr><tr><td>message</td><td>string</td><td>接口请求状态描述，例如：<code>success</code>。</td></tr><tr><td>data</td><td>object</td><td>返回数据对象。</td></tr></tbody></table>

***

#### data 字段说明

<table><thead><tr><th width="122.5625">参数名</th><th width="121.7890625">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>unique_id</td><td>string</td><td>唯一请求 ID（标识当前风险筛查请求，可用于日志追踪与问题排查）。</td></tr><tr><td>tcr_score</td><td>int</td><td>地址风险评分（TCR Score）。数值越高表示风险程度越高。</td></tr><tr><td>risk_level</td><td>string</td><td>风险等级：<code>severe</code>（严重）、<code>high</code>（高风险）、<code>medium</code>（中风险）、<code>low</code>（低风险）、<code>none</code>（未命中风险或未知）。</td></tr><tr><td>risk_types</td><td>string[]</td><td>风险类型数组（风险大类），例如：<code>["Stolen Crypto"]</code>。</td></tr><tr><td>risk_tags</td><td>string[]</td><td>风险标签数组（更细粒度风险标签），例如：<code>["Theft","Stolen Crypto"]</code>。</td></tr><tr><td>risk_detail</td><td>object</td><td>风险明细对象（风险命中情况说明）。</td></tr></tbody></table>

***

#### risk\_detail 字段说明

<table><thead><tr><th width="199.58203125">参数名</th><th width="101.98046875">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>is_blacklist_address</td><td>boolean</td><td>是否为黑名单地址（平台公共风险库判定结果）。</td></tr><tr><td>hit_direct_risk_review</td><td>boolean</td><td>是否命中直接风险review。</td></tr><tr><td>hit_indirect_risk_review</td><td>boolean</td><td>是否命中间接风险审review。</td></tr><tr><td>hit_aml_review</td><td>boolean</td><td>是否命中反洗钱review。</td></tr></tbody></table>
