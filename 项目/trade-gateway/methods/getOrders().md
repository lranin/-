
请求:
| AllOrderRequest | 字段名称    | 抽象含义 | 火币         | bitfinex     | binance      | okex         |
|:----------------|:------------|:---------|:-------------|:-------------|:-------------|:-------------|
| string          | symbol      | 货币对   | **required** | x            | **required** | **required** |
| string          | state       | 订单状态 | **required** | **optional** | **optional** | **required** |
| string          | accountCode | 账户code |              |              |              |              |

回执:
| AllOrderResponse | 字段名称 | 抽象含义   | 火币 | bitfinex | binance | okex |
|:-----------------|:---------|:-----------|:-----|:---------|:--------|:-----|
| CommonResponse   | CRsp     | 返回状态值 |      |          |         |      |
| Order(repeated)  | data     |            |      |          |         |      |

抽象类详情:(测试)
| Order  | 字段名称        | 抽象含义               | 火币            | bitfinex        | binance         | okex            |
|:-------|:----------------|:-----------------------|:----------------|:----------------|:----------------|:----------------|
| string | accountCode     |                        |                 |                 |                 |                 |
| Symbol | symbol          | 交易对                 | 通过缓存查找    | 通过缓存查找    | 通过缓存查找    | 通过缓存查找    |
| int64  | exchangeOrderId | 交易所的orderId        | **√**           | **√**           | **√**           | **√**           |
| string | price           | 价格                   | **√**           | **√**           | **√**           | **√**           |
| string | origQty         | 初始目标数量           | **√**(需要转换) | **√**           | **√**           | **√**           |
| string | executedQty     | 已经交易的数量         |                 | **√**           | **√**           | **√**           |
| string | orderStatus     | 订单状态               | **√**(需要转换) | **√**(需要转换) | **√**(需要转换) | **√**(需要转换) |
| string | type            | 交易类型(limit/market) | **√**(需要转换) | **√**(需要转换) | **√**           | **√**(需要转换) |
| string | side            | 交易方向(buy/sell)     | **√**(需要转换) | **√**           | **√**           | **√**(需要转换) |
| string | stopPrice       |                        | **√**(需要转换) |                 | **√**           |                 |
