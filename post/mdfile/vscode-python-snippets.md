---
title: VSCode增加Python语言用户代码片段
date: 2019-10-14 20:28:43
categories: Editor
tags:
- snippets
- Variables
---

这段时间练习Python编程，编写的时候每次都要写文件头有点麻烦，所以就编辑了一个自己使用的代码片段。方便插入文件编码、时间、作者之类的程序信息。

<!--more-->

在编写的过程中发现需要用到vscode的变量，经过查找官方文档完成了自己的代码。具体代码如下：

<!--more-->

```javascript
"Print file header": {
		"prefix": "header",
		"body": [
			"# -*- coding: utf-8 -*-",
			"$BLOCK_COMMENT_START",
			"@Author: aoenian",
			"@Filename: $TM_FILENAME",
			"@Date: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND $CURRENT_DAY_NAME",
			"$BLOCK_COMMENT_END",
		]
	},

	"Insert modify time": {
		"prefix": "modi",
		"body": "@Last Modified time: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND $CURRENT_DAY_NAME",
    }
```

第一个 header 是插入常用的一些文件信息，作者创建日期，文件名之类。后面的 modi 则是手动插入更新的日期。

这里面还遇到一个问题是，插入文件更改日期的时候由于是插入到Python的块注释中，所以软件认为是普通文本，不会自动触发代码片段。这个时候需要利用手动触发来执行，由于那个默认的快捷键与系统冲突，所以需要更改。

更改方法：点击-Code-首选项-键盘快捷方式-在搜索框中输入 **触发建议** - 更改为 ALT+/ （可以根据自己的需要进行更改）

具体的配置代码如下：

```javascript
{
  "key": "alt+/",
  "command": "editor.action.triggerSuggest",
  "when": "editorHasCompletionItemProvider && textInputFocus && !editorReadonly"
}
```

参考文章：

- [Snippets in Visual Studio Code](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_variables)
- [Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)
