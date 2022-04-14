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

![颜色字符](<../.gitbook/assets/image (9) (1).png>)

![风格字符](<../.gitbook/assets/image (4).png>)

![标记字符](<../.gitbook/assets/image (11) (1).png>)

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
* edgecolor: 边框颜色；
* frameon:是否显示边框。

## 绘制价格折线图

```python
import matplotlib.pyplot as plt
import pandas as pd

def read_data(path):
    data = []
    df = pd.read_csv(path)
    for index, row in df.iterrows():
        # 筛选你想要的数据
        data.append([row['tradeDate'], row['closePrice']])
    return data

def draw_price():
    data = read_data("./data/600036_half_year.csv")
    time = [line[0] for line in data]
    price = [line[1] for line in data]
    # 画布属性设置，分辨率参数-dpi，画布大小参数-figsize
    plt.figure(dpi=72, figsize=(15, 6))
    # 定义title, x y轴
    plt.title('Price Line')
    plt.xlabel('Time')
    # 设置x轴每隔15隔显示坐标
    plt.xticks(range(0, len(time), 15))
    plt.ylabel('Price')
    # 分别设置x, y轴数据
    plt.plot(time, price, 'k-')
    plt.show()

if __name__ == '__main__':
    draw_price()
```

## 绘制量能图

大家自己试一下。

## 合并两张图

刚刚是分别绘制了两个图——价格折线图和量能图，但现在我想把这两个放在一张图中对比，该怎么实现？

这就引出了一个概念，之前的绘图方式都是用`pyplot`的**API方式绘图**的，matplotlib本身是一个面向对象的绘图库，下面介绍一下**使用面向对象绘图方式**绘图，及产生网格，图例显示。

整体思想是**先产生figure画布对象**，**在画布上产生子图对象**，接着调整坐标轴，标题，图例，网格，文字说明等，按照从大到小的顺序。

看一个示例代码：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1,10,1)  # 随便产生一点数据
y = np.random.randn(len(x))

fig = plt.figure()  #产生一个画布
ax = fig.add_subplot(111) # 在画布上创建一个子图
ax.plot(x,y)
ax.set_title("object oriented")  #设置子图标题

plt.show()
```

第8行代码add\_subplot()可能有点不能理解，关于它的介绍请看[https://blog.csdn.net/wyf2017/article/details/109297737](https://blog.csdn.net/wyf2017/article/details/109297737)

### 创建子图

在画布上创建两个子图(一行两列)，只显示一个：

```python
x = np.arange(1,100,1)

fig = plt.figure()
ax1 = fig.add_subplot(121)
ax1.plot(x,x)

plt.show()
```

![](<../.gitbook/assets/image (2).png>)

在画布上创建两个子图(一行两列)，都显示出来：

```python
# 自己写一下代码，第二个子图的y参数设为x^2
```

![](<../.gitbook/assets/image (5) (1).png>)

### **添加网格**

```python
x = np.arange(1,10)

fig = plt.figure()
ax = fig.add_subplot(111)
ax.plot(x,x*2)
ax.grid(color='g',linestyle='--')

plt.show()
```

![](<../.gitbook/assets/image (9).png>)

### **显示图例**

```python
x = np.arange(1,11)

fig = plt.figure()
ax = fig.add_subplot(111)
ax.plot(x,x,label='inline label')  # 显示图例
ax.legend()  # 显示图例

plt.show()
```

![](<../.gitbook/assets/image (5).png>)
