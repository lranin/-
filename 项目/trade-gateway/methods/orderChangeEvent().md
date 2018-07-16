抽象类详情:
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

com.softwin.common.dao.domain.Order
| order  | 字段名称       | 含义                       | 对应字段 |
| ------ | -------------- | -------------------------- | -------- |
| int    | id             |                            |          |
| String | subaccountCode |                            |          |
| string | Symbol         |                            | **√**    |
| String | baseCurrency   |                            | **√**    |
| String | quoteCurrency  |                            | **√**    |
| String | exchangeType   |                            | **√**    |
| String | price          |                            | **√**    |
| String | origQty        |                            | **√**    |
| String | side           |                            | **√**    |
| String | type           |                            | **√**    |
| String | orderStatus    |                            | **√**    |
| Date   | insertTime     | 插入时间:自动生成          |          |
| Date   | updateTime     | 修改时间:自动生成          |          |
| String | orderId        | 业务逻辑中订单的唯一标识符 |          |


bitfinex的订单状态
