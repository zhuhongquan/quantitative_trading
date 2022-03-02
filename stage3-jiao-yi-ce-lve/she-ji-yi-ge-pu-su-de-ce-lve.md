# 设计一个朴素的策略

## 一些概念

### K线图（柱子）

首先我们找到**该日或某一周期的最高和最低价**，垂直地**连成一条直线**；然后再找出**当日或某一周期的开市和收市价**，把这二个价位**连接成一条狭长的长方柱体**。假如当日或某一周期的收市价较开市价为高（即低开高收），我们便以红色来表示，或是在柱体上留白，这种柱体就称之为“阳线”。如果当日或某一周期的收市价较开市价为低（即高开低收），我们则以绿色表示，又或是在柱上涂黑色，这柱体就是“阴线”了。

![](<../.gitbook/assets/image (5).png>)

### MA

MA是“移动平均线”的简称，后面的数字：5、10、20.....是时间周期。例如，若以天为单位，则MA10为前十天平均价格，如何计算？——大盘10天的收盘价格再除以10即可。

看一个例子：[招商银行](https://wallstreetcn.com/c/chart?symbol=600036.SS\&interval=1D\&description=%E6%8B%9B%E5%95%86%E9%93%B6%E8%A1%8C)

## 一个策略

当股票价格低于MA(5/10/15/20..)一定百分比时，进行买入操作；当股票高于MA(5/10/15/20...)一定百分比时，进行卖出操作。

会有什么问题？——以招商银行为例...

我们现在的目的是用代码实现策略，所以能否盈利不是我们关注的内容。

## 实现

### 流程图

![](<../.gitbook/assets/image (6).png>)

### 代码模块设计

```python
import requests
from datetime import datetime, time

def getTick():
    """获取最新股票价格"""
    headers={"Referer": "http://finance.sina.com.cn"}
    response = requests.get(url="https://hq.sinajs.cn/?format=text&list=sh600519", headers=headers)
    stock_info = response.text.strip().split(",")
    last = float(stock_info[3])
    trade_datetime = stock_info[30] + " " + stock_info[31]
    tick = (trade_datetime, last)
    return tick


def bar_generate(tick, Dt, Open, High, Low, Close):
    """
    根据最新的tick，更新当前bar或者生成一个新的bar
    :param tick: getTick()函数返回的一个元组，(trade_datetime, last)
    :param Dt: List[], [datatime(...), ...]
    :param Open: List[float], [45.79, 45.66, 45.72]
    :param High: List[float], 每个bar的最高价格
    :param Low: List[float], 每个bar的最低价格
    :param Close: List[float], 每个bar的最后价格
    :return: Dt, Open, High, Low, Close
    """
    
    last_bar_start_minute = None
    # 每5分钟产生一个新的bar
    if tick[0].minute % 5 == 0 and tick[0].minute != last_bar_start_minute:
        # 创建一个新的bar
        ...
    else:
        # 更新bar的high/low/close
        ...
    return Dt, Open, High, Low, Close

def strategy(Close):
    """
    根据策略决定是否买入/卖出股票
    :param Close: List[float], 存放每个bar的最后价格，第一个元素代表当前bar的最新价格
    :return: None
    """
    # 朴素策略：最新价格<均价*0.95时买入，>均价*1.05时卖出
    # 根据历史数据，计算出均价(ma20)
    if 1:  # TODO: 如果新的bar被创建，才需要计算ma20——且听下回分解
        ...  # 除了当前正在更新的bar以外，计算之前20个已经生成的bar的均值（20*5分钟）
    if Close[0] < 0.95 * ma20:  # Close[0]是最新价格
        buy()
    elif Close[0] > ma20 * 1.05:
        if 1:  # TODO: 如果之前买过，有long信号，才能卖，没买过是不能卖的——且听下回分解
            sell()
    else:    # 如果在均线5%附近波动，什么也不操作
        pass
    return

def buy():
    pass

def sell():
    pass

if __name__ == "__main__":
    # 下面是全局变量，用于存放及构建bar，里面已经填充了一些数据，模拟之前的交易日中的数据，以后我们会正式从文件中读取前面交易日的数据
    Dt = [datetime(2022, 3, 2, 14, 55),  # 最前面的位置是当前bar的开始时间
          datetime(2022, 3, 2, 14, 50),  # 前一个bar的开始时间
          datetime(2022, 3, 2, 14, 45)]
    Open = [45.79, 45.66, 45.72]  # 当前bar的开始时间价格
    Close = []  # 当前bar的结束时间价格
    High = []
    Low = []
    is_new_bar = True

    cur_time = datetime.now()  # datetime库的用法：https://blog.csdn.net/cmzsteven/article/details/64906245
    while time(9, 30) < cur_time.time() < time(15):
        last_tick = getTick()  # 获取最新的股票数据，一般是每3秒更新一次
        # 获得一个最新价格后就去更新bar
        Dt, Open, High, Low, Close = bar_generate(last_tick, Dt, Open, High, Low, Close)
        strategy(Close)
```
