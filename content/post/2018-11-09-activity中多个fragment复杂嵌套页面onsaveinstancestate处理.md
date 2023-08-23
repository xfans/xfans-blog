---
title: Activity中多个Fragment复杂嵌套页面onSaveInstanceState处理
author: xfans
type: post
date: 2018-11-09T10:39:18+00:00
url: /archives/171
views:
  - 288
categories:
  - Android
tags:
  - onSaveInstanceState

---
按照最简单的原则即可:Activity处理自己的SaveInstanceState,Fragment处理自己的SaveInstanceState,不要调用Activity与Fragment之间数据传递的方法.