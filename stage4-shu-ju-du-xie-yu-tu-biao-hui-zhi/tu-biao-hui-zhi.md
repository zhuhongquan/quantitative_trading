# 图表绘制

## 安装绘制包

matplotlib 是Python的绘图包，其中包含大量的绘图工具，可以通过它进行多种图表的绘制。数据可视化也是Python在科学领域中迅速发展的原因之一。

安装命令如下：

```
pip install matplotlib
```

## 常用API及参数介绍

一般我们用到的是matplotlib库下面的pyplot，因此，导入方式为：

```
import matplotlib.pyplot as plt
```

### plt对象的属性设置

* plt.xlabel()：设置x轴的标签，plt.ylabel()：设置y轴标签。
* plt.title：设置图的标题。
* plt.xlim():设置x轴的起始坐标
* plt.ylim():设置y轴的起始坐标。
* plt.legend()：显示图例，即图中表示每条曲线的标签和样式的矩形区域。

### plt.plot(x,y)

> 详细用法可参考：[https://zhuanlan.zhihu.com/p/258106097](https://zhuanlan.zhihu.com/p/258106097)

plt.plot(x, y, format\_string, \*\*kwargs)

* x：x轴数据，列表或数组，可选；
* y：y轴数据，同上；
* format\_string：控制曲线的格式字串，format\_string 由“<mark style="color:orange;">颜色字符</mark>，<mark style="color:orange;">风格字符</mark>，和<mark style="color:orange;">标记字符</mark>”三个字符组成。
* \*\*kwargs：\*\*kwargs是键值参数，相当于一个字典，比如你输入参数为：(1,2,3,4,k,a=1,b=2,c=3)，\*args=(1,2,3,4,k)，\*\*kwargs={'a':'1,'b':2,'c':3}。

#### format\_string介绍：

![颜色字符](<../.gitbook/assets/image (9).png>)

![风格字符](<../.gitbook/assets/image (4).png>)

![标记字符](<../.gitbook/assets/image (11).png>)

### plt.figure()

matplotlib中最大的概念就是figure，一个figure就是一幅图，可以把它理解成一个有大小的画布。

那么，下面的问题自然是：如何获得一个figure？如何在figure上画图？首先，我们来看看获得一个figure的办法：

```python
fig1 = plt.figure(num='vae', figsize=(10, 3), dpi=75, facecolor='#FFFFFF', edgecolor='#0000FF')
```

当然，**你可以直接用plt.figure(...)来设置plt的一些figure属性**，下面来看一下各个参数的含义：

figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True)&#x20;

* num: 图像编号或名称，数字为编号 ，字符串为名称；
* figsize: 指定figure的宽和高，单位为<mark style="color:red;">英寸</mark>；
* dpi参数指定绘图对象的分辨率，即每英寸多少个像素，缺省值为80, 1英寸等于2.5cm, A4纸是 21\*30cm的纸张；
* facecolor: 背景颜色；
* edgecolor: 边框颜色 frameon:是否显示边框。

## 绘制价格折线图

```python
import matplotlib.pyplot as plt
from dateutil.parser import parse
import pandas as pd

def read_data(path):
    data = []
    df = pd.read_csv(path)
    for index, row in df.iterrows():
        # 筛选你想要的数据
        data.append([row['tradeDate'], row['closePrice']])
    return data

def draw():
    data = read_data("./data/600036_half_year.csv")
    # 将时间字符串转换成时间格式，如果直接使用字符串会怎样？
    time = [parse(line[0]) for line in data]
    price = [line[1] for line in data]
    # 画布属性设置，分辨率参数-dpi，画布大小参数-figsize
    # plt.figure(dpi=72, figsize=(15, 6))
    # 定义title, x y轴
    plt.title('Price Line')
    plt.xlabel('Time')
    plt.ylabel('Price')
    # 分别设置x, y轴数据
    plt.plot(time, price, 'k-')
    plt.show()

if __name__ == '__main__':
    draw()
```
