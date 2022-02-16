# \*实验1

## 实验内容：

1、完成Python及Spyder的安装，并验证安装是否成功。&#x20;

2、完成git的安装、Gitee的注册。

3、用Python完成两道题目（代码源文件上传到Gitee，同时将代码贴在实验报告中）：&#x20;

（1）编写程序，判断两个字符串是否同构。如果有两个字符串，其中一个字符串的字符重新排列后，能变成另一个字符串，那么称为同构。（注：去除def行之外，即函数体**不允许超过5行代码**）&#x20;

（2）对一个未排序的数组`nums`（元素均为整数），找出最长的连续数字序列的长度（不要求序列元素在原数组中连续）。请你设计并实现**时间复杂度为 O(n)** 的算法解决此问题。（函数体**不允许超过16行代码**）

> 示例1：
>
> **输入**：nums = \[100,4,200,1,3,2]
>
> **输出**：4
>
> **解释**：最长数字连续序列是 \[1, 2, 3, 4]。它的长度为 4。
>
> 示例2：
>
> **输入**：nums = \[0,3,7,2,5,8,4,6,0,1]:
>
> **输出**：9
>
> 提示：
>
> 0 <= nums.length <= $$10^5$$

提示：第一道题目考察Python列表及其相关API的使用，第二道题目考察的是对集合（哈希表）的运用。

附：代码源文件模板

```python
from typing import *

def isReactStr(s1: str, s2: str) -> bool:
    ...

def longestConsecutive(nums: List[int]) -> int:
    ...
```