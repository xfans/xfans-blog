---
title: "Flutter Page curl effect like epub reader"
description: ""
date: "2023-10-10T17:50:12+08:00"
thumbnail: ""
summary: "If you want to implement a page-turning effect for a reader in Flutter, you typically overlay a view that performs the page-turn animation on a container. When a left or right swipe gesture is detected, the view with the page-turn animation is displayed. After the page-turn animation ends, the content of the underlying container is changed to achieve the page-turn effect."
categories:
  - "flutter"
tags:
  - ""
---
## Overview

Flutter Page curl effect like epub reader

If you want to implement a page-turning effect for a reader in Flutter, you typically overlay a view that performs the page-turn animation on a container. When a left or right swipe gesture is detected, the view with the page-turn animation is displayed. After the page-turn animation ends, the content of the underlying container is changed to achieve the page-turn effect.


## Preview
{{< video "https://apixfans.oss-cn-zhangjiakou.aliyuncs.com/2024/11/29/flutter_page_curl_1.mp4" >}}



## How To
1.First, you need to write a page-turn animation or find an existing algorithm that implements it. To facilitate implementation, I found a Flutter effect: https://github.com/Rahiche/riveo_page_curl. This library is based on Flutter's shader implementation, which offers better performance. You can refer to the ProjectCard class in this library for implementation. The core code is as follows:
    
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

backImage is used to draw the background image for the page turn, while curlImage is used to draw the image of the current page during the page turn.


