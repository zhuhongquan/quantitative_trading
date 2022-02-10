---
description: Python 拥有丰富的第三方库，这使得我们可以站在巨人的肩膀上去探索编程的乐趣。而管理这些第三方库的主要方法就是使用 Python 的 pip 工具。
---

# 用pip来安装第三方库

## 第三方库简介

一般来说，我们要安装的第三方库都是可以在 Python 官方的 Python 库索引网站 [PyPI ](https://pypi.org)找到的，比如后面要用到的科学计算库 numpy：

![](<../.gitbook/assets/image (2) (1).png>)

![](<../.gitbook/assets/image (1) (1).png>)

点击打开numpy的详细页面numpy，可以复制它的安装命令（pip install numpy），在页面左边的 Project links 中还有 numpy 的官网链接和文档链接：

![](<../.gitbook/assets/image (2).png>)

一般来说，我们知道一个第三方库的名字之后，可以直接`pip install xxxx`。不过pip默认使用的国外下载源通常比较慢，可以将其更换为国内的镜像源：\
linux, window: [https://blog.csdn.net/yuzaipiaofei/article/details/80891108](https://blog.csdn.net/yuzaipiaofei/article/details/80891108)\
mac os: [https://blog.csdn.net/qq\_34067821/article/details/84668855](https://blog.csdn.net/qq\_34067821/article/details/84668855)\
推荐使用阿里或者清华的镜像源。\
\
还有一点需要说明的是，有时候当你开启了代理(vpn)时，使用pip会报网络错误，关闭代理即可。

## pip常用命令

> 小提示：部分同学的linux/mac系统含有python2，则在为python3安装对应库的时候，要使用`pip3`，运行python3程序时也要使用`python3`命令

以 numpy 为例：

* pip 安装第三方库：

```
pip install numpy
```

* 安装指定版本：

```
pip install numpy==1.22.2
```

* pip 更新第三方库：

```
pip install --upgrade numpy
```

* pip 卸载第三方库：

```
pip uninstall numpy
```

* pip 查看库的详细信息：

```
pip show numpy
```

* pip 查询已安装的库的清单：

```
pip list
```
