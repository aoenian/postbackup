---
title: Emacs配置-Org导出HTML代码高亮显示
date: 2018-10-04
categories: Editor
tags:
- Org
- highlight
---

前端时间一直用Emacs编辑Org模式的文档，然后导出html文件用浏览器导出为pdf。但是发现导出的html文件没有代码高亮，搜索了一下，发现方法比较简单，安装 `htmlize.el` 包然后在配置文件启用即可。

<!--more-->

具体操作如下：

第一种方案：

- 去htmlize.el的 [github地址](https://github.com/hniksic/emacs-htmlize) 下载 htmlize.el文件放在你的 `~/.emacs.d` 中。也可以直接执行如下命令直接下载。
  `wget https://raw.githubusercontent.com/hniksic/emacs-htmlize/master/htmlize.el`

- 然后在配置文件中加入 `(require 'htmlize)` 启用插件。

第二种方案：（直接安装，但我没有试过）

- 打开Emacs然后输入 `M-x package-refresh-contents` 刷新包信息

- 然后输入 `M-x package-list` 列出可安装的所有包，然后用 `C-s` 查找一下 `htmlize.el` 点击安装即可。

第二种方案需要配置包管理，我使用的子龙山人的配置，具体教程在 [Master Emacs in 21 Days](http://book.emacs-china.org/#orgheadline2) 也可以使用我的配置文件，位置在 [myconfigfiles](https://github.com/aoenian/myconfigfiles)
