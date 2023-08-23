---
title: RecyclerView包含TextView并打开TextIsSelectabl后,点击TextView自动滚动
author: xfans
type: post
date: -001-11-30T00:00:00+00:00
draft: true
url: /?p=296
categories:
  - 未分类

---
## 现象

RecyclerView的item中包含TextView时,当TextView打开TextIsSelectabl功能,并且TextView的内容超过一屏后,点击TextView后,RecyclerView会自动滚动到TextView底部.

## 原因

当调用TextView的setTextIsSelectable时,