---
title: Codewars网站刷题笔记3
date: 2019-11-10 17:15:22
categories: Python
tags:
- codewars
---

## Calculate BMI

题目很简单，这里面有意思的还是如何简化if-else语句，按照一般的套路我也想到了使用列表或者字典之类，但是呢如何处理还是没想到的。

```python
['Underweight', 'Normal', 'Overweight', 'Obese'][(b > 30) + (b > 25) + (b > 18.5)]
```

<!--more-->

## Sort and Star

这个放在这里主要是因为自己对字符串方法还是不熟，这里记录一下

- list(str) 直接把字符串变为列表
- join 不仅能够连接列表也能连接字串
- min() 和 max() 不仅仅局限比较数字

字符串的操作很多，还是要浏览文档有一个大概了解，[点这里](https://docs.python.org/zh-cn/3/library/stdtypes.html?highlight=str#textseq)

## Draw stairs

题目解决容易，但是做得优雅就比较考验能力了，这里要习惯使用列表生成式或者生成器。他们的区别不大，主要就是外边的括号是[]还是()。

最佳答案使用了生成器来处理`'\n'.join(' '*i+'I' for i in range(n))`。还有就是要习惯返回结果而不是总是用输出的思路来解决问题。

## SetAlarm

Python中可以直接使用**and or not**来连接处理逻辑问题。

## My head is at the wrong end

把列表倒排的几种方法：`l.reverse() list(reversed(l)) l[::-1]`

## Multipleofindex

这个题目中使用了列表生成式，`[l[i] for i in range(1, len(l)) if l[i] % i == 0]` 虽然没有考虑列表中第一个元素如果是0的情况，不过列表生成式是简单好用。

## Simplevalidationofausernamewithregex

这个涉及到了正则表达式，在编程的时候思路总是朝着比较直观的方法，没有养成效率和优雅的习惯，当然主要原因就是练得太少。

附正则表达式的[全集](https://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
