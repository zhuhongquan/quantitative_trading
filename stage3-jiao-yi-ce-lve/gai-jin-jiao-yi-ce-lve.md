---
description: >-
  上节课的代码仍有很多不完善的地方：例如会每隔3s都要计算一次ma10、买入时没有判断当前账户资金余量、卖出时没有判断是否有能够卖出的单子等等......所以这节课需要把一些细节完善。
---

# 改进交易策略

## 需要改进的地方

### 减少ma10计算次数

通过定义实例属性（True/False）来设置和判断当前是否是一个newBar。

### 买入/卖出逻辑

1. 为了简化操作，我们默认一次只买100股（一手）。
2. 需要设置一个资金池（定义一个实例属性），代表当前账户余额。（友情提醒：建议设置的资金总量大于300股的价格，低于100股则永远无法买入）
3. 设置一个实例属性self.current\_orders，类型为字典，存放当前购买的股票信息。
4. 设置一个实例属性self.history\_orders，类型为字典，存放卖出的股票信息（注意卖出时，是把当前order从current\_orders删除，再添加到history\_orders）。
5. 如果达到了买入条件（即低于ma10一定价格），那么先判断账户里资金是否足够买100股，如果够，则买入，否则退出（return）。
6. 如果达到了卖出条件（即高于ma10一定价格），那么判断账户里是否有已购买的，如果有，则一次性全部卖出，否则退出（return)。
7. 买入卖出时，别忘了对账户余额进行加减。

## 一些代码示例

以下内容仅供参考，你还可以将它变得更完善！

### 1、实例属性

```python
class AstockTrading:
    def __init__(self, strategy_name):
        self.strategy_name = strategy_name
        self.Dt = None
        self.Open = None
        self.High = None
        self.Low = None
        self.Close = None
        self.ma10 = None

        self.money = 100000  # 初始资金10w
        self.isNewBar = False  # 初始设置为False
        # self.current_orders初始应该为空，当买入一次股票时，则往里面添加一个order
        # 由于字典key不可重名，所以order需要递增，这需要在买入订单时，根据self.order_number计算当前id
        # 所以当第二次买入时，current_orders里面就应该为order2: {...}
        self.current_orders = {
            'order1': {'open_price': 1,
                       'open_datetime': '2022-03-03 9:30',
                       'volume': 100,
                       'close_price': 2,  # 这个是在卖出时才添加的信息，买入时不需要
                       'close_datetime': 'xxxx'}  # 这个也是
        }
        self.history_orders = {}
        self.order_number = 0  # 用于计算order_id

```

### 2、主函数

```
if __name__ == "__main__":
    trade = AstockTrading("myStrategy")
    trade.get_history_data_from_local()
    cur_time = datetime.now()
    while time(9, 30) < cur_time.time() < time(11, 30) or time(13, 30) < cur_time.time() < time(15):
        last_tick = getTick()
        trade.bar_generate(last_tick)
        print(trade.Dt[0], "HIGH:", trade.High[0], "LOW:", trade.Low[0], "CLOSE:", trade.Close[0])
        trade.strategy()
        sleep(3)

```
