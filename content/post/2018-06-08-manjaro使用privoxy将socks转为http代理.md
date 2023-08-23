---
title: Manjaro使用privoxy将socks转为http代理
author: xfans
type: post
date: 2018-06-08T08:57:27+00:00
url: /archives/163
views:
  - 1295
categories:
  - Linux
tags:
  - Manjaro

---
## 安装

`pacman -S privoxy`

## 配置

修改/etc/privoxy/config

添加：

`forward-socks5   /               127.0.0.1:1080 .` #ss的代理

`listen-address localhost:8118`

## 重启

`systemctl restart privoxy`

## PS 终端中使用代理

`export http_proxy=http://127.0.0.1:8118`

`export https_proxy=http://127.0.0.1:8118`