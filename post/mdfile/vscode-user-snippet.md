---
title:  VSCode建立用户代码片段
date: 2018-08-13
categories: Editor
tags:
- vscode
- snippet
---

编辑器尝试过很多，感觉没有特别适合的，当然我并不是深度用户，所以不做过多的评论。遇到VSCode以后感觉还是蛮不错，从一个普通用户的角度来说。

现在主要用的编辑器是两个 `Emacs` 和 `VSCode` ，前者用来写org文件，后者编辑修改一些小程序和配置文件。

遇到这个问题是因为在编写简单的 `HTML` 文件时使用VSCode自带的补全有些内容暂时不需要，所以想自己写一个类似 `emmet` 官方的自动补全，试了一下操作很方便，这里做个记录备份。

<!--more-->

在code中如果编辑html文件，直接输入 `html:5`
则可以自动补全页面的基本标签，具体如下：

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

不过里面的 `lang` 默认是 `en` ，而且做个简单的页面也不需要那么多 `meta` 数据，当然放在那里影响也不大，看个人需要。

既然不用软件自带的，那么咱们就创建一个自己用的代码片段。找到 `文件-首选项-用户代码片段` 在弹出的输入框中输入 `html` 回车，会打开一个 `html.json` 的文件，在这里面编辑就可以了。

**注意：代码需要在两个大括号之间，不能超出。** 下面是我自己的代码片段，供大家参考。

``` json
"Simple html5 head": {
    "prefix": "myhtml",   
    "body": [             
        "<!DOCTYPE html>",
        "<html lang=\"${1:en}\">",
        "<head>",
        "    <meta charset=\"UTF-8\" />",
        "    <title>${2:Document}</title>",
        "</head>",
        "<body>",
        "    $3",
        "</body>",
        "</html>",
    ]
}
```

具体的解释文档在相应语言的 `json` 文件中，这里简单做个介绍。

> 第一行是你的这个代码片段的描述。
> 第二行很重要，是你的这个代码片段的激活口令。
> 第三行一直到最后body结束，是你需要插入的代码。
> 其中如果你插入的代码不需要换行，或者你使用换行符 `\n` 换行，则不需要 `[]` 符号把内容括起来。直接一个双引号把内容包住即可，当然内容里面的双引号需要转义。
> `$num` 则为 `tab` 键跳转位置， `${num:string}` 则是默认用 `string` 占位。
