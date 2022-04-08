# Python编码风格指南

Guido（吉多）的一个重要的见解是，代码阅读的次数比编写的次数多。这里提供的指南旨在提高代码的可读性，并使各种不同的Python代码一致。如PEP20所说，“易读性非常重要”。

## **代码布局**

### 缩进

**每级缩进使用4个空格**。 连续行应该对齐折叠元素，无论是垂直的Python的隐式行连接圆括号内的，中括号内的，大括号内的，还是使用悬挂缩进\[5]。使用悬挂缩进应注意以下几点：

```python
# Correct:

# 与开始分隔符对齐。
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 添加4个空格(额外的缩进级别)来区分参数。
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# 悬挂缩进应该增加一个层次。
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

```python
# Wrong:

# 当不使用垂直对齐时，禁止在第一行设置参数。
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# 需要进一步的缩进，因为缩进无法区分。
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

if语句条件块足够长时需要编写多行，这个PEP没有明确如何（是否）进一步区分条件行和if语句内的嵌入行。这种情况下，可以接受的选项包括，但不仅限于：

![](<../.gitbook/assets/image (12).png>)

多行结构中的结束花括号/中括号/圆括号是最后一行的第一个非空白字符，如：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```

或者是最后一行的第一个字符，如：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

### **制表符还是空格？**

空格是缩进方法的首选。 制表符仅用于与已经用制表符做缩进的代码保持一致。

Python3不允许混用制表符和空格来缩进。

Python2代码混用制表符和空格缩进，将被转化为只使用空格。

### **行的最大长度**

**限制所有行最多79个字符**。

下垂的长块结构限制为更少的文本**（文档字符串或注释），行的长度应该限制在72个字符**。

有些团队强烈喜欢较长的行长度。对于代码维护完全或主要由一个团队的来进行的情况，可以在这个问题上达成协议，象征性的将行长度从80个字符增加到100个字符（有效地增加最大长度到99个字符）也是可以的，但是注释和文档字符串仍是72个字符。

需要将较长的行进行换行——折叠长行的首选方法是在小括号，中括号，大括号中使用Python隐式换行。长行可以在表达式外面使用小括号来变成多行。连续行**使用反斜杠更好**。

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

**换行应该在二元操作符的前面还是后面？**

过去二十年（指在编写PEP8之前）我们都是推荐放在二元操作符的后面。但是这种做法会以两种方式伤害可读性：

```python
# Wrong:
# operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)
```

为了解决可读性问题，数学家和印刷业者通常是在二元操作符之前换行的。

出于遵循数学传统，所以我们这样写这段代码将更加可读：

```python
# Correct:
# easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

### 空行

顶级函数和类的定义之间有两行空行。

类内部的函数定义之间有一行空行。

额外的空行用来（谨慎地）分离相关的功能组。相关的行（例如：一组虚拟实现）之间不使用空行。

在函数中谨慎地使用空行来表示逻辑部分。

## **源文件编码**

Python核心发布中的代码应该始终使用UTF-8（或Python2中用ASCII）。&#x20;

文件使用ASCII（Python2中）或UTF-8（Python3中）不应有编码声明。

## import

导入通常是单独一行，例如：

```
# Correct:
import os
import sys
```

```
# Wrong:
import sys, os
```

这样也是可以的：

```
# Correct:
from subprocess import Popen, PIPE
```

**导入常常位于文件顶部**，在模块注释和字符串文档之后，在模块的全局变量和常量之前。

导入应该按照以下顺序分组：

1. 标准库导入
2. 相关的第三方导入
3. 特定的本地应用/库导入 在每个导入组之间放一行空行。

## **模块级别的内置属性**

模块级别的内置属性（名字有前后双下划线的），例如**\_\_all\_\_**, **author**, **version**，应该放置在模块的文档字符串后，任意import语句之前，from \_\_future\_\_导入除外。Python强制要求from \_\_future\_\_导入必须在任何代码之前，只能在模块级文档字符串之后。

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

## **字符串引号**

Python中，单引号字符串和双引号字符串是一样的。但不建议如此。

当一个字符串包含单引号字符或双引号字符时，**使用另一种字符串引号来避免字符串中使用反斜杠**。这提高可读性。

三引号字符串，与PEP 257 文档字符串规范一致总是使用双引号字符。

## **表达式和语句中的空格**

以下情况避免使用多余的空格：

（1）紧挨着小括号，中括号或大括号。

```
# Correct:
spam(ham[1], {eggs: 2})
```

```
# Wrong:
spam( ham[ 1 ], { eggs: 2 } )
```

（2）在逗号和右括号之间：

```
# Correct:
foo = (0,)
```

```
# Wrong:
bar = (0, )
```

（3）紧挨在逗号，分号或冒号前：

```
# Correct:
if x == 4: print(x, y); x, y = y, x
```

```
# Wrong:
if x == 4 : print(x , y) ; x , y = y , x
```

（4）在切片中冒号像一个二元操作符，冒号两侧得有相等数量空格（把它看作最低优先级的操作符）。在一个扩展切片中，两个冒号必须有相等数量的空格。例外：当一个切片参数被省略时，该空格被省略。

```python
# Correct:
ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset : upper+offset]
ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
ham[lower + offset : upper + offset]

# Wrong:
ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

（5）紧挨着左括号之前，函数调用的参数列表的开始处：

```
# Wrong:
spam (1)
```

```
# Correct:
spam(1)
```

（6）紧挨着索引或切片开始的左括号之前：

```
# Correct:
dct['key'] = lst[index]
```

```
# Wrong:
dct ['key'] = lst [index]
```

（7）为了与另外的赋值（或其它）操作符对齐，不止一个空格。

```python
# Correct:
x = 1
y = 2
long_variable = 3
```

```python
# Wrong:
x             = 1
y             = 2
long_variable = 3
```

**其他建议**

始终避免行尾空白。因为它们通常不可见，容易导致困惑：如果\后面跟了一个空格，它就不是一个有效的续行符了。很多编辑器不保存行尾空白，CPython项目中也设置了commit前检查以拒绝行尾空白的存在。

始终在这些二元操作符的两边放置一个空格：赋值（= ），增强赋值（+= ，-= 等），比较（== ， < ， > ， != ， <> ， <= ， >= ，in ， not in ，is ，is not ），布尔（and ，or ，not ）。

```python
# Correct:
i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

```python
# Wrong:
i=i+1
submitted +=1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)
```
