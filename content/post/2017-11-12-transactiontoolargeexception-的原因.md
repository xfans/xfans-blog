---
title: TransactionTooLargeException 的原因
author: xfans
type: post
date: 2017-11-12T13:43:02+00:00
url: /archives/53
views:
  - 525
bigfa_ding:
  - 1
categories:
  - Android

---
## 问题

适配7.0时发生了TransactionTooLargeException的异常. 

## 原因

1.Fragment关闭时会调用onSaveInstanceState,而在onSaveInstanceState中保存了比较大的数据.

2.调用fragment.setArguments(bundle)时，往bundle中放了过大的数据，FragmentManager管理Fragment时saveState报错.

## 解决方法  


由于保存的数据较多,无法确定那个数据比较大,最后使用这个库_toolargetool_定位到数据较大的对象.

将_toolargetool_引入到项目

<pre class="wp-block-preformatted"><em>compile 'com.gu.android:toolargetool:0.1.3@aar</em></pre>

在Application中初始

<pre class="wp-block-preformatted"><em>TooLargeTool.startLogging(this)</em></pre>

这个库会在每次关闭Activity,Fragment时,打印出onSaveInstanceState的对象及大小.如图<figure class="wp-block-image">

<img loading="lazy" width="855" height="90" src="https://xfans.me/wp-content/uploads/2019/11/toolargetool.png" alt="" class="wp-image-203" srcset="https://xfans.me/wp-content/uploads/2019/11/toolargetool.png 855w, https://xfans.me/wp-content/uploads/2019/11/toolargetool-300x32.png 300w, https://xfans.me/wp-content/uploads/2019/11/toolargetool-768x81.png 768w" sizes="(max-width: 855px) 100vw, 855px" /> </figure> 

查找log中较大的对象,进行相应的处理即可.比如保存到数据库,缩小对象等  


## 相关链接

<https://github.com/guardian/toolargetool>