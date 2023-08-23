---
title: launcher 使用windowIsTranslucent引发的bug
author: xfans
type: post
date: 2021-09-07T09:01:19+00:00
url: /archives/382
categories:
  - Android

---
## 说明

把launcher的theme添加  
`<item name="android:windowIsTranslucent">true</item>`  
会引起生命周期的异常，引发home键无效等bug。

## 解决方法

使用  
`<item name="android:windowBackground">@android:color/transparent</item>`  
`<item name="android:windowShowWallpaper">true</item>`  
代替