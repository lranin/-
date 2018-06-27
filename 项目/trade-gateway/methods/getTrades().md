请求:
| QryTradeRequest | 字段名称    | 抽象含义 | 火币 | bitfinex | binance | okex |
|:----------------|:------------|:---------|:-----|:---------|:--------|:-----|
| String          | accountCode |          |      |          |         |      |
| string          | Symbol      |          |      |          |         |      |
| int64           | limit       |          |      |          |         |      |

回执:
| TradeResponse   | 字段名称 | 抽象含义 | 火币 | bitfinex | binance | okex |
|:----------------|:---------|:---------|:-----|:---------|:--------|:-----|
| CommonResponse  | CRsp     |          |      |          |         |      |
| Trade(repeated) | data     |          |      |          |         |      |

抽象类详情:
| Trade           | 字段名称        | 抽象含义 | 火币 | bitfinex | binance | okex |
|:----------------|:----------------|:---------|:-----|:---------|:--------|:-----|
| string          | accountCode     |          |      |          |         |      |
| Symbol          | symbol          |          |      |          |         |      |
| int64           | exchangeOrderId |          |      |          |         |      |
| string price    |                 |          |      |          |         |      |
| string quantity |                 |          |      |          |         |      |
| string          | executedQty     |          |      |          |         |      |
| int64           | timestamp       |          |      |          |         |      |
| int64           | tradeId         |          |      |          |         |      |
| string          | commission      |          |      |          |         |      |
| string          | side            |          |      |          |         |      |
| string          | orderPrice      |          |      |          |         |      |


注释:
1. bitfinex
  在bitfinex中getTrades是webSocket方式获取的
