# 设计一个朴素的策略

## 一些概念

### K线图（柱子）

首先我们找到**该日或某一周期的最高和最低价**，垂直地**连成一条直线**；然后再找出**当日或某一周期的开市和收市价**，把这二个价位**连接成一条狭长的长方柱体**。假如当日或某一周期的收市价较开市价为高（即低开高收），我们便以红色来表示，或是在柱体上留白，这种柱体就称之为“阳线”。如果当日或某一周期的收市价较开市价为低（即高开低收），我们则以绿色表示，又或是在柱上涂黑色，这柱体就是“阴线”了。

![](<../.gitbook/assets/image (5) (1) (1) (1).png>)

### MA

MA是“移动平均线”的简称，后面的数字：5、10、20.....是时间周期。例如，若以天为单位，则MA10为前十天平均价格，如何计算？——大盘10天的收盘价格再除以10即可。

看一个例子：[宁德时代](https://wallstreetcn.com/c/chart?symbol=300750.SZ\&interval=1D\&description=%E5%AE%81%E5%BE%B7%E6%97%B6%E4%BB%A3)

## 一个策略

当股票价格低于MA(5/10/15/20..)一定百分比时，进行买入操作；当股票高于MA(5/10/15/20...)一定百分比时，进行卖出操作。

会有什么问题？——以招商银行为例...

我们现在的目的是用代码实现策略，所以能否盈利不是我们关注的内容。

## 实现

### 流程图

![](<../.gitbook/assets/image (6) (1) (1) (1).png>)

### 代码模块设计

```python
import requests
from datetime import datetime, time
from dateutil.parser import parse
from time import sleep

def getTick():
    """获取最新股票价格"""
    headers={"Referer": "http://finance.sina.com.cn"}
    response = requests.get(url="https://hq.sinajs.cn/?format=text&list=sz300750", headers=headers)
    stock_info = response.text.strip().split(",")
    last = float(stock_info[3])
    trade_datetime = stock_info[30] + " " + stock_info[31]
    trade_datetime = parse(trade_datetime)  # 将时间字符串转为datetime对象
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

    # 每读取到一个tick，就要把tick[0]-时间存放到Dt的0位置,
    # tick[1]-价格插入Open, High, Low, Close
    # 每1分钟产生一个新的bar,如果每5分钟产生一个新bar，该怎么判断？
    if tick[0].minute != Dt[0].minute:
        print("===============Create a new bar!===============")
        print("Bar_ID:", len(Dt))
        # 创建一个新的bar
        ......
    else:
        # 更新bar的high/low/close
        ......
    return Dt, Open, High, Low, Close

def strategy(Close):
    """
    根据策略决定是否买入/卖出股票
    :param Close: List[float], 存放每个bar的最后价格，第一个元素代表当前bar的最新价格
    :return: None
    """
    # 朴素策略：最新价格<均价*0.998时买入，>均价*1.002时卖出
    # 根据历史数据，计算出均价(ma10)
        if len(Close) < 10:  # TODO: 如果没有10个bar，无法计算ma10，直接return
        return
    # TODO：只有新bar被创建，才需要计算ma10，不用每次都重新计算。且听下回分解
    ma10 = sum(Close[1:11]) / 10  # 除了当前正在更新的bar以外，计算之前10个已经生成的bar的均值（20*5分钟）
    if Close[0] < 0.998 * ma10:  # Close[0]是最新价格
        print("===============进行买入操作===============")
        buy()
    elif Close[0] > ma10 * 1.002:
        if 1:  # TODO: 如果之前买过，有long信号，才能卖，没买过是不能卖的。且听下回分解
            print("===============进行卖出操作===============")
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
    Dt = [datetime(2022, 3, 2, 14, 59),  # 最前面的位置是当前bar的开始时间
          datetime(2022, 3, 2, 14, 58),  # 前一个bar的开始时间
          datetime(2022, 3, 2, 14, 57),
          datetime(2022, 3, 2, 14, 56),
          datetime(2022, 3, 2, 14, 55)]
    Open = [513.64, 513.64, 513.66, 513.63, 513.70]  # 当前bar的开始时间价格
    High = [513.84, 513.64, 513.66, 513.69, 513.70]
    Low = [513.64, 513.64, 513.64, 513.48, 513.59]
    Close = [513.84, 513.64, 513.64, 513.64, 513.59]  # 当前bar的结束时间价格

    cur_time = datetime.now()  # datetime库的用法：https://blog.csdn.net/cmzsteven/article/details/64906245
    while time(9, 30) < cur_time.time() < time(15):
        last_tick = getTick()  # 获取最新的股票数据，一般是每3秒更新一次
        # 获得一个最新价格后就去更新bar
        ...
        # 暂停三秒钟
        ...


```
