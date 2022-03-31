# 数据模拟

有时候，我们可能无法获得大量真实数据，但为了解决代码的测试问题，我们可以来模拟一些数据。

## 数据下载

以5min为周期的bar数据：[https://box.nju.edu.cn/f/be01e1fff94c44179ba3/?dl=1](https://box.nju.edu.cn/f/be01e1fff94c44179ba3/?dl=1)

![600036\_5m.csv](<../.gitbook/assets/image (12) (1).png>)

## 模拟任务

根据这个以5min为周期的bar数据，模拟出每一秒的数据。

要点：

* 确保每个周期内的“开高低收”数据和提供的数据文件一致；
* 使用随机数生成方法，生成每一秒价格。

### 一个简单的思路

![](<../.gitbook/assets/image (6) (1) (1).png>)

Open、High、Low、Close数据是文件中已经提供的，我们可以只需要模拟出剩下的数据即可，确保数据范围不高于High且不低于Low，并且每个周期的第0秒是Open价格，且最后一秒是Close价格。

我们用row来代表一行数据（也就是一个周期），然后维护一个列表：

1. 列表第一个元素为Open价格；
2. 后面生成若干Open到High的随机数，然后把High价格加入；
3. 后面再生成若干High到Low的随机数，然后把Low价格加入；
4. 再生成若干Low到Close的随机数；（偷懒一点的话，这一步可以省略）
5. 将Close作为最后一秒价格加入列表。

### 随机数生成

函数原型：numpy.random.uniform(low,high,size)

功能：从一个均匀分布\[low,high)中随机采样，注意定义域是左闭右开，即包含low，不包含high.

参数介绍:

* low: 采样下界，float类型，默认值为0；&#x20;
* high: 采样上界，float类型，默认值为1；&#x20;
* size: 输出样本数目，为int或元组(tuple)类型，例如，size=(m,n,k), 则输出m_n_k个样本，缺省时输出1个值。

返回值：ndarray类型，其形状和参数size中描述一致。
