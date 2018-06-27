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
| Symbol | 字段名称        | 抽象含义 | 火币 | bitfinex | binance | okex |
|:-------|:----------------|:---------|:-----|:---------|:--------|:-----|
| String | exchangeType    |          |      |          |         |      |
| String | Symbol          |          |      |          |         |      |
| stirng | baseCurrency    |          |      |          |         |      |
| String | quoteCurrency   |          |      |          |         |      |
| int32  | pricePrecision  |          |      |          |         |      |
| int32  | amountPrecision |          |      |          |         |      |
| string | minAmount       |          |      |          |         |      |
