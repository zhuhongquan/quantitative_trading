# 运行第一个程序

## Python的两种编程方式

Python 是一种解释型的脚本编程语言，这样的编程语言一般支持两种代码运行方式：

1. **交互式编程**：在命令行窗口中直接输入代码，按下回车键就可以运行代码，并立即看到输出结果；执行完一行代码，你还可以继续输入下一行代码，再次回车并查看结果……整个过程就好像我们在和计算机对话，所以称为交互式编程。
2. **编写源文件**：创建一个源文件，将所有代码放在源文件中，让解释器逐行读取并执行源文件中的代码，直到文件末尾，也就是批量执行代码。这是最常见的编程方式，也是我们**要重点学习的**。

关于以上两种编程方式的具体介绍，可以查看：[http://c.biancheng.net/view/7032.html](http://c.biancheng.net/view/7032.html)

## 编写第一个程序

分别使用以上两种方式来编写并运行一个程序，下面是一段非常简单的代码：

```python
a = 5
b = 10
c = a*b
print(c)
```

### 交互式编程

Spyder的右下角是交互式编程区域，在此区域内，我们逐行输入代码。可以观察到，前三行并没有给出任何输出，当输入最后一行print时，程序给出了计算结果。

![](<../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png>)

### 编写源文件

直接在左侧的编辑器中写入以上完整代码（# 开头的行以及三引号之间的内容都是注释）：

![](<../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png>)

然后点击"run"，运行该文件，终端便输出了结果。

![](<../.gitbook/assets/image (6) (1) (1) (1) (1).png>)

## Tips

大家应该都已经发现了Python与Java/C/C++之类语言的不同，它的代码并不一定要求被包裹在“函数(方法)”里，但实际上，为了提高应用的模块性和代码的重复利用率，我们一般也需要将Python代码组织成函数模块的形式：[https://www.runoob.com/python3/python3-function.html](https://www.runoob.com/python3/python3-function.html)。
