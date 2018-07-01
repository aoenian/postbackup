---
title: 如何避免Emacs破坏文件的硬链接
date:  2017-04-19
categories: Editor
tags:
- Emacs
- hard link
---


使用了markdown以后感觉不错，能够全心放在写作内容上面，不过用了一段时间以后开始发愁编辑器的问题，在网络搜索的过程中进入了org-mode的坑，配上emacs，用着很爽，也不会发愁注册啦，版权之类的问题。所以就开始使用org-mode进行文字记录。

问题的出现是在写博客的过程中，在blog目录下写完一篇文章后我会把文件硬链接到post目录下，这样呢如果文章预览解析有问题需要更改，那么留在blog目录下的文章也会随之改变，保证两边的文章是一样的。

<!--more-->

原来用vim的时候一直样做，改成emacs也没多想，但是发现问题了，建立文件硬链接以后都正常，但是用emacs打开其中一个文件更改保存后，发现两个文件的
`inode` 不同了，感觉就像emacs自动创建了一个副本。

解决的方法就是在你的配置文件里面加入如下配置：

`(setq backup-by-copying t)`

如果你只想在链接的时候用这个配置，请使用下面的代码：

`(setq backup-by-copying-when-linked t)`

具体的详细介绍可查看 [How to prevent Emacs from breaking hard links?](https://emacs.stackexchange.com/questions/4237/how-to-prevent-emacs-from-breaking-hard-links/4240#4240)
