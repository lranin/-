抽象类详情:
| Trade  | 字段名称        | 抽象含义           | 火币  | bitfinex | binance | okex  |
|:-------|:----------------|:-------------------|:------|:---------|:--------|:------|
| string | accountCode     |                    |       |          |         |       |
| Symbol | symbol          | 币对               |       | 缓存查找 |         |       |
| int64  | exchangeOrderId | 交易所的订单ID     | **√** |          | **√**   |       |
| string | price           | 单价               | **√** |          | **√**   | **√** |
| string | quantity        | 数量               | **√** |          | **√**   | **√** |
| string | executedQty     | 已经交易的数量     |       |          |         |       |
| int64  | timestamp       | 时间戳             | **√** |          | **√**   | **√** |
| int64  | tradeId         | 交易ID             | **√** |          | **√**   | **√** |
| string | commission      | 费率               | **√** |          | **√**   |       |
| string | side            | 交易方向(buy/sell) | **√** |          | **√**   | **√** |
| string | orderPrice      | 订单总价格         | **√** |          | **√**   |       |

com.softwin.common.dao.domain.Trade
| Trade  | 字段名称        | 含义 | 对应字段 |
|:-------|:----------------|:-----|:---------|
| int    | id              |      |          |
| String | subaccountCode  |      |          |
| long   | exchangeOrderId |      | **√**    |
| String | exchangeType    |      | **√**    |
| String | baseCurrency    |      | **√**    |
| String | quoteCurrency   |      | **√**    |
| String | price           |      | **√**    |
| String | quantity        |      | **√**    |
| Date   | tradeTime       |      | **√**    |
| long   | tradeId         |      | **√**    |
| String | Symbol          |      | **√**    |
| String | commission      |      | **√**    |
| String | side            |      | **√**    |
| String | orderPrice      |      | **√**    |
