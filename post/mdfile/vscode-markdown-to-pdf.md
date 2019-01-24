---
title: vscode编辑markdown文件并导出为PDF格式
date: 2018-12-24
categories: Editor
tags:
- vscode
- markdown
- pdf
---

之前一直用vscode编辑md文件，然后在chrome预览导出pdf文件。有段时间没用了突然发现chrome无法预览md文件了。把md文件拖入浏览器就会直接下载，而不能出现预览。

更有意思的是我写了三个md文件，其中有一个可以预览，另外两个不管是改名字换目录都不行，拖入浏览器就是直接把原文件又下载一遍，网络搜索了一下几乎没人遇到，可能运气不好。不过搜索过程中了解到vscode可以直接导出，索性就不再用chrome的插件处理md文件了，直接用vscode。

<!-- more -->

# markdown-pdf 插件的使用

1. 安装`Chrome`或者`Chromium`，如果已安装可跳过此步骤。

2. 在vscode中安装插件`Markdown PDF`，重启vscode后那个插件会自动下载`Chromium`，右下角会有个提示，不过放心它是不会成功的。

3. 如果没有任何设置就直接在md文件右键执行`Markdown PDF: Export(pdf)`，十有八九会出现`TypeError: Cannot read property 'newPage' of undefined`，`ERROR: puppeteer.launch()`，`Error: spawn EACCES`这些错误，如果没有提示且pdf正常导出则不用继续往下看了，直接用就可以了。

4. 如果出现了第三步的错误提示，打开vscode的**首选项**-**设置**，然后在搜索框中搜索`markdown-pdf.executablePath`，点击右边的**在 settings.json 中编辑**。

    在用户配置文件中写入如下配置：
    ```js
    // Mac下面Chrome的位置
    "markdown-pdf.executablePath": "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
    // Windows 请根据自己系统情况配置
    "markdown-pdf.executablePath": "C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe"
    ```

5. 保存并重启vscode。试试吧！！

# 如何使用自定义css

在mac下面导出的pdf文件个人感觉还是很不错的，不过在windows下面导出后汉字的字体大小不一，而且颜色也有深有浅，这样就需要我们使用自定义css。

同样在**setting.json**里面加入如下配置

```js
// 路径和css文件名以你的文件为准
"markdown-pdf.styles": ["E:\\css\\markdowncss.css"],
```

css文件可以去github上面或者网络找一下，选一个自己喜欢的。

PS：这里推荐一个md文件语法检查插件`markdownlint`，还有就是打开vscode的**查看**-**切换控制字符**可以看到一些误输入的奇怪的符号。

参考文章：

- [can not work with lastest VS Code version](https://github.com/yzane/vscode-markdown-pdf/issues/77)

- [markdowncss_github_style_blue_by_jwsky](https://github.com/jwsky/markdowncss)
