# 数据获取及保存

## 能够抓取的数据

只要是网页上可以看到的内容，不出意外都是可以抓取的。但是能不能抓得到很大程度上取决于你的爬虫水平。

* 网页文本： 如HTML文档，Json格式文本等
* 图片： 获取的都是二进制文件，如果保存为图片格式
* 视频： 也是也二进制文件，保存为视频格式
* 其他 : 只要你能请求到，就能获取到

## 如何解析网页

![](<../.gitbook/assets/image (4).png>)

### BeautifulSoup提取网页元素

直接用.text获取网页的话，内容很多，格式也比较乱，那我们如何获取特定的信息？——BeautifulSoup

Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库。它能够通过你喜欢的转换器实现惯用的文档导航、查找、修改文档的方式。官方文档：[https://beautifulsoup.readthedocs.io/zh\_CN/v4.4.0/](https://beautifulsoup.readthedocs.io/zh\_CN/v4.4.0/)

BeautifulSoup库基础及一般元素提取方法：[https://www.cnblogs.com/hanmk/p/8724162.html](https://www.cnblogs.com/hanmk/p/8724162.html)

具体问题具体分析，需要根据自己的应用场景选择合适的方法，下面是一个获取新闻正文内容的例子：

```python
import requests
from bs4 import BeautifulSoup

res = requests.get(url="https://finance.sina.com.cn/china/gncj/2022-02-23/doc-imcwiwss2403820.shtml")
res.encoding = "utf-8"  # 一般根据网页的编码来确定(可以试一下不写这行代码的区别)

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

### 数据的保存

先尝试一下最基本的方法：写入本地.txt文件。
