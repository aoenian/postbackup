---
title: Java的replaceAll方法
date: 2018-12-05
categories: Java
tags:
- replaceall
- 正则
- $1
---

今天遇到了一个js的题目，题目主要的意思就是要把字符串替换成用连字符连接的单词，其中不好处理的是字符串中单词的分隔不同。主要是以下几种

- “thisIsTheBook" 以大写字母分隔
- "This_is_The_book" 以下划线分隔
- "this,is,The,book" 以逗号分隔
- "this Is the book" 以空格分隔

<!--more-->

如果按照测试数据通过这个题目没有太大的难度，判断好间隔的符号然后替换为`-`，再转换为小写即可。不过在程序的编写过程中遇到了`replaceAll()`的一些使用问题，具体记录如下：

1. `thisIsTheBook` 这种情况需要把大写字母替换为`-`再加上替换的大写字母。

    这里面使用到了正则表达式中的小括号`(pattern)`，这个圆括号的作用是**匹配pattern并获取这一匹配**。当然在java中可以通过`$1 $2 ... $9`来获取这一匹配。
  
    具体此题目使用示例： `s.replaceAll("([A-Z])","-$1");`

2. 匹配特殊符号如`* + .`等

    因为这些符号在正则中都有使用，如果直接使用则会报错在使用中需要告诉程序你需要替换的是字面的`*`号。这里就需要转义。`split()`方法同样也存在这种问题。

    具体使用示例： `String str = s.replaceAll("\\*", "-");`

参考：

- [replaceAll in a regex with $1](https://stackoverflow.com/questions/43220001/java-replaceall-in-a-regex-with-1)
