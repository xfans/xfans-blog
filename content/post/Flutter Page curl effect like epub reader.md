---
title: "Flutter Page curl effect like epub reader"
description: ""
date: "2023-10-10T17:50:12+08:00"
thumbnail: ""
summary: "如果要在Flutter中实现阅读器的翻书效果，一般都是在一个容器上边覆盖一个实现翻页动画的view，当监听到左右滑动的手势的时候显示出来实现翻页动画的view，翻页动画结束后改变底层容器的内容，来实现翻页。"
categories:
  - "flutter"
tags:
  - ""
---
## Overview

Flutter Page curl effect like epub reader

如果要在Flutter中实现阅读器的翻书效果，一般都是在一个容器上边覆盖一个实现翻页动画的view，当监听到左右滑动的手势的时候显示出来实现翻页动画的view，翻页动画结束后改变底层容器的内容，来实现翻页。


## Preview
{{< video "https://apixfans.oss-cn-zhangjiakou.aliyuncs.com/2024/11/29/flutter_page_curl_1.mp4" >}}



## How To
1.首先要写一个翻页的动画或者找一个已经实现了的算法。
    为了方便实现，找到了一个Flutter实现的效果：https://github.com/Rahiche/riveo_page_curl。这个库是基于Flutter的shader实现的，性能更好。实现可以参考这个库的ProjectCard类。
    核心代码如下：
    
    ShaderBuilder(
      (context, shader, _) {
        return AnimatedSampler(
          (image, size, canvas) async {
            // print("AnimatedSampler");
            if (backImage != null) {
              ShaderHelper.drawShaderRectBack(backImage!, size, canvas);
            }
            if (curlImage != null) {
              ShaderHelper.configureShader(
                  shader, size, curlImage!, _animation.value);
              ShaderHelper.drawShaderRect(shader, size, canvas);
            }
          },
          enabled: true,
          child: Container(
            width: double.infinity,
            height: double.infinity,
          ),
        );
      },
      assetKey: shaderAssetKey,
    )

其中drawShaderRectBack是画翻页时候的图片，curlImage是画翻页翻页当前页的图片


