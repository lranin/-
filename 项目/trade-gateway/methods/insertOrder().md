请求:
| InsertOrderRequest | 字段名称    | 抽象含义               | 火币 | bitfinex                    | binance | okex |
|:-------------------|:------------|:-----------------------|:-----|:----------------------------|:--------|:-----|
| String             | accountCode |                        |      |                             |         |      |
| string             | Symbol      | 交易对                 |      |                             |         |      |
| string             | quantity    | 数量                   |      |                             |         |      |
| String             | price       | 价格                   |      |                             |         |      |
| String             | type        | 交易类型(limit/market) |      | exchange(前缀) limit/market |         |      |
| string             | side        | 交易方向(buy/sell)     |      |                             |         |      |

回执:
| InsertOrderResponse | 字段名称 | 抽象含义 | 火币 | bitfinex | binance | okex |
|:------------------- |:-------- |:-------- |:---- |:-------- |:------- | ---- |
| CommonResponse      | CRsp     |          |      |          |         |      |
| int64               | orderId  | 订单ID   |      |          |         |      |
