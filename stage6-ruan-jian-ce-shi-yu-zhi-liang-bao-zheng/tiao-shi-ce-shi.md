---
description: 本节将简单介绍一下测试的概念，学习如何在Python中进行单元测试以及如何在PyCharm中进行调试（Debug）。
---

# 测试与调试

## 测试

软件产品中可能会隐藏着许多<mark style="color:red;">错误</mark>和<mark style="color:red;">缺陷</mark>。尤其对于规模大、复杂性高的软件，造成的危害可能更多、 更大。在这些错误中，有些是致命性的错误，如果不排除，就会导致生命与财产的重大损失。<mark style="color:orange;">软件测试是保障软件质量的有效手段</mark>。

### 为什么要进行软件测试？

![软件无处不在](<../.gitbook/assets/image (7).png>)

### 什么是软件测试？

软件测试是<mark style="color:orange;">**为了发现错误而执行程序的过程**</mark>。

软件测试是根据软件开发各阶段的规格说明和程序内部结构而精心设计的一批测试用例（即输入数据及预 期的输出结果），并利用这些测试用例去运行程序，以发现程序错误的过程。

### 一些测试术语

**测试用例( Test Case）**

<mark style="color:orange;">是为某个特殊目标而编制的一组测试输入、执行条件以及预期结果</mark>，以便测试某个程序路径或者核实是否满足某个特定需求。

* 测试数据（输入）
* 执行环境（条件）
* 预期结果（评估）

**静态与动态**

**静态方法**是指不运行被测程序本身，仅通过分析或检查源程序的语法、结构、过程、接口等来检查程序的正确性。\
**动态方法**是指通过运行被测程序，检查运行结果与预期结果的差异，分析运行效率和健壮性等性能，这种方法由三部分组成：构造测试实例、执行程序、分析程序的输出结果。

**白盒与黑盒**

**白盒测试：**通过程序的源代码进行测试而不使用用户界面。 这种类型测试需要从代码句法发现内部代码在算法、溢出、路径、条件等中的缺点或者错误，进而加以修正。 \
**黑盒测试：**通过使用整个软件或某种软件功能来进行测试。测试人员通过输入他们的数据然后看输出的结果从而了解软件怎样工作。测试者在程序接口进行测试，它只检查程序功能是否按照需求规格说明书的规定正常使用，程序是否能适当地接收和正确的输出。

**测试与调试(Debug)**

测试的目的是显示存在故障；调试的目的是发现故障所在，并修复程序。 \
**调试通常在测试之后**。 \
测试和调试在**目标、方法和思路**上都有所不同:&#x20;

1. 测试从已知条件开始，使用预先定义的过程，有预知的结果。调试从未知条件开始，结束的过程不可预计。&#x20;
2. 测试过程可以事先设计，进度可事先确定。调试不能描述过程或持续时间。
3. 测试是显示错误的行为。调试是推理的过程。&#x20;
4. 测试显示开发人员的错误。调试是开发人员为自己辩护。
5. 测试能预期和可控。调试需要想象、经验和思考。
6. 测试能在没有详细设计的情况下完成。没有详细设计的信息调试不可能进行。
7. 测试能由非开发人员进行。调试必须由开发人员进行。

### 测试与软件开发阶段的关系

软件开发过程是一个自顶向下、逐步细化的过程，而测试则是依相反的顺序安排的，自底向上、 逐步集成。低一级为上一级测试准备条件。当然不排除两者平行地进行。

![](<../.gitbook/assets/image (11).png>)

## 如何进行单元测试？

### 前言

在现代大型项目开发中，往往将软件的不同模块或功能进行拆分，交由不同的开发者去完成。对于开发者而言，要尽量保证自己所编写的代码模块的质量。那么单元测试是必不可少的。

单元测试(Unit Testing)是根据特定的输入数据，针对**程序模块**输出的正确性进行验证的工作。这些程序模块包括：

* 单个程序
* 函数
* 类
* ……

我们在实现一个程序时不能仅仅实现功能方面的端到端调试，仅仅是能够从数据输入到数据输出能够实现贯通是远远不够的。还要保证每个最小模块能够按照对应的输入能够实现正确的输出，这样我们就需要设定一些测试数据来验证程序的正确性。

举个例子，参加过IT、互联网行业的同学应该都有过刷题的经历，例如，比较知名的LeetCode，我们实现一项功能，LeetCode会提供多个测试用例去验证我们程序的输出和预期输出是否形同，以此来验证我们编写程序的正确性。

可想而知，这一系列的工作是无法手动完成的，因此，很多自动化测试工具就应用而生了。在Python中比较知名的有以下几种单元测试工具：

* unittest
* pytest
* doctest
* nose
* ……

本节课的主角就是**unittest**，这是一款受到**JUnit（JAVA中的测试框架）**的启发、与其他语言中的主流单元测试框架有着相似的风格。其支持测试自动化，配置共享和关机代码测试。在Python自动化测试中使用频率较高，后续还会讲到**pytest**和**doctest**。

### 基本示例

下面先看一个unittest单元测试实例：

demo.py

```python
class demo:
    def add(self, x, y):
        return x + y

    def minus(self, x, y):
        return x - y

    def is_string(self, s):
        return type(s) == str

    def raise_error(self):
        raise KeyError("test.")
```

首先，我在这里实现了一个用于测试的类demo，它包含4个方法，分别是加法add，减法minus，他们返回的是一个确切的数值；其次，是一个判断是否为字符串的方法is\_string，返回的是一个布尔型的结果；最后是抛出异常的raise\_error方法，返回的是一个异常类型。

我们接下来就要测试`demo`中的3个方法是否按照我们期望的那样获取正确的结果，也就是设计测试用例：

* `add`：输入数据为1、2，如果功能正确，返回值是3；
* `minus`：输入数据为3、2，如果功能正确，返回值是1；
* `is_string`：输入数据为“hello world!”，如果功能正确返回的是True；
* `raise_error`：直接抛出异常；

明确了我们要测试的方法和重点，接下来就是写测试用例，在这个示例中我的测试用例是这样写的：

**插曲：什么是断言(assert)？**

当你对类，模块或者方法的可以接受的输入和输出有着很明确的定义和认识，当程序的输出结果和输入不匹配时， 你想让程序有一个明确的返回。比如：我明确地知道1+1=2，我现在就想知道我输入1+1会不会得到2，这时候我们通常会使用到断言。

**断言就是专门用来验证输出和期望是否一致的一个工具**。在内容的实现上，它是通过比较一个实际值和一个期望值来实现的。

test\_demo.py

```python
import unittest
from demo import demo


class TestDemo(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        print("this setupclass() method only called once.\n")

    @classmethod
    def tearDownClass(cls):
        print("this teardownclass() method only called once too.\n")

    def setUp(self):
        self.test_class = demo()
        print("do something before test : prepare environment.\n")

    def tearDown(self):
        print("do something after test : clean up.\n")

    def test_add(self):
        """Test method add(a, b)"""
        self.assertEqual(3, self.test_class.add(1, 2))
        self.assertNotEqual(3, self.test_class.add(2, 2))

    def test_minus(self):
        """Test method minus(a, b)"""
        self.assertEqual(1, self.test_class.minus(3, 2))
        self.assertNotEqual(1, self.test_class.minus(4, 2))

    def test_bool_value(self):
        self.assertTrue(self.test_class.is_string("hello world!"))

    def test_raise(self):
        self.assertRaises(KeyError, self.test_class.raise_error)

    @unittest.skip("Don't run as not ready")
    def test_minus_with_skip(self):
        """Test method minus(a, b)"""
        self.assertEqual(1, self.test_class.minus(3, 2))
        self.assertNotEqual(1, self.test_class.minus(3, 2))


if __name__ == '__main__':
    # verbosity=*：默认是1；设为0，则不输出每一个用例的执行结果；2-输出详细的执行结果
    unittest.main(verbosity=1)
```

上述这个例子就概括了unittest的基本使用。

但是！一个函数只有一个测试用例是完全不够的，看下面这个相对复杂的例子（肯定有错误）：

> 题目：这个程序从一个输入框中读取三个整数值，这三个整数值代表了三角形三条边的长度。程序返回信息，指出该三角形是何种三角形：不规则三角形、等腰三角形还是等边三角形。

```python
def triangle(a, b, c):
    if a + b > c and a + c > b and b + c > a:
        if a != b != c:  # 等价于a!=b and b!=c，没有考虑到a!=c
            return '不规则三角形'
        elif a == b and a != c:
            return '等腰三角形'
        elif a == c and a != b:
            return '等腰三角形'
        elif b == c and a != b:
            return '等腰三角形'
        elif a == b == c:
            return '等边三角形'
        else:
            pass
    else:
        return '不是三角形'
```

可通过测试用例(3, 4, 3)来发现错误。

正确写法：

```python
def triangle(a, b, c):
    if a + b > c and a + c > b and b + c > a:
        if a != b and b != c and a != c:
            return '不规则三角形'
        elif a == b and a != c:
            return '等腰三角形'
        elif a == c and a != b:
            return '等腰三角形'
        elif b == c and a != b:
            return '等腰三角形'
        elif a == b == c:
            return '等边三角形'
        else:
            pass
    else:
        return '不是三角形'
```

## 调试

### Debug的前世

在正式讲解之前，先来了解下 debug 这个词的由来，就像我们初学Python 时，先要了解下它历史的由来。

> DEBUG是计算机排除故障的意思。马克2号（Harvard Mark II）编制程序的格蕾丝·霍珀（Grace Hopper）是一位美国海军准将及计算机科学家，同时也是世界最早的一批程序设计师之一。有一天，她在调试设备时出现故障，拆开继电器后，发现有只飞蛾被夹扁在触点中间，从而“卡”住了机器的运行。于是，霍珀诙谐地把程序故障统称为“臭虫（[BUG](https://baike.baidu.com/item/BUG/3353935)）”，把排除程序故障叫DEBUG，而这奇怪的“称呼”，竟成为后来计算机领域的专业行话。如[DOS](https://baike.baidu.com/item/DOS/32025)系统中的调试程序，程序名称就叫DEBUG。
>
> ——百度百科

如今，如果软件在使用的过程中，程序出现了一些错误，就会把这些错误称之为 bug。

### Bug分类

程序中的错误大致分为两种，**编译时错误**和**运行时错误**。

#### 1.编译时错误

这种问题基本都是—些语法错误，哪里有红色波浪线，哪里就有问题。

* a.类名，方法名，变量名写错了
* b.标点符号写错了
* c.括号不匹配
* d.修改了代码，没有保存
* e.方法中少了return语句

以上这些问题都是最基本的语法格式问题，必须知道怎么解决，这不是我们此次课程的重点。&#x20;

编译错误是针对C/C++、JAVA等静态语言而言的；不过Python是动态语言，不存在编译，所以即使有语法错误，也是在运行到对应代码行之后才会报错。

#### 2.运行时错误

程序运行时出现的bug才是我们课程的重点。 程序运行时出现错误的原因就比较复杂了，有下面几种情况&#x20;

* a.程序不能正常执行，报错(如NullPointerException)&#x20;
* b.程序可以正常执行，但结果不对

上述这些bug出现的原因就比较复杂，特别是那种看起来没问题，执行也没报错，但是结果就是不对的代码，很头疼。这个时候就需要我们用到专业的Debug工具来调试程序。

### 如何Debug？

在现代软件开发过程中，我们通常可以借助IDE来进行Debug。IDE提升开发效率主要靠以下几点：

* 编辑器的自动提示，可以大大提高敲代码的速度；
* 代码修改后可以自动重新编译，并直接运行；
* **可以方便地进行断点调试。**

### **什么是断点调试？**

断点调试是指在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住， 然后你可以一步一步往下调试，调试过程中可以看各个变量当前的值，出错的话，调试到出错的代码行即显示错误,停下。进行分析从而找到这个Bug**。（当然，你可以同时设置很多个断点）**

我们以如下的一个例子，来演示如何使用PyCharm的断点调试功能：

```python
def sum_demo(x, y):
    for _ in range(2):
        x += 1
        y += 1
        result = x + y
    return result

if __name__ == '__main__':
    result = sum_demo(1, 1)
    print(result)
```

**演示时间......**

****

### **小试牛刀**

请你找出下面这段代码存在什么问题？

```python
# 传递list给函数，让它在这个list末尾追加一个‘bar’元素
def foo(bar=[]):
    bar.append('bar')
    return bar
```

**怎么测？**

* 给不同的输入
* ...

这里存在一个常见的错误就是**误以为**：函数在每次不提供可选形参时会将参数设置为默认值，也就是本例中的`[]`，一个空的list。

通过肉眼观察，似乎很难知道为什么。

这里我们便可以通断点调试的方式进行追踪，在每次函数调用（不显示传递形参）时，观察形参值的变化。 如图所示为：

![](<../.gitbook/assets/image (8).png>)

**解决方案：**

```python
def foo(bar=None):
    if not bar:
        bar = []
        bar.append('baz')
    return bar
>>>foo()
['baz']
>>>foo()
['baz']
```
