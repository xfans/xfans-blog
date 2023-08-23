---
title: Linux Manjaro安装kernel及Kernel Headers
author: xfans
type: post
date: 2017-09-29T11:43:55+00:00
url: /archives/16
views:
  - 1535
categories:
  - Linux
tags:
  - linux

---
## 查找可用内核

`pacman -Ss linux-headers`<figure class="wp-block-image size-large">

<img loading="lazy" width="654" height="473" src="https://xfans.me/wp-content/uploads/2019/12/headers.png" alt="" class="wp-image-225" srcset="https://xfans.me/wp-content/uploads/2019/12/headers.png 654w, https://xfans.me/wp-content/uploads/2019/12/headers-300x217.png 300w" sizes="(max-width: 654px) 100vw, 654px" /> </figure> 

## 安装

`pacman -S linuxXY linuxXY-headers`

注意:XY为你要安装的版本号,如:

`pacman -S linux49 linux49-headers`