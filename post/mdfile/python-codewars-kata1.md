---
title: Codewars网站刷题笔记1
date: 2019-10-27 20:18:53
categories: Python
tags:
- codewars
- kata
- skills
---

很早就注册了[codewars](https://www.codewars.com/)网站，却一直没有刷题，主要原因是懒，一小部分原因是这个网站访问比较慢。现在准备重新练习python，其实python也是很早就学了，一直没有练习，然后基本就一直处于入门水平，知道简单的语法而已，这次要好好练一次。

以codewars网站上面的题目为基础进行练习，每周都会刷一些题目，然后把这周做题过程中获得的一些技巧和经验，记录在博客中。作为一个系列，归类在python中。这是第一篇。

网站题目的github地址：<https://github.com/aoenian/codewars-katas-python>

<!--more-->

## math.ceil

在CenturyFromYear题目中有解决方案使用如下代码`math.ceil(year / 100)`

这里把相关方法的解释摘录如下：

> math.ceil(x)
>
> Return the ceiling of x, the smallest integer greater than or equal to x. If x is not a float, delegates to x.__ceil__(), which should return an Integral value.

还有一个类似的方法：

> math.floor(x)
>
> Return the floor of x, the largest integer less than or equal to x. If x is not a float, delegates to x.__floor__(), which should return an Integral value.

具体内容可查看相关文档。

## xrange()

在GrasshopperSummation题目中看到了`xrange()`的使用。网络搜索后得到`xrange`和`range`的区别在于**xrange()生成的不是一个数组，而是一个生成器。**

不过通过查找[文档](https://docs.python.org/zh-cn/3/whatsnew/3.0.html?highlight=xrange)发现在3.0版本以后用`range()`的功能与`xrange()`完全一样，而后者已经删除。

## HQ9

这里处理的是字符串的问题。第一，长字串如何比较方便的表示；第二，如何格式化字符串。

- 长字串表示

    这里主要使用的是括号和双引号来处理这个问题。代码示例如下：

    ```python
    # 个人比较推荐这种
    long_str = (
        "2 bottles of beer on the wall, 2 bottles of beer.\n"
        "Take one down and pass it around, 1 bottle of beer on the wall.\n"
    )
    # 也可以使用 \ 符号 但要注意第二行的缩进问题
    long_str = "2 bottles of beer on the wall, 2 bottles of beer.\n \
    Take one down and pass it around, 1 bottle of beer on the wall.\n"
    # 也可以使用三引号来处理
    # 三引号包裹的所有字符都会作为字串的一部分，特别是缩进和换行
    long_str = '''2 bottles of beer on the wall, 2 bottles of beer.
    Take one down and pass it around, 1 bottle of beer on the wall.'''
    ```

- 格式化字符串 format()

搜索文档和相关资料的时候发现有`format()` 和 `str.format()` 两种用法。但是网络上大部分介绍的都是第二种，但是第一种还是内置函数呢，所以就很奇怪。查了一下，在stackoverflow里面看到了相似的问题。具体网址：[what-is-global-format-function-meant-for](https://stackoverflow.com/questions/35060732/what-is-global-format-function-meant-for)

里面的观点摘录如下：

> Use the format() function when you want to format just a single value, use str.format() when you want to place that formatted value in a larger string.

具体的格式字符串语法的文档地址：[格式字符串语法](https://docs.python.org/zh-cn/3/library/string.html#formatstrings) 中文的 不过建议还是看看英文互相对照，感觉有点像是机器翻译的。

## Messi goals function

这个题目本身比较简单，这里记录的原因是如何优雅的解决函数接收参数较多的问题。

```python
# 用可变参数，然后通过sum()函数进行和的计算
def goals(*a):
    return sum(a)
```

## PersonalizedMessage

这个题目中是列表使用的一个小技巧，在进行逻辑判断的时候，False和True分别对应不同的对象，这样可以通过此技巧来优雅处理。具体实现如下：

```python
# 通过判断name和owner是否相等来得到不同的结果
'Hello '+['guest','boss'][name==owner]
```

## RemoveStringSpaces

字符串的替换，推荐使用自带的 replace() 方法。 如：`x.replace(' ' ,'')`

## WellOfIdeasEasyVersion

判断不同条件返回不同结果时可以写在一行处理。

```python
if count > 2:
    res = 'I smell a series!'
elif count == 0:
    res = 'Fail!'
else:
    res = 'Publish!'
return res

# 可以调整为
return 'I smell a series!' if c > 2 else 'Publish!' if c else 'Fail!'
```
