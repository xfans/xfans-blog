---
title: 一个bad request http request or invalid response的错误
author: xfans
type: post
date: 2018-01-19T11:56:59+00:00
url: /archives/80
views:
  - 392
categories:
  - Android
tags:
  - Charles
  - Wireshark

---
## 问题

部分客户端api报错，特别是8.0系统上，访问api出现`bad request http request or invalid response,from ulb`错误。

## 解决步骤

### 1.服务器联调

服务器端未发现问题，参数都对。服务器上也没有这句错误信息。猜测应该是服务器上层报出来的错误，如负载均衡。

### 2.用Charles抓包

Charles抓包后，就不会出现该错误了。后来发现Charles对错误报文进行了兼容处理，发给服务器的应该对的。

### 3.去GIthub查网络库的issues

项目中使用的是,Android-async-http这个库,issues中没有人说8.0中有这个问题，猜想应该是自己代码中的问题。

### 4.大鲨鱼Wireshark抓包

大鲨鱼Wireshark抓包后会出现该错误，没有看出报文有异常。

比较正确的报文和错误的报文，发现大小不一样。

最终发现（找了好久，反正就是很艰难），错误的报文Header中多了一个空行。<figure class="wp-block-image size-large">

![][1] </figure> 

## 原因

在往Header中添加参数时，有一个参数出现了`\n`,正常情况下客户端发送报文的时会忽略`\n`，而有的版本把`\n`放在了报文中，导致报文格式出错。

 [1]: http://img.xfans.me/xfans/image/wireshark_http.png