---
title: Codewars网站刷题笔记2
date: 2019-11-06 17:08:36
categories: Python
tags:
- 7kyu
---
这次跳到了7kyu，看看高一级的题目。

## AlphabeticalAddition

这个是字母加法，里面涉及到的就是常用的两个字符和数字的转换方法，`ord()` 和 `chr()`。

然后还有一个就是 `sum(ord(c)-96 for c in letters)` 这个应该属于列表生成式。循环体简化到了一行，不过我个人觉得开始学习的时候看着比较不清楚。

<!--more-->

## DistinctDigitYear

这个找每个位数都不同的年份题目，里面主要使用了一个集合去重复的特性。 `set()` 可以去掉列表或元组中重复的元素。

## InviteMoreWomen

这个题目是处理人数，男士用1表示，女士用-1表示。两个点，一个是计算列表元素个数的`count()`方法，另一个是思路问题，想知道男士和女士哪个性别多直接计算列表的和即可。`sum(list)`

## Powersof3

题目涉及到`math`模块中的`log()`函数，这个是计算根的。数学知识都快忘光了。

```python
log(...)
    log(x, [base=math.e])
    Return the logarithm of x to the given base.
```

## RangeBitCounting

题目是计算给出的整数转换为二进制有几个1，这里涉及到一个进制转换的方法 `bin()` 把十进制转换为二进制。还有剩余两种进制也一并记录。

```python
bin(number, /)
    Return the binary representation of an integer.

oct(number, /)
    Return the octal representation of an integer.

hex(number, /)
    Return the hexadecimal representation of an integer.
```

## FindtheSpeedcuber'stimes

还是思路问题，

- 我的思路是先去掉最大值、最小值再求和。
- 另一种思路是先求和然后再减去最大值和最小值
- 把列表排序然后只取其中的有效的数字

涉及到的方法介绍

```python
min()       # 返回最大值
max()       # 返回最小值
max(...)
    max(iterable, *[, default=obj, key=func]) -> value
    max(arg1, arg2, *args, *[, key=func]) -> value

    With a single iterable argument, return its biggest item. The
    default keyword-only argument specifies an object to return if
    the provided iterable is empty.
    With two or more arguments, return the largest argument.

# 保留小数位方法
round(number, ndigits=None)
    Round a number to a given precision in decimal digits.

'%.2f' % decimal

'{:.2f}'.format(decimal)

format(decimal, '.2f')

```
