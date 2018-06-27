
请求:
| AllOrderRequest | 字段名称    | 抽象含义 |
|:----------------|:------------|:---------|
| string          | symbol      | 货币对   |
| string          | state       | 订单状态 |
| string          | accountCode | 账户code |

回执:
| AllOrderResponse | 字段名称 | 抽象含义   | 火币 | bitfinex | binance | okex |
|:---------------- |:-------- |:---------- |:---- |:-------- |:------- |:---- |
| CommonResponse   | CRsp     | 返回状态值 |      |          |         |      |
| Order(repeated)  | data     |            |      |          |         |      |

抽象类详情:
| Order  | 字段名称        | 抽象含义               | 火币 | bitfinex                    | binance | okex |
| ------ | --------------- | ---------------------- | ---- | --------------------------- | ------- | ---- |
| string | accountCode     |                        |      |                             |         |      |
| Symbol | symbol          | 交易对                 |      |                             |         |      |
| int64  | exchangeOrderId | 交易所的orderId        |      |                             |         |      |
| string | price           | 价格                   |      |                             |         |      |
| string | origQty         | 初始目标数量           |      |                             |         |      |
| string | executedQty     | 已经交易的数量         |      |                             |         |      |
| string | type            | 交易类型(limit/market) |      | exchange(前缀) limit/market |         |      |
| string | side            | 交易方向(buy/sell)     |      |                             |         |      |
| string | stopPrice       |                        |      |                             |         |      |
