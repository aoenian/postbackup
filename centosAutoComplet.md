---
title: CentOS命令行自动补全
date: 2016-10-16 19:15:31
categories: Linux
tags: 
- centos
- autocomplet 
---

安装CentOS以后会发现，命令行没有自动补全，以前超好用的Tab键居然不管用了，这就不好玩了。网络上很多方法就是直接安装 `bash-completion` 可是你执行安装命令以后如果不出意外，结果就是安装不上，提示没有这个安装包。

靠谱的解决方法如下：

<!-- more -->

+ 进入[EPEL的wiki页面](https://fedoraproject.org/wiki/EPEL)

+ 找到[How can I use these extra packages?](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F)

+ 找到相对应的版本这里以CentOS7为例，下载[The newest version of 'epel-release' for EL7](https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm)

+ 安装上一步下载的软件包 `rpm -ivh rpm_name` ,然后执行 `yum install bash-completion` 记得要root。等待完成即可。

完成，试试 `Tab` 键的魔法吧！
