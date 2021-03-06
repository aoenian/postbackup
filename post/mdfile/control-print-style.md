---
title: 控制打印的css样式
date: 2018-09-25
categories: Net
tags:
- front
- web
---

前段时间遇到一个问题，有很多图片然后需要打印出来，但是呢图片太大放在word里面以后发现只有一半在打印区域，剩下的都出去了。

首先想到的解决方法就是找一下如何在word里面批量修改图片大小，你别说还真有，但是步骤很复杂，当然复杂不怕，关键是起作用就行。不过里面有个操作很可怕，里面需要图片的插入方式是 **四周型**，我一看我的这个word文件，里面的图片都是嵌入式，一张张改类型还不如我直接一张张改大小得了。对了， _word里面的格式刷好像对图片也没用。_

<!--more-->

然后突然想到，只是为了打印出来，可以使用html和css控制样式，然后用浏览器输出为pdf。这里面遇到了问题，在页面中的样式调整完成，但是一旦用浏览器打印pdf文件，样式就全没了。后来搜索后才知道需要引入控制打印的样式。

具体操作如下：

```html
<link rel="stylesheet" href="print.css" media="print" />
```

使用 `media` 属性来标明样式为打印服务。

其他的方法和注意事项，大家可以参考下面的博客。

- [css控制print打印样式](https://blog.csdn.net/pangni/article/details/6224533)
