---
title: GithubPages和Hexo建立个人博客
date: 2016-08-08 12:50:39
categories: Github
tags:
- github
- hexo
---

之前一直用现成的博客网站写博客（其实就是记性不好），后来感觉限制太多，而且编辑方式也不是很方便（没有markdown）。也想到建一个自己的网站，不过太麻烦（其实不会弄）。后来发现了githubpages，良心产品，所以就整了一个。

不说废话，第一篇博客还是记录一下自己的配置过程。（下面并没有具体的操作步骤，要学会站在巨人的肩膀）

<!-- more -->

## 参考博客链接

+ [Mac上搭建基于GitHub的Hexo博客](http://gonghonglou.com/2016/02/03/firstblog/)

+ [史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/) 这个是基于**Windows**的。

+ [Windows Git网址](https://git-scm.com/download)

+ [hexo文档](https://hexo.io/zh-cn/docs/)

+ [Next主题文档](http://theme-next.iissnan.com/getting-started.html)
    建议刚开始使用的话选择Next主题，主要是文档详细、丰富。十分适合像我这样的新手使用。

## 一些注意事项

+ 申请githubpages的过程比上面博客中多了几步，不过基本不影响。

+ 安装node.js的时候尽量按照hexo里面教程说的，用nvm安装。(适用于类unix系统)

+ 中间如果出现hexo命令找不到的情况，如果没有安装错误的话，那就是没有选择node的版本。
    请执行`$nvm use 6.2.0`   6.2.0是你安装的版本号。(适用于类unix系统)

+ 因为需要查看自己的next主题版本号是否支持一些新的特性.
    **查看方法**进入主题的配置目录，找到`_config.yml`配置文件然后打开在末尾即可看到。

