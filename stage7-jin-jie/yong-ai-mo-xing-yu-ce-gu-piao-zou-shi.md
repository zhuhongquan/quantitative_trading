---
description: 由于时间关系，这边只能粗略地介绍一下AI模型的一般构建方式以及如何训练模型，关于具体的模型信息，以及代码编写方面的问题，还需要大家课后自己去了解。
---

# 用AI模型预测股票走势

本节内容属于进阶内容，学有余力的同学可以在自己的项目中加上该模块。

## 数据集&#x20;

数据集下载地址：[https://box.nju.edu.cn/f/c7e91e03fb7c488383a0/?dl=1](https://box.nju.edu.cn/f/c7e91e03fb7c488383a0/?dl=1)

我们先来看看数据集，这里包含了一只股票的开盘价，最高价，最低价，收盘价，交易量的信息。

![](<../.gitbook/assets/image (11).png>)

本节基于LSTM对收盘价（close）进行预测，当然你也可以选择开盘价、最高价等等。

## 单维对单步的预测

我们这是用前n天的数据预测第n+1天的数据。 单维单步的含义如下图，假如我们利用2天的数据预测第3天的数据。trainX的形状为（5,2），trainY的形状为（5,1）

![](<../.gitbook/assets/image (10).png>)

所以它和一般的数据集不一样，一般的数据集有一组特定的特征作为trainX，然后再对应一组label，即trainY。而我们对于股票的预测，是通过过往的价格来预测未来的价格，那么未来的价格预测出来后，又作为过往价格，去预测未来的未来，所以价格既是trainX，也是trainY。

## 导入需要的包

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from keras.models import Sequential
from keras.layers import Dense, Activation, Dropout, LSTM
from sklearn.preprocessing import MinMaxScaler
import time
import os
```

## 数据读取及构造

这个函数是创建变量x和y的，因为lstm时间序列不像别的回归一个x，另一个值y，lstm的x和y全是一组数据产生的，也就是它自己和自己比。

```python
def creat_dataset(dataset, look_back=1):
    """把前look_back天的数据作为后面的预测依据，所以依次把前look_back天作为x，后一天作为y"""
    dataX, dataY = [], []
    for i in range(len(dataset)-look_back-1):
        a = dataset[i: (i+look_back)]
        dataX.append(a)
        dataY.append(dataset[i+look_back])
    return np.array(dataX), np.array(dataY)
```

### 读取文件

```python
data = pd.read_csv("./data/zgpa_train.csv")
dataframe = pd.DataFrame(data, columns=['date', 'close'])
dataset = dataframe["close"].values
dataframe.head(10)
```

### 数据归一化

这里就是归一化操作，有一个feature\_range=(0, 1)这个意思是所有数据标准化到0-1区间内，就是归一化的意思，如果(0,100)那就是标准化到0-100这个区间。最后把它转换成列向量

```python
scaler = MinMaxScaler(feature_range=(0, 1))
dataset = scaler.fit_transform(dataset.reshape(-1, 1))
```

### 分割数据集

```python
train_size = int(len(dataset)*0.8)   # 80%作为训练集
test_size = len(dataset)-train_size  # 剩下的作为测试集
train, test = dataset[0: train_size], dataset[train_size: len(dataset)]
```

### 生成trainX和trainY

调用前面函数分别生成训练集和测试集的x，y。

```python
look_back = 1
trainX, trainY = creat_dataset(train, look_back)
testX, testY = creat_dataset(test, look_back)
```

## 建立模型

这里注意下如果是多层lstm的话一定要注意前后维度对接，也就是input和units(即输出维度)维度，当前层的input维度是上一次层的units维度。return\_sequences=True表示返回这个维度给下面。对这一过程想深入研究的同学可以去学习一下keras。

```python
model = Sequential()
model.add(LSTM(input_dim=1, units=50, return_sequences=True))
model.add(LSTM(input_dim=50, units=100, return_sequences=True))
model.add(LSTM(input_dim=100, units=200, return_sequences=True))
model.add(LSTM(300, return_sequences=False))
model.add(Dropout(0.2))
model.add(Dense(100))
model.add(Dense(units=1))
model.add(Activation('relu'))

start = time.time()
model.compile(loss='mean_squared_error', optimizer='Adam')
# 打印模型结构信息
model.summary()
```
