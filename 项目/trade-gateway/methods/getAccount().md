#请求:
| AccountRequest | 字段名称    |
|:---------------|:------------|
| string         | accountCode |

#回执:
| AccountResponse | 字段名称 |
|:----------------|:---------|
| CommonResponse  | CRsp     |
| Account         | data     |

#抽象类详情:
| Account                | 字段名称             | 具体含义   | 火币                  | bitfinex       | binance        | okex           |
|:---------------------- |:-------------------- |:---------- | --------------------- | -------------- | -------------- | -------------- |
| int32                  | **markerCommission** | 做市费率   | **程序设定默认值2‰ ** | **√** | **√** | **√** |
| int32                  | **takerCommission**  | 吃单费率   | **程序设定默认值2‰ ** | **√** | **√** | **√** |
| int32                  | **buyerCommission**  |            | **程序设定默认值2‰ ** |                | **√** |                |
| int32                  | **sellerCommission** |            | **程序设定默认值2‰ ** |                | **√** |                |
| AssetBalance(repeated) | **balance**          | 余额详情   | **√**        | **√** | **√** | **√** |
| string                 | **exchangeType**     | 交易所名称 |                       |                |                |                |

| AssetBalance | 字段名称   | 具体含义 | 火币           | bitfinex       | binance        | okex           |
|:------------ |:---------- |:-------- | -------------- | -------------- | -------------- | -------------- |
| String       | **asset**  | 币种     | **√** | **√** | **√** | **√** |
| String       | **free**   | 可使用   | **√** | **√** | **√** | **√** |
| string       | **locked** | 被冻结   | **√** | **√** | **√** | **√** |

#具体交易所实现(bitfinex,huobi,binance,okex):
##1. bitfinex

### accountInfo
| AccountInfo         | 字段名称  | 具体含义       | 对应抽象类字段       |
|:------------------- |:--------- |:-------------- |:-------------------- |
| float               | makerFees | 全局做单费率       | **markerCommission** |
| float               | takerFees | 全局吃单费率       | **takerCommission**  |
| pairsFees(repeated) | parisFees | 指定币种的费率 | **无对应字段**           |

| parisFees | 字段名称  | 具体含义 | 对应抽象类字段 |
|:--------- |:--------- |:-------- |:-------------- |
| string    | pairs     | 币种     | **无对应字段** |
| float     | makerFees | 做单费率 | **无对应字段** |
| float     | takerFees | 吃单费率 | **无对应字段** |

### AssetBalance
| AssetBalance | 字段名称  | 具体含义 | 限定                            | 对应抽象字段                |
|:------------ |:--------- |:-------- |:------------------------------- | --------------------------- |
| string       | type      | 资金类型 | “trading”, “deposit”,“exchange” | **无对应字段**              |
| string       | currency  | 币种     | eg.:BTC                         | **asset**                   |
| string       | amount    | 资金总额 |                                 | amount-available=**locked** |
| string       | available | 可用资金 |                                 | **free**                    |
