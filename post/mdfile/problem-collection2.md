---
title: 日常使用问题汇总（二）
date: 2019-06-25
categories: Collection
tags:
- dns
- pandoc
- gitment
- Bad credentials
- vscode font
---

## 公共DNS服务器

这个本来是个很正常的事情，几乎在上网的过程中应该是忽略不计的事，但是却让我花费了很多时间寻找。

情况大家都懂，就不多介绍了。这里说几个常见的问题（纯属个人观点，欢迎讨论）：

<!---more-->

1. 如果你对DNS服务器有了解并且想更换dns配置，不建议选择114（_个人经历过劫持，不知道现在是否还有_），但我有时候会临时使用。好记而且速度还不错。
2. 有特殊功能的DNS肯定见光死，建议不要费力寻找，有条件可以自己搭建。
3. 测试DNS速度建议使用`dig`命令，有时`ping`命令并不准确，更有的DNS服务器会拒绝ping。
4. 国外的速度非常慢，建议能不用就不用。国内的几个大的公司凑合找一个用吧。
5. 如果没有特殊需求，不想折腾，运营商默认就很快了（_如果运营商没有给你下套_），少折腾留点时间干点别的，别太较真，容易内伤。
6. [公共dns服务器](https://www.iplaysoft.com/public-dns.html) 收集

## gitment 出现 Error: Bad credentials

这个好解决，使用你的账户登入然后再登出即可搞定。具体的issues在[这里](https://github.com/imsun/gitment/issues/145)。

## vscode更改编程字体不起作用

这几天想把vscode的英文字体更改一下，更适合编程，但是首选项-设置里面的用户设置更改了字体后，不起作用，很郁闷。

刚开始以为字体问题，更换其他的依然无效，但是总觉得这种大bug应该不会一直留到现在吧，google了一下，在官方的issues中找到了原因，具体可查看[Can't set editor font](https://github.com/Microsoft/vscode/issues/50407)，**主要的原因是工作区的配置优先级比较高，所以工作区的字体配置覆盖了用户的配置**，所以把工作区的字体修改为你想要的就可以看到效果了。

修改的方法有两种，一种直接首选项-设置-工作区即可；另一种直接修改配置文件，具体的位置在工作区中的`.vscode`目录下面的settings.json文件。

如果想改变**中文字体**，直接在英文字体后面加上中文字体即可生效。我的配置如下，我使用的中文字体是华文仿宋，可供参考。

```json
"editor.fontFamily": "Hack, STFangsong, Monaco, monospace",
```

## macOS使用pandoc命令出现pdflatex not found错误

本来想测试pandoc命令把markdown文件导出成pdf的质量如何，但是执行命令后出现`pdflatex not found`错误，在网络搜索后找到如下解决方法：

```bash
# 不用安装pdflatex，需要安装mactex
$ brew cask install mactex
# 重启终端后可查看
$ which pdflatex
```

参考博文：

- [Visual Studio Code 设置中文字体](https://segmentfault.com/a/1190000004168301)

- [VSCode--setting](https://zhuanlan.zhihu.com/p/55062528)

- [CSS font-family 各字体一览表](https://www.jianshu.com/p/44ef95b2c86f)

- [where-do-i-get-the-pdflatex-program-for-mac](https://superuser.com/questions/1038612/where-do-i-get-the-pdflatex-program-for-mac)
