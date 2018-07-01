---
title: Vim安装vundle和markdown插件(Windows)
date: 2016-10-06 21:04:37
categories: Editor
tags:
- Vim
- vundle
- markdown
---

**2018-06-24更新：现在编程用VSCode，文章用Org-mode，用pandoc转换为md文件发布**

因为这段时间发现了markdown这个好东西，所以就学习一下，而又在练习vim，所有还是希望能在vim里面也使用markdown语法。下面记录一下配置过程。

由于是刚开始入门vim的使用都还是基本的配置，所以我使用的方法就是最原始的，没有插件管理之类。但是发现vim安装插件还是比较麻烦的，所以就搜索插件管理插件的使用，比较拗口吧。下面先安装vundle插件。

### Vundle插件安装

vim-markdown 地址如下 https://github.com/plasticboy/vim-markdown

不过安装插件之前先安装vundle插件，这样比较方便。当然也可以直接安装。

<!-- more -->

#### 安装前的准备

windows安装vundle的方法可查看[这里](https://github.com/VundleVim/Vundle.vim/wiki/Vundle-for-Windows),如果不想看英文的话可以看看下面我自己备忘的安装教程。Linux和Mac安装比较简单，可以参考官方的文档即可，这里就不多说了。


+ 软件准备

windows版本的git：[下载地址](https://git-for-windows.github.io/)

gvim:[下载地址](http://www.vim.org/download.php#pc)

我用的版本是 git 2.8.4    vim 7.4    大家可以作为参考，如果你用的新版本直接下一步安装即可，英文教程中的那个git安装时注意选择在哪里执行git命令，在新版中已默认是windwos的命令行，所以不用更改就可以。

+ 软件完成后测试安装是否成功

打开cmd执行如下命令

`git --version`

`vim --version`

*如果都返回软件的版本号，那么说明安装成功，如果提示无法找到命令，不好意思请手动添加环境变量。*

+ 安装curl

具体应该说是添加，因为英文文档里面好像说是git里面已经有了，但是如果vundle想用还需要在让curl在命令行里面跑起来。具体的操作如下：

建立一个**curl.cmd**的文件内容如下：

```bash
    @rem Do not use "echo off" to not affect any child calls.
    @setlocal
    
    @rem Get the abolute path to the parent directory, which is assumed to be the
    @rem Git installation root.
    @for /F "delims=" %%I in ("%~dp0..") do @set git_install_root=%%~fI
    @set PATH=%git_install_root%\bin;%git_install_root%\mingw\bin;%git_install_root%\mingw64\bin;%PATH%
    @rem !!!!!!! For 64bit msysgit, replace 'mingw' above with 'mingw64' !!!!!!!
    
    @if not exist "%HOME%" @set HOME=%HOMEDRIVE%%HOMEPATH%
    @if not exist "%HOME%" @set HOME=%USERPROFILE%
    
    @curl.exe %*
```

然后把curl.cmd文件放在git安装目录的相应位置即可默认的话就在`C:\Program Files\Git\cmd\curl.cmd`。*不过我看有的教程里面介绍是新版的git已经包含了curl，具体我没有验证。*

+ 验证curl是否安装成功

打开cmd执行命令：`curl --version`

#### 安装vundle

首先安装完成vim以后查看你的用户目录下是否有：`vimfiles`目录和`_vimrc`文件。windows下的用户目录位置在`C:\Users\yourusername`。

`vimfiles`目录需要保存插件的文件。 `_vimrc`文件是vim的配置文件。**如果没有，请自行建立**

然后打开cmd，执行如下命令：

```bash
    cd %USERPROFILE%
    git clone https://github.com/gmarik/Vundle.vim.git %USERPROFILE%/vimfiles/bundle/Vundle.vim
```

然后你应该在vimfiles目录下看到bundle目录以及里面的Vundle.vim目录，那么就说明到现在你依然走在正确的道路上。 

+ 编辑vimrc文件

这个需要转到vundle的文档里面，地址在[这里](https://github.com/VundleVim/Vundle.vim#quick-start),直接看下面的操作也行。


操作如下：
在你的_vimrc文件里面加入如下内容，当然你的_vimrc文件也在你的用户目录下。

```
    """"""""""""""""""""""""""""""""""""""""""""""""""
    " => 插件管理vundle配置
    """"""""""""""""""""""""""""""""""""""""""""""""""
    set nocompatible              " be iMproved, required
    filetype off                  " required
    
    " set the runtime path to include Vundle and initialize
    " windows系统路径
    set rtp+=$HOME/vimfiles/bundle/Vundle.vim/
    call vundle#begin('$USERPROFILE/vimfiles/bundle/')
    
    " 原文件的配置，适合linux、mac
    " set rtp+=~/.vim/bundle/Vundle.vim
    " call vundle#begin()
    
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')
    
    " let Vundle manage Vundle, required
    Plugin 'VundleVim/Vundle.vim'
    
    " The following are examples of different formats supported.
    " Keep Plugin commands between vundle#begin/end.
    
    " 安装markdown插件
    Plugin 'godlygeek/tabular'
    Plugin 'plasticboy/vim-markdown'
    
    " All of your Plugins must be added before the following line
    call vundle#end()            " required
    filetype plugin indent on    " required
    " To ignore plugin indent changes, instead use:
    "filetype plugin on
    "
    " Brief help
    " :PluginList       - lists configured plugins
    " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
    " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
    "
    " see :h vundle for more details or wiki for FAQ
    " Put your non-Plugin stuff after this line
```

这里我把原来的配置文件中的插件全部删掉了，因为那些插件只是例子，并不是每个人都需要。所以在插件安装里面我只写了vim-markdown。

关于vim-markdown的安装方法具体的地址请点击[这里](https://github.com/plasticboy/vim-markdown)

#### 使用命令安装插件

这里面根据vundle的文档，可以使用两种方法进行插件安装。

```bash
    # 第一种
    打开vim 执行 
    :PluginInstall

    # 第二种
    打开cmd 执行 
    vim +PluginInstall +qall
```

我使用的是第二种，在命令行安装，因为我在gvim里面使用第一种方法执行会出错，大家可以试试在cmd终端中的vim里面执行是否可以。这里需要注意**cmd命令窗口需要能够执行vim命令**

**然后新建一个后缀为md的文档，开始享受输入的过程吧。**

其他的常用命令**以下命令请在cmd中的vim执行**：

```bash
    # 更新所有插件
    :PluginUpdate
    # 更新特定的插件需要在后面加上相应的名字
    :PluginUpdate vim-surround vim-fugitive
```

具体的帮助文档地址：https://github.com/VundleVim/Vundle.vim/blob/master/doc/vundle.txt

#### Chrome查看markdown文件

vim编辑md文件有一个不好的地方就是不能及时的查看，这时候就需要Chrome浏览器大显身手了。

**安装插件总能解决问题-_-**

安装 `markdown preview plus` 地址：https://chrome.google.com/webstore/detail/markdown-preview-plus/febilkbfcbhebfnokafefeacimjdckgl?hl=zh-CN

如果无法访问可以查看这个网站：http://chrome-extension-downloader.com/

**记得在扩展安装的界面勾选上`允许访问文件网址`**

完成，一切OK!

