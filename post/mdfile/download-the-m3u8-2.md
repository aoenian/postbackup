---
title: 记一次视频下载过程2
date: 2019-09-02
categories: Python
tags:
- pyenv
- vim
---

这篇文章基本跟视频下载没啥关系了，主要是一些环境的配置和编辑软件的使用。不想每个单写了，所以就写在一起，省点地方。

## Vim自动生成连续数字列

这个是生成视频名称的时候使用的，因为想生成如 001.ts 002.ts 003.ts ... 之类，并且需要每一个视频名称占一行，其实就是制作下载地址的文件。

<!--more-->

本来想用python写个循环，不过树莓派没有安装python3环境，突然想到vim，一搜还真有这种方法。

具体查看地址：[在vim中插入连续数字列](https://blog.csdn.net/cwcmcw/article/details/44498147)

方法摘录如下：

1. 首先是Insert模式输入起始数字如 1.ts
2. 然后按ESC回到Normal模式，按`qa`进入recording状态，录制后面的操作，并把动作录制在寄存器a中
3. 按Y复制整行
4. 按p粘贴刚才复制的内容
5. 按`ctrl+a`则会使数字自动加1
6. 按q停止录制

这时刚才的增加一行并把数值增1的一系列操作已经录制并存放在a寄存器中，在normal模式下，通过命令`100@a`调用，100是次数可根据需要进行更改。

**这里有个问题要注意**，如果你的数字是001、002...这种情况，这可能会出现增加到007以后下一个数字是**010**；增加到017后下个数字变成**020**。开始我以为是vim出问题了或者我的生成序列的方式不对，后来才知道原因是**vim把0开头的数字自动认为是八进制**，解决方法是使用这个命令：`:set nrformats-=octal` 或者 `:set nrformats=`  一个应该是删除八进制，另一个让数字默认都是十进制

## ls命令按照文件名的数字大小排序

这个需求紧接上个问题，如果你的文件名是这样的：001，...010，011，...0100，0101 这种情况如果你用`ls -l`命令查看时，则会出现 0100文件排在011文件之前的情况。这个时候如果想让文件按照文件名的大小排列可执行命令如下：`ls | xargs stat -c "%n" | sort -n`

## 安装pyenv来实现多版本python

建议直接查看官方介绍[pyenv](https://github.com/pyenv/pyenv#basic-github-checkout)。这里只是把步骤翻译出来。

### macOS安装

1. 安装Homebrew 在命令行执行如下命令

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

2. 安装pyenv

    ```bash
    brew update             # 更新brew软件列表
    brew install pyenv      # 安装pyenv
    brew upgrade pyenv      # 更新pyenv
    ```

### Linux

这里以Debian/Ubuntu系统的默认Bash为例，其他的请参照官方文档

```bash
# 更新系统软件包
sudo apt-get update
# 安装依赖文件
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
# 把pyenv安装在自己的home目录下
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 设置环境变量
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
# 添加自动补全
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
# 重启终端
exec "$SHELL"
# 安装python版本,版本可自行选择
pyenv install 3.7.0
```

pyenv的使用方法参见[pyenv commands](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)。

参考文章:

- [Increasing or decreasing numbers](https://vim.fandom.com/wiki/Increasing_or_decreasing_numbers)

- [VisIncr : Produce increasing/decreasing columns of numbers, dates, or daynames](https://www.vim.org/scripts/script.php?script_id=670)

- [vim usr_26.txt](http://vimcdoc.sourceforge.net/doc/usr_26.html)

- [vim 使用counts做简单的加减](https://blog.csdn.net/baiyu9821179/article/details/70198816)

- [按照文件名的数字大小排序文件](https://blog.csdn.net/xiangqiao123/article/details/38707653)
