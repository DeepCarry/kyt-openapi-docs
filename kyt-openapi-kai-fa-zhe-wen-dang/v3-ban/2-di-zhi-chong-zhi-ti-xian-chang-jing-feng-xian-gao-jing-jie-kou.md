# 2、地址充值|提现场景风险告警接口

说明：在充值或提现交易预先获知对方钱包地址的交易业务场景，提前对钱包地址进行风险筛查。此接口会执行规则引擎配置的策略。

#### 1. 接口URL

GET    /openapi/v3/risk/rule/address/screening

#### 2. 请求参数

<table data-header-hidden><thead><tr><th width="137.3125"></th><th width="131.49609375"></th><th width="124.69140625"></th><th width="350.265625"></th></tr></thead><tbody><tr><td><strong>参数名</strong></td><td><strong>数据类型</strong></td><td><strong>是否必传</strong></td><td><strong>描述</strong></td></tr><tr><td>apikey</td><td>string</td><td>true</td><td>apikey</td></tr><tr><td>chain</td><td>string</td><td>true</td><td>主链名称</td></tr><tr><td>address</td><td>string</td><td>true</td><td>钱包地址</td></tr><tr><td>address_role</td><td>string</td><td>true</td><td>地址角色（from：转出地址； to：转入地址 ）</td></tr><tr><td>coin</td><td>string</td><td>true</td><td>代币名称</td></tr><tr><td>app_id</td><td>string</td><td>true</td><td>应用id</td></tr><tr><td>value</td><td>float64</td><td>false</td><td>代币数量</td></tr></tbody></table>

#### 3. 返回参数

```json
{
    "code": 200, 
    "message": "success",
    "data": {
        "unique_id": "0140......8be5",
        "risk_level": "severe",
        "risk_types": [
            "Stolen Crypto"
        ],
        "risk_tags":[ 
            "Theft",
            "Stolen Crypto"
        ],
        "risk_code": 4444, 
        "risk_detail": {
            "private_data": { 
                "hit_private_whitelist": false,
                "hit_private_balcklist": false,
            }, 
            "is_blacklist_address": true,
            "hit_direct_risk_review": false, 
            "hit_indirect_risk_review": true,
            "hit_aml_review": true, 
        }
    }
}
```

#### 返回字段说明

<table><thead><tr><th width="196.90234375">参数名</th><th width="114.94140625">数据类型</th><th>描述</th></tr></thead><tbody><tr><td>code</td><td>int</td><td>业务状态码。<strong>200</strong>：请求成功；其他值表示失败（具体错误码以实际返回为准）。</td></tr><tr><td>message</td><td>string</td><td>接口请求状态描述，例如：<code>success</code>。</td></tr><tr><td>data</td><td>object</td><td>返回数据对象。</td></tr><tr><td>data.unique_id</td><td>string</td><td>唯一请求 ID（标识当前请求，可用于排查/追踪）。</td></tr><tr><td>data.risk_level</td><td>string</td><td>风险等级：<code>severe</code>（严重）、<code>high</code>（高风险）、<code>medium</code>（中风险）、<code>low</code>（低风险）、<code>none</code>（未知/未命中）。</td></tr><tr><td>data.risk_types</td><td>string[]</td><td>风险类型（英文标签数组），用于表达命中的风险大类/具体类别，例如：<code>["Stolen Crypto"]</code>。</td></tr><tr><td>data.risk_tags</td><td>string[]</td><td>风险标签（更细粒度标签数组），可能包含多个维度标签，例如：<code>["Theft","Stolen Crypto"]</code>。</td></tr><tr><td>data.risk_code</td><td>int</td><td>风险 code：<code>0</code>（未筛查到风险）、<code>-1</code>（未查询到）、<code>4444</code>（命中黑名单）、其他 code 详见《risk_code说明书》。</td></tr><tr><td>data.risk_detail</td><td>object</td><td>风险明细对象（命中原因/策略命中情况等）。</td></tr><tr><td>data.risk_detail.private_data</td><td>object</td><td>私有名单命中信息。</td></tr><tr><td>data.risk_detail.private_data.hit_private_whitelist</td><td>boolean</td><td>是否命中<strong>私有白名单</strong>。<code>true</code> 命中，<code>false</code> 未命中。</td></tr><tr><td>data.risk_detail.private_data.hit_private_balcklist</td><td>boolean</td><td>是否命中<strong>私有黑名单</strong>。<code>true</code> 命中，<code>false</code> 未命中。</td></tr><tr><td>data.risk_detail.private_data.private_balcklist_name</td><td>string</td><td>命中的私有黑名单名称（未命中时可能为空或不返回）。</td></tr><tr><td>data.risk_detail.is_blacklist_address</td><td>boolean</td><td>是否为黑名单地址（综合判定）。</td></tr><tr><td>data.risk_detail.hit_direct_risk_review</td><td>boolean</td><td>是否命中<strong>直接风险review</strong></td></tr><tr><td>data.risk_detail.hit_indirect_risk_review</td><td>boolean</td><td>是否命中<strong>间接风险review</strong></td></tr><tr><td>data.risk_detail.hit_aml_review</td><td>boolean</td><td>是否命中<strong>反洗钱review</strong> </td></tr></tbody></table>

