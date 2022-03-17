---
description: 本节，将介绍 Pandas 最常用的一些功能，几乎在所有的数据处理和分析过程中，都会用到，也是最简单的方法。
---

# 数据读写

## 安装

首先，需要进行安装 pandas 库才可使用它强大的功能（已安装请忽略）。打开「终端」执行以下命令：

```
pip install pandas
# 国外网络慢，可指定国内源快速下载安装
pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 数据集

我们的数据大部分存在于文件当中，所以pandas会支持复杂的IO操作，pandas的API支持众多的文件格式，如CSV、SQL、XLS、JSON、HDF5。

![](<../.gitbook/assets/image (10).png>)

其中，最常用的是HDF5和CSV文件。我们课程中主要使用CSV文件进行数据的存储和读取，下面以CSV文件为例进行介绍。

接下来，我们看看这个数据（下载下来查看内容）：

> [https://box.nju.edu.cn/f/d1824584c6ef46bf8a83/?dl=1](https://box.nju.edu.cn/f/d1824584c6ef46bf8a83/?dl=1)

![](<../.gitbook/assets/image (11).png>)

这是一个学生各季度成绩总表（节选），各列说明：

* name: 学生的姓名，这列没有重复值，一个学生一行，即一条数据，共 100 条；
* team: 所在的团队、班级，这个数据会重复；
* Q1 - Q4: 各个季度的成绩，各个成绩可能会重复；

## 读取数据 <a href="#_3" id="_3"></a>

### read\_csv

* pandas.read\_csv(filepath\_or\_buffer, sep =',', usecols )
  * filepath\_or\_buffer: 文件路径（必填项）
  * sep: 分隔符，默认用","隔开（注意，**" ,"和","是不同的分隔符，留心空格！**）
  * usecols: 指定读取的列名，列表形式

我们将数据读取到 pandas 里，变量名用 `df` DataFrame 的意思，它是 Pandas 二维数据的基础结构。

```python
import pandas as pd # 引入 pandas 库，按惯例起别名 pd

# 以下两种效果一样，如果是网址它会自动下载数据到内存
df = pd.read_csv("https://box.nju.edu.cn/f/d1824584c6ef46bf8a83/?dl=1")
df = pd.read_csv("./data/demo.csv")  # 注意相对路径
# 如果是 .xlsx 的话使用 pd.read_excel() ，还支持很多类型的数据读取
```

然后我们输出`df`看一下内容，验证是否读取正确：

![](<../.gitbook/assets/image (1).png>)

可以发现：

* 自动增加了第一列，是 pandas 为数据增加的索引，从 0 开始，程序不知道我们真正的索引是哪一列，往往需要后边重新指定，否则它会自动添加索引列；
* 由于数据量大，自动隐藏了中间部分，只显示前后五条；
* 最底部显示了列数和行数。

### 建立索引

以上数据真正意义上的索引是 name 列，所以我们需要使它成功索引：

```python
df.set_index('name', inplace=True) # 建立索引并生效
```

**注意**：其中可选参数 `inplace=True` 会直接在原来的内存上改变它的值，将指定好索引的数据再赋值给 `df` ；而 `inplace=False` 相当于另创建了一个数据对象，没有对原来的`df`进行修改。<mark style="color:orange;">请分别尝试设置False/True，然后输出df看看有什么区别</mark>。在pandas中，不写inplace参数的话，<mark style="color:orange;">默认是为False</mark>的，所以你可以<mark style="color:orange;">使用一个新变量</mark>，例如`new_df`<mark style="color:orange;">来获得修改后的df数据</mark>。注意，我们这儿并没有对原 csv 做修改，从我们读取数据后就已经和它没有关系了，我们处理的是内存中 `df` 变量。

```
df.head() # 查看前 5 条，括号里可以传你想看的条数
```

![](<../.gitbook/assets/image (7).png>)

### 数据选取

有时候，我们需要对数据做一些筛选工作。

#### 总体筛选

```python
df.head(n)   # 查看 DataFrame 对象的前n行
df.tail(n)   # 查看 DataFrame 对象的最后n行
df.sample(n) # 查看 n 个样本，随机
df.index     # 查看索引内容
df.columns   # 查看行名
```

#### 选择列

```python
# 查看指定列
df['Q1']
df.Q1 # 同上，如果列名符合 python 变量名要求，可使用
```

输出：

```
name
Liver       89
Arry        36
Ack         57
Eorge       93
Oah         65
            ..
Gabriel     48
Austin7     21
Lincoln4    98
Eli         11
Ben         21
Name: Q1, Length: 100, dtype: int64
```

这里返回的是一个 `Series` 类型数据, 可以理解为数列，它也是带索引的，我们在之前建立了索引，在这儿发挥出了作用，不然我们索引是一个数字，我们无法知道是谁的数据。

```python
# 选择多列
df[['team', 'Q1']] # 只看这两列，注意括号
df.loc[:, ['team', 'Q1']] # 和上边效果一样
```

`df.loc[x, y]` 是一个非常强大的数据选择函数，其中 `x` 代表行，`y` 代表列，行和列都支持条件表达式，也支持类似列表那样的切片（如果要用自然索引需要用 `df.iloc[]`）。下边的例子中会进行演示。

#### 选择行

```python
# 用人工索引选取
df[df.index == 'Liver']     # 指定索引

# 用自然索引选择，类似列表的切片
df[0:3]                     # 取前三行
df[0:10:2]                  # 前10个，每两个取一个
df.iloc[:10,:]              # 前10个
```

#### 指定行列

```python
df.loc['Ben', 'Q1':'Q4']                  # 只看 Ben 的四个季度成绩
df.loc['Eorge':'Alexander', 'team':'Q4']  # 指定行区间
```

#### 条件选择

```python
# 单一条件
df[df.Q1 > 90]           # Q1 列大于90的
df[df.team == 'C']       # team 列为 'C' 的
df[df.index == 'Oscar']  # 指定索引即原数据中的 name

# 组合条件
df[(df['Q1'] > 90) & (df['team'] == 'C')]  # and 关系
df[df['team'] == 'C'].loc[df.Q1>90]        # 多重筛选
```

#### 排序

```python
df.sort_values(by='Q1')                  # 按 Q1 列数据升序排列
df.sort_values(by='Q1', ascending=False) # 降序
df.sort_values(['team', 'Q1'], ascending=[True, False]) # team升，Q1降序
```

## 写入数据

### to\_csv <a href="#12-tocsv" id="12-tocsv"></a>

* DataFrame.to\_csv(path\_or\_buf=None, sep=', ’, columns=None, header=True, index=True, mode='w', encoding=None)
  * path\_or\_buf: 文件路径
  * sep: 分隔符，默认用","隔开
  * columns: 选择需要的列索引
  * header: boolean or list of string, default True,是否写进列索引值
  * index: 是否写进行索引
  * mode: 'w'重写,  'a' 追加。

接着上面的例子，将`df`按照 Q1 列数据降序排列，然后选择前20行数据（表头及表头后的20行）写入`demo_copy.csv`。

踩坑记录：当你发现排序没有起作用时，想一想为什么？（前面强调的一个内容）
