# 注释规范

## 重要前提

PEP 只是规范，不是规定也不是语法，切忌死守教条，按图索骥。

## Specification <a href="#specification" id="specification"></a>

### 什么是Docstring？ <a href="#e4-bb-80-e4-b9-88-e6-98-afdocstring-ef-bc-9f" id="e4-bb-80-e4-b9-88-e6-98-afdocstring-ef-bc-9f"></a>

一个docstring是一个string literal，作为模块、函数、类或者方法定义的第一个陈述。这样一个docstring就成为了一个对象的特殊属性\_\_doc\_\_。

为了一致性，一直使用`"""三双引号"""`来写docstrings。如果在docstrings里面使用任意的反斜杠，用`r"""raw triple double quotes"""`。如果用Unicode docstrings，用`u"""Unicode triple-quoted strings"""`。

有两种形式的docstrings，一种是单行docstrings，另一种是多行docstrings。

### 单行docstrings <a href="#e5-8d-95-e8-a1-8cdocstrings" id="e5-8d-95-e8-a1-8cdocstrings"></a>

单行适用于非常明显的可以用单行的场景。比如：

```python
def kos_root():
    """Return the pathname of the KOS root directory."""
    global _kos_root
    if _kos_root: return _kos_root
    ...
```

这样的docstrings是对函数的一种概括（Code Summary），所以比较推荐的描述形式为：

```python
def function(a, b):
    """Do X and return a list."""
```

### 多行docstrings <a href="#e5-a4-9a-e8-a1-8cdocstrings" id="e5-a4-9a-e8-a1-8cdocstrings"></a>

多行docstrings由像单行docstrings的**一组概要行**，**一个空行**和**一个更加详细的描述**组成。概要行可能会被自动索引工具使用。它单独成行，然后由一个空行和剩下的内容分割，这样的形式安排很重要。**概要行可以是在开三双引号这一行，也可以在它的下一行**。整个docstrings都像第一个开三双引号这样的缩进（详见下文例子）。

```python
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """
    if imag == 0.0 and real == 0.0:
        return complex_zero
    ...
```

下面这个是Pycharm中自动生成的格式。

```python
def get_history_data_from_local(self, path):
    """从本地文件中读取历史数据
    
    :param path: 文件路径
    :return: 历史数据列表
    """
    df = pd.read_csv(path)
    for index, row in df.iterrows():
        self.history_data.append(row)
    return self.history_data
```

如果是对一个类添加document，在单行docstrings或者多行docstrings后面添加一个空行。

一个脚本的docstrings应该可以被当作“usage”消息使用。“usage”消息是在脚本使用的时候没有用正确的或缺失的参数的时候打印出来的（或者可能是通过一个“-h”选项）。

一个模块的docstrings通常应该列出**模块输出的类、异常和函**数（以及任何其它对象），每**一个一个单行概要**。

函数或者方法的docstrings应该总结它的行为，归档它的**参数、返回值，副作用，异常**，以及使用的约束。可选参数应该表明。无论关键词参数是不是接口的一部分，都应该归档。

类的docstrings应该总结它的行为，**列出公共方法和实例变量**。如果这个类可能要被作为子类，并且由额外的子类的接口，这个**接口也应该独立的列出来**（在docstrings里面）。

如果一个类是另一个类的子类，并且它的行为和另一个子类很像。那么这个类的docstrings应该总结这一点，并且总结它们的差异。用“override”这个词来表示一个子类方法替换父类方法，并且不调用父类方法。除了自身行为之外，使用“extend”这个词来表示子类方法调用父类方法。

**要点：**

1. Class 的文档字符串，无论单行多行都要在其后插入空行(为了与属性或方法间隔开)
2. 罗列参数时最好插入空行
3. 第一句话可与起始三元引号同行，也可单独一行，结束引号单独成行
4. 模块或者包应在 \_\_ init\_\_.py 中用简要说名可导出名称的用途
5. **对于方法或函数**，文档字符串应包括：功能概述，入参介绍，返回值，效果，附加效果，可能抛出的异常以及被限制使用的条件
6. **对于类**，文档字符串应包括：功能概述，公共方法，实例变量
7. **对于继承类**，务必用 **override** 和 **extend** 标明是覆盖还是扩展





本节参考自PEP 257：[https://peps.python.org/pep-0257/](https://peps.python.org/pep-0257/)
