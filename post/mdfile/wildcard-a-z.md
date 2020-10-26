---
title: Shell中通配符[a-z]为什么会匹配大写字母
date: 2020-04-25 17:26:55
categories: Linux
tags:
- [a-z]
- [A-Z]
---

在学习通配符和扩展符的时候发现了问题，[a-z]按照理论来说应该匹配所有的小写字母，但是在实际操作过程中不仅匹配小写而且同时匹配了大写的问题。

## [a-z]和[A-Z]的问题

```bash
$ touch a b c x y z A B C X Y Z
$ ls
A  B  C  X  Y  Z  a  b  c  x  y  z
$ ls [a-z]
A  B  C  X  Y  a  b  c  x  y  z
$ ls [A-Z]
A  B  C  X  Y  Z  b  c  x  y  z
```
<!--more-->
从上面的例子可以看出，本来应该只显示小写子母命名的文件或者大写字母命名的文件，但是结果和想像出现了差别。

咱们先说如何解决这个问题。

## 问题解决方法

### 使用POSIX字符集

```C
[:alnum:]：匹配字面和数字字符。等同于A~Z,a~z,0~9
[:alpha:]：匹配字母字符。等同于A~Z，a~z
[:blank:]：匹配空格或制表符
[:cntrl:]：匹配控制字符
[:digit:]：匹配十进制数字。等同于0~9
[:graph:]：匹配ASCII码值范围33~126的字符。与[:print:]相似，但不包括空格字符
[:print:]：与[:graph:]相同，但多了空格字符
[:lower:]：匹配小写字母，等同于a~z
[:upper:]：匹配大写字母，等同于A~Z
[:space:]：匹配空白字符（空格和制表符）
[:xdigit:]：匹配十六进制数字。等同于0~9，A~F，a~f
```

例如：

```bash
# 这里注意 [:lower:] 相当于 a-z 外边还要加上 [] 来进行任意字符匹配
$ ls [[:lower:]]
a  b  c  x  y  z
$ ls [[:upper:]]
A  B  C  X  Y  Z
```

### 修改LC_ALL变量的值

```bash
# 建议使用第一种方法
$ LC_ALL=C    # 设置LC_ALL变量为C 或 使用 LANG=C也可以
$ ls [a-z]
a  b  c  x  y  z
$ ls [A-Z]
A  B  C  X  Y  Z
$ unset LC_ALL    # 释放变量，取消改变
```

## 简单解释原因

系统通过一些环境变量设置本地偏好，通过命令`locale`可以查看设置摘要。因为语系不同导致编码的顺序不同。

LANG=C：ABC...Zabc...z

zh_CN和en_US：aAbBcC...xXyYzZ

所以会出现开头的问题，由于能够设置的环境变量很多，LC_ALL是覆盖所有其他本地化设置的环境变量（在某些情况下除外$LANGUAGE）。所以一些脚本为了避免用户本地配置的干扰，通常在开头设置`LC_ALL=C`。

>在C语言环境中，字符是单个字节，字符集是ASCII（不是必需的，但实际上将在我们大多数人都会使用的系统中使用），排序顺序基于字节值，该语言通常是美国英语（尽管对于应用程序消息（与月份或日期名称或系统库中的消息相反），它由应用程序作者自行决定），并且未定义货币符号之类的东西。

参考资料

- [POSIX字符类](https://www.cnblogs.com/xiluhua/p/5688627.html)
- [export LC_ALL=C的含义](http://blog.sina.com.cn/s/blog_81b27a5a0101gkqf.html)
- [Linux中LANG,LC_ALL,local详解](https://blog.csdn.net/z4213489/article/details/7937894)
- [Linux中[]方括号匹配问题[A-Z]会匹配小写a-z](https://www.jianshu.com/p/1ec75d18464d)
- [为什么[AZ]匹配bash中的小写字母？](https://qastack.cn/unix/227070/why-does-a-z-match-lowercase-letters-in-bash)
- [“ LC_ALL = C”是做什么的？](https://qastack.cn/unix/87745/what-does-lc-all-c-do)
