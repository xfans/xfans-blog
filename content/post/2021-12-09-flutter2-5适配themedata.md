---
title: Flutter2.5适配ThemeData
author: xfans
type: post
date: 2021-12-09T02:29:49+00:00
excerpt: Flutter2.5
url: /archives/392
categories:
  - Flutter
tags:
  - flutter

---
## 说明

Flutter 2.5 版本中的破坏性改动

  * 默认的设备拖动和滚动
  * v2.2 版后删除了废弃的 API
  * Package 介绍: flutter_lints
  * ThemeData 的 accent 属性已被弃用
  * 手势识别器清理
  * 将 AnimationSheetBuilder.display 替换为 collate
  * 使用 HTML 插槽在 Web 中渲染平台视图
  * 将 LogicalKeySet 迁移至 SingleActivator

项目使用Flutter2.5编译后，部分页面的颜色发生了变化。  
需要对ThemeData 的 accent 属性进行适配。

#### 改动

主题信息 ThemeData 中的 accentColor、accentColorBrightness、accentIconTheme 和 accentTextTheme 属性现已被弃用。

Material 设计规范 不再为 Material 组件指定或使用「强调」色。组件的默认颜色来自整个主题的 颜色方案。现在通常使用 ColorScheme 的 secondary color 代替 accentColor，并且在需要对比色时使用 onSecondary 属性。

## 适配

适配前：

<pre><code class="language-dart">theme: ThemeData(
    primaryColor: Color.fromRGBO(156, 184, 133, 1.0),
    accentColor: Color.fromRGBO(91, 109, 77, 1.0),),</code></pre>

适配后：

<pre><code class="language-dart">ColorScheme colorSchemeLight = ColorScheme.light(
    primary: primaryColor,
    secondary: accentColor,
    primaryVariant: primaryColor,
    secondaryVariant: accentColor,
);

theme: ThemeData(
    brightness: Brightness.light,
    primaryColor: Color.fromRGBO(156, 184, 133, 1.0),
    colorScheme: colorSchemeLight,),</code></pre>

使用时：

<pre><code class="language-dart">Theme.of(context).accentColor;</code></pre>

改为：

<pre><code class="language-dart">Theme.of(context).colorScheme.secondary;</code></pre>

## 相关链接

<https://flutter.cn/docs/release/breaking-changes/theme-data-accent-properties>

<https://flutter.cn/posts/whats-new-in-flutter-2-5>