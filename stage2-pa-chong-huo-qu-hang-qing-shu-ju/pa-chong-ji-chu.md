---
description: 爬虫：请求网站，并提取数据的自动化程序请求网站，并提取数据的自动化程序
---

# 爬虫基础

## HTML

HTML简介：[https://www.runoob.com/html/html-intro.html](https://www.runoob.com/html/html-intro.html)

HTML的本质就是一棵“树”，爬虫的目的就是从这个“树”中获取自己需要的信息。

## 爬虫基本流程

第一步：发起请求。一般是通过HTTP库，对目标站点进行请求（request）。等同于自己打开浏览器，输入网址。

第二步： 获取响应内容（response）。如果请求的内容存在于服务器上，那么服务器会返回请求的内容，一般为：HTML，二进制文件（视频，音频），文档，Json字符串等。&#x20;

第三步：解析内容。对于用户而言，就是寻找自己需要的信息。对于Python爬虫而言，就是利用正则表达式或者其他库提取目标信息。&#x20;

第四步：保存数据。解析得到的数据可以多种形式，如文本，音频，视频保存在本地。

![](<../.gitbook/assets/image (9) (1).png>)

## 请求与响应

爬虫最主要的任务就是发起请求(Request)，然后获取服务器的响应(Response)。

![](<../.gitbook/assets/image (8) (1).png>)

### Request

爬虫的第一步就是发起请求，请求包括如下内容：

* 请求方式： 主要有GET, POST两种类型，另外还有HEAD, PUT, DELETE, OPTIONS等。\
  GET特点：请求的参数全部包含在请求的网址内。\
  POST特点： 需要构造Form Data（也就是参数）才能发起请求。
* 请求URL：URL的全名是统一资源定位符。网络上的一切资源都是位于服务器的某一个位置，而URL就是告知浏览器去哪里获取这些资源。
* 请求头：请求头（header）就是告诉服务器你是谁，包括User-gaget,Host,Cookies等信息。添加请求头信息，保证请求合法
* 请求体：请求时包含的额外数据，如POST请求需要输入的表单数据，一般用于模拟登陆。

![什么是请求](<../.gitbook/assets/image (8).png>)

![console中的request](<../.gitbook/assets/image (10) (1).png>)

### Response

向服务器发出请求后，不出意外，服务器就会返回一个响应（response）。包括如下内容：

* 响应状态：用于表示请求的结果，如200代表成功，404找不到页面，502服务器错误等。
* 响应头 ：如内容类型，内容长度，服务器信息，设置Cookie等等
* 响应体就是网页源代码，也就是用于解析数据的部分。\
  访问网页遇到的第一个文件一般都是document形式，都是网页源代码。然后解析内部的超链接，继续发起请求。

![响应内容](<../.gitbook/assets/image (7) (1).png>)

![console中的response](<../.gitbook/assets/image (3) (1).png>)

## 代码示例

### 1. 用get请求爬取一个网页

```python
import requests

res = requests.get(url="https://finance.sina.com.cn/china/gncj/2022-02-23/doc-imcwiwss2403820.shtml")
res.encoding = "utf-8"  # 一般根据网页的编码来确定(可以试一下不写这行代码的区别)
print(res)  # <Response [200]>  为什么？
print(res.headers)
print(res.status_code)
print(res.text)
```

### 2. BeautifulSoup提取特定网页元素的内容

直接用.text获取网页的话，内容很多，格式也比较乱，那我们如何获取特定的信息？——BeautifulSoup

BeautifulSoup库基础及一般元素提取方法：[https://www.cnblogs.com/hanmk/p/8724162.html](https://www.cnblogs.com/hanmk/p/8724162.html)

具体问题具体分析，需要根据自己的应用场景选择合适的方法，下面是一个获取新闻正文内容的例子：

```python
soup = BeautifulSoup(res.text, features="html.parser")
# 获取网页<head>中的<title>
title = soup.head.title.string
print(title)

# 找到正文
artibody = soup.find(name="div", attrs={'id': 'artibody'})
content = artibody.find_all('p')  # 获取到的是<p>元素对象列表，不是字符串列表
print(content)
# 获取字符串（文本内容）
for i in content:
    print(i.string)  # 使用.string就可以获得字符串了
```
