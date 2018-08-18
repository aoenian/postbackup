---
title: Next主题样式配置1-行内代码、下划线、字体
date: 2018-08-06
categories: Hexo
tags:
- next
- style
---
使用Github Pages做博客以后，一直用的主题就是Next，中间换过一次，但是太折腾了，后来又换回来了。

不过在使用过程中修改了一些配置和样式，让有些地方更适合自己，这里把配置记录一下，以便以后参考。

<!--more-->

这篇博文没有介绍特效的增加，一个是很多博客已经有这方面的介绍，二是个人觉得博客还是内容为主，主要原因还是懒。

样式的主要处理目的是使文章更加易读，其他的这个主题已经做的很好了。

行内代码背景颜色加深
====================

有一次我在给别人看我的博文的时候发现的，由于行内代码的背景色太浅，所以他没有注意到需要强调的特殊代码，然后我Google了一下，修改了行内代码的背景色。

**这篇博文修改的样式文件位置: `next-source-css-_variables-base.styl`**

帮你挑选颜色的网站： [color-hex](http://www.color-hex.com/)

找到 `Code & Code Blocks` 下面的 `$code-background` 对应的参数，不过由于 `$gainsboro` 是变量，所以你要到文件的开始找到这个变量的位置。

| 变量       | 原值  | 更改后 |
|------------|-------|--------|
| $gainsboro | \#eee | \#bbb  |

加深页面内链接下划线的颜色
==========================

和上面基本同样的操作，不过更改的变量换了。需要更改的是 `$link-decoration-color` 的颜色。

| 变量        | 原值  | 更改后   |
|-------------|-------|----------|
| $grey-light | \#ccc | \#4d4d80 |

这个颜色在你的博客页码的背景色也用到。可以根据自己的情况修改。

字体的修改
==========

由于感觉代码块的字体有点宽，所以就像修改紧凑一点，不过这个不需要修改 `base.styl` 文件，而是修改主题的 `_config.yml` 文件。

找到 `font` 选项，修改如下代码：

``` yaml
font:
  enable: true
  host: /fonts

  logo:
    external: true
    family: Georgia

  codes:
    external: true
    family: Inconsolata for Powerline
    size: 16

```

*并不是所有字体都能够起作用，建议选用网络安全字体。* 选用的字体文件请放在 `next-source-fonts` 中。
