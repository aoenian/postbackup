---
title: Vim系统剪切板
date: 2016-08-08 13:09:39
categories: Editor
tags:
- clipboard
- Vim
---

学习了Vim以后感觉挺好用的，现在基本的编辑文本都用Vim来处理。不过也有一些不是太方便的地方：比如和其他程序间的复制、粘贴。这个就比较麻烦,这次就是解决这个问题（也是平常网络上说的很多的Vim的`+`号剪切板）。由于Windows下面使用的Gvim基本不存在这种问题，下面说一下Mac和Linux。

2018-06-24更新：Mac下直接使用 `Command+C` `Command+V` 即可。

**下面说的情况全部在终端环境，不习惯用图形化的，个人感觉不方便。**

<!-- more -->

**在查看之前请首先在终端执行`vim --version`，确认vim是否能够使用系统剪切板。如果有出现`+xterm_clipboard`或`+clipboard`，则可以直接使用，如果前面是`-`号，那么请参考下面的方法解决。**

### Mac

在终端安装一个比较全活的、新的Vim。直接执行如下命令：

``` bash
brew install vim
```

*提示没有brew命令，落伍了吧，点这里：[brew](http://brew.sh/index_zh-cn.html)*

### Linux

思路是一样的，装一个靠谱的。这里以debian系列为例

``` bash
sudo apt-get install vim-gtk
# 或者下面的命令
sudo apt-get install vim-gonme
# 目的就是安装一个比较全的Vim，也可以自己编译安装，不过感觉麻烦我没试过。
```

*如果以上没能解决问题，请参考http://vim.wikia.com/wiki/Accessing_the_system_clipboard*
