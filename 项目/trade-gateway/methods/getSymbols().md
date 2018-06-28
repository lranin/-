请求:
| SymbolRequest | 字段名称     | 抽象含义 | 火币     | bitfinex | binance | okex |
|:--------------|:-------------|:---------|:---------|:---------|:--------|:-----|
| String        | exchangeType | huobi    | bitfinex | binance  | okex    |      |

回执:
| SymbolResponse   | 字段名称 | 抽象含义   | 火币 | bitfinex | binance | okex |
|:---------------- |:-------- |:---------- |:---- |:-------- |:------- |:---- |
| CommonResponse   | CRsp     | 返回状态值 |      |          |         |      |
| Symbol(repeated) | data     |            |      |          |         |      |

抽象类详情:
| Symbol | 字段名称        | 抽象含义     | 火币                  | bitfinex          | binance | okex  |
|:------ |:--------------- |:------------ |:--------------------- |:----------------- |:------- |:----- |
| String | exchangeType    | 交易所名称   |                       |                   |         |       |
| String | Symbol          | 交易币对     | **√**                 | **√**             | **√**   | **√** |
| stirng | baseCurrency    | 基础币种     | **√**                 | **√**(截取symbol) | **√**   | **√** |
| String | quoteCurrency   | 目标币种     | **√**                 | **√**             | **√**   | **√** |
| int32  | pricePrecision  | 价格精度     | **√**                 | **√**             | **√**   | **√** |
| int32  | amountPrecision | 数量精度     | **√**                 |                   | **√**   |       |
| string | minAmount       | 最小交易数额 | **√**(从详情接口获取) | **√**             | **√**   | **√** |


注释:
1. minAmount
   所有交易所的minAmount需要统一为0.000001方式,而不是科学计数法
