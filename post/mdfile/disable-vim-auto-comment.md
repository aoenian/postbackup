---
title: 禁止Vim换行自动添加注释符号
date: 2016-10-08 16:19:19
categories: Editor
tags: 
- Vim
- comment
---

在用vim编写程序的时候换行后总会自动添加注释符号，所以就上网搜索如何去掉vim中换行后自动添加的注释符号。

找了很多终于有个靠谱的，地址为：http://stackoverflow.com/a/23326474 

*总觉得自己遇到的问题都特别奇葩。*

具体操作方法就是在vimrc文件中加入如下命令即可

<!-- more -->

```vim
    augroup Format-Options
        autocmd!
        autocmd BufEnter * setlocal formatoptions-=c formatoptions-=r formatoptions-=o
    
        " This can be done as well instead of the previous line, for setting formatoptions as you choose:
        autocmd BufEnter * setlocal formatoptions=crqn2l1j
    augroup END
```

网络很多直接设置的方法在我这里都没有用，我的vim版本是7.4，而且我两台电脑上都不起作用。有时候找个靠谱的方法太难了。

