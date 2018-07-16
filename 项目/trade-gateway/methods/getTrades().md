请求:
| QryTradeRequest | 字段名称    | 抽象含义 | 火币  | bitfinex | binance | okex  |
|:--------------- |:----------- |:-------- |:----- |:-------- |:------- |:----- |
| String          | accountCode |          |       |          |         |       |
| string          | Symbol      |          | **√** | **√**    | **√**   | **√** |
| int64           | limit       |          |       |          |         |       |

回执:
| TradeResponse   | 字段名称 | 抽象含义 | 火币 | bitfinex | binance | okex |
|:--------------- |:-------- |:-------- |:---- |:-------- |:------- |:---- |
| CommonResponse  | CRsp     |          |      |          |         |      |
| Trade(repeated) | data     | 交易流水 |      |          |         |      |

抽象类详情:
| Trade  | 字段名称        | 抽象含义           | 火币  | bitfinex                 | binance | okex  |
|:------ |:--------------- |:------------------ |:----- |:------------------------ |:------- |:----- |
| string | accountCode     |                    |       |                          |         |       |
| Symbol | symbol          | 币对               |       | 缓存查找                 |         |       |
| int64  | exchangeOrderId | 交易所的订单ID     | **√** | **√**                    | **√**   |       |
| string | price           |                    | **√** | **√**                    | **√**   | **√** |
| string | quantity        | 数量               | **√** | **√**                    | **√**   | **√** |
| string | executedQty     | 已经交易的数量     | **√** | **√**                    | **√**   | **√** | 
| int64  | timestamp       | 时间戳             | **√** | **√**                    | **√**   | **√** |
| int64  | tradeId         | 交易ID             | **√** | **√**                    | **√**   | **√** |
| string | commission      |                    | **√** | **√**(暂定精度为8位小数) | **√**   |       |
| string | side            | 交易方向(buy/sell) | **√** | **√**                    | **√**   | **√** |
| string | orderPrice      | 订单价格           | **√** | **√**                    | **√**   |       |


注释:
1. bitfinex
  在bitfinex中getTrades是webSocket方式获取的
2. timestamp
  时间戳需要统一转换到本地时区
