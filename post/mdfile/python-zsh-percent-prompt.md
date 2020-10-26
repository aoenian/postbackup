---
title: zsh执行python脚本输出的内容最后多了一个%
date: 2019-06-30
categories: Python
tags:
- zsh
- percent sign
---

## zsh执行python脚本出现 %

准备重新拾起python，不仅仅因为他简单优雅，更是因为他能够随手处理很多繁杂的事情，而且也是第一个自己特别喜欢的语言。但是因为种种原因，自己错过他的时间太久了。

重新学习、练习编程的时候出现了一个问题，在一次终端执行python脚本的时候输出文本的最后多了一个`%`，而且**终端还着重显示了**。

<!--more-->

感觉很奇怪，程序的输出并没有这个`%`，然后我使用了`ipython`，在里面用`run`命令执行，发现%并没有出现，看来是终端的问题，在网络上搜索后得到如下结论，简单说就是如果你输出的文本最后没有换行符，那么zsh会在后面加一个反色显示的`%`，如果你是root用户则会看到`#`。具体的官方英文解释如下：

> When a partial line is preserved, by default you will see an inverse+bold character at the end of the partial line: a ‘%’ for a normal user or a ‘#’ for root. If set, the shell parameter PROMPT_EOL_MARK can be used to customize how the end of partial lines are shown.

解决方法：也不能说是解决方法，这也不是问题，如果不想看到，那就在输出的文本最后加个换行符，或者用ipython。

这里多说一点，在编写python脚本的时候，由于经常在Windows和macOS之间切换，所以换行符有点不一样，这里可以使用vscode来更改换行符，右下角直接切换即可。

## mac查看文件换行符

如果想在windows下查看文件的换行符，可推荐notepad++。mac下面没有好用的软件，vim好像也无法很好的显示。经过搜索，mac下查看换行符的方法如下：

```bash
# 文件名可根据需要进行替换
od -c filename.csv
# CR as \r and LF as \n.
```

参考：

- [Why is a percent sign at the end of the output of the python script?](https://stackoverflow.com/questions/36270945/percent-sign-at-the-end-of-the-output-of-python-script)

- [Why ZSH ends a line with a highlighted percent symbol?](https://unix.stackexchange.com/questions/167582/why-zsh-ends-a-line-with-a-highlighted-percent-symbol)

- [Prompting](http://zsh.sourceforge.net/Doc/Release/Options.html#Prompting)

- [How to view (Windows) line ending characters on Mac OSX](https://www.palaniraja.com/2011/06/how-to-view-windows-line-ending-characters-on-mac-osx/)
