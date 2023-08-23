---
title: 使用ffmpeg将MP4高质量的转换成GIF
author: xfans
type: post
date: 2018-07-14T09:20:00+00:00
url: /archives/278
categories:
  - Linux
tags:
  - ffmpeg

---
## 方法

`ffmpeg -v warning -ss 00:00:02 -t 2 -i 5.mp4 -vf scale=300:-1 -gifflags +transdiff -y 5.gif`

-ss 截取开始时间

-t 截取时长

5.mp4 是视频名称

5.gif 是转换后的名称

## 相关链接

<http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html>