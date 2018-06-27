请求:
| CancelOrderRequest | 字段名称    |
| ------------------ | ----------- |
| string             | accountCode |
| string             | symbol      |
| int64              | orderId     |

回执:
| CancelOrderResponse | 字段名称 |
| ------------------- | -------- |
| CommonResponse      | CRsp     |
| CancelOrder         | data     |

字段属性
| CancelOrder | 字段名称     | 抽象含义       | 火币 | bitfinex | binance | okex |
|:------------|:-------------|:---------------|:-----|:---------|:--------|:-----|
| string      | accountCode  |                |      |          |         |      |
| string      | cancelstatus | 订单的取消状态 |      |          |         |      |
| int64       | orderId      | 交易所的订单ID |      |          |         |      |
