---
title: FFmpeg剪切连接音视频
date: 2018-05-26
categories: FFmpeg
tags:
- concat video
---

这几天需要处理一下视频，开始以为就是简单的剪切和连接，没想到耗费了很多时间，现在把使用过的方法总结出来，以后就不用重复寻找。

音频
====

平时用音频操作基本就是剪切制作手机铃声，所以就只记录剪切的方法

`ffmpeg -i input.mp3 -ss 00:01:12 -t 00:01:42 -acodec copy output.mp3`

<!--more-->

视频
====

剪切
----

`ffmpeg -i input.mp4 -ss STARTTIME -t STOPTIME -acodec copy -vcodec copy output.mp4`

**STARTTIME/STOPTIME** 格式可以有两种：

1.  以秒为单位，直接写数字即可
2.  时:分:秒 01:02:03

如果需要对视频编辑，建议使用软件，这里推荐[Shotcut](https://www.shotcut.org/) ，或者从[这里](https://zhuanlan.zhihu.com/p/21879714)找找适合的。

连接
----

连接比较复杂，这里分情况介绍。

1.  视频是由同一个设备录制，其实就是视频的码率，声音，宽高等要素全部一样，这样连接比较简单
    -   文件方法 [原文](https://segmentfault.com/a/1190000000414341)

    ``` bash
    # 基本就是把想要连接的视频的路径写在一个文本文件中，然后用命令连接，适合连接文件较多的情况
    $ cat mylist.txt
    file '/path/to/file1'
    file '/path/to/file2'
    file '/path/to/file3'
    $ ffmpeg -f concat -i mylist.txt -c copy output
    ```

    -   直接命令方法

    ``` bash
    # 连接文件较少比较合适
    ffmpeg -i "concat:input1.mp4|input2.mp4|input3.mp4" -c copy output.mp4
    ```

2.  视频虽然格式相同，如都是MP4，但是码率，宽高都不相同，这就比较麻烦，还没有好的解决方法，以下仅供参考 **如果大家有解决方案，请麻烦告诉一下**

    在网络上搜索的这种情况，基本思路就是把视频转换到临时格式，合并后再转换回源格式

    -   使用ts格式过渡 [原文](https://superuser.com/a/1059261)

    ``` bash
    ffmpeg -i input1.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate1.ts
    ffmpeg -i input2.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate2.ts
    ffmpeg -i "concat:intermediate1.ts|intermediate2.ts" -c copy -bsf:a aac_adtstoasc output.mp4
    ```

    *这种方法可以完成视频的连接，画面没有问题，但是视频的声音会丢失*

    -   使用mpg格式过渡 [原文](https://stackoverflow.com/a/7333453)

    ``` bash
    ffmpeg -i 1.mp4 -qscale 0 1.mpg
    ffmpeg -i 2.mp4 -qscale 0 2.mpg
    cat 1.mpg 2.mpg | ffmpeg -f mpeg -i - -qscale 0 -vcodec mpeg4 output.mp4
    ```

    *视频可以连接，声音还在，不过视频会压缩为一样的宽高*

    -   最后是我已经实验过的几种转换方法和连接方法，不过都失败了

    `ffmpeg -i 2.mp4 -f lavfi -i anullsrc -c:v copy -c:a aac -shortest 2.ts`
    [原文](https://superuser.com/a/1029552) 这个命令转换的`ts`文件连接后部分视频会有声音，不过部分会丢失，我查看了转换后的 `ts`文件，发现有的转换为 `ts` 文件后声音就没有了。

    `cat 1.ts 2.ts >all.ts ffmpeg -i all.ts -c copy all.mp4`
    [原文](https://video.stackexchange.com/a/20074)另一种连接方法，比较简单，能够连接不过效果和之前的方式相同，没有声音

    `ffmpeg -i infilename -f mpegts -vcodec copy -acodec copy outfile.ts`
    [原文](https://superuser.com/q/227036) 这种方式转换后的 `ts` 文件有声音，不过连接后还是不行。
