---
title: Manjaro使用deepin-wine-wechat微信无法输入中文
author: xfans
type: post
date: 2019-08-13T03:29:00+00:00
url: /archives/264
categories:
  - Linux
tags:
  - Manjaro
  - wechat
  - wine

---
## 说明

把manjaro滚到最新版本后deepin-wine-wechat微信无法输入中文

## 解决方法

deepin-wine-wechat的github上给出了兼容列表<figure class="wp-block-table">

<table class="">
  <tr>
    <th>
      微信版本
    </th>
    
    <th>
      wine版本
    </th>
    
    <th>
      兼容性
    </th>
    
    <th>
      备注
    </th>
    
    <th>
      deepin-wine版本
    </th>
    
    <th>
      兼容性
    </th>
    
    <th>
      备注
    </th>
  </tr>
  
  <tr>
    <td>
      2.7.1.88
    </td>
    
    <td>
      4.18-1
    </td>
    
    <td>
      部分
    </td>
    
    <td>
      不能使用中文输入法
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      2.7.1.85
    </td>
    
    <td>
      4.18-1
    </td>
    
    <td>
      部分
    </td>
    
    <td>
      不能使用中文输入法
    </td>
    
    <td>
      2.18_18-2
    </td>
    
    <td>
      ?
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      2.7.1.82
    </td>
    
    <td>
      4.18-1
    </td>
    
    <td>
      部分
    </td>
    
    <td>
      不能使用中文输入法
    </td>
    
    <td>
      2.18_18-2
    </td>
    
    <td>
      不支持
    </td>
    
    <td>
      闪退
    </td>
  </tr>
  
  <tr>
    <td>
      2.7.1.82
    </td>
    
    <td>
      4.17-1
    </td>
    
    <td>
      部分
    </td>
    
    <td>
      不能使用中文输入法
    </td>
    
    <td>
      2.18_18-2
    </td>
    
    <td>
      不支持
    </td>
    
    <td>
      闪退
    </td>
  </tr>
  
  <tr>
    <td>
      2.6.8.65
    </td>
    
    <td>
      4.16-1
    </td>
    
    <td>
      支持
    </td>
    
    <td>
    </td>
    
    <td>
      2.18_18-2
    </td>
    
    <td>
      支持
    </td>
    
    <td>
    </td>
  </tr>
</table></figure> 

根据表格,使用4.16版本的wine即可.

### 降级方法

安装downgrade

`sudo pacman -S downgrade`

查看wine历史版本

`DOWNGRADE_FROM_ALA=1 downgrade wine`<figure class="wp-block-image size-large">

<img loading="lazy" width="407" height="431" src="https://xfans.me/wp-content/uploads/2019/12/wine.png" alt="" class="wp-image-265" srcset="https://xfans.me/wp-content/uploads/2019/12/wine.png 407w, https://xfans.me/wp-content/uploads/2019/12/wine-283x300.png 283w" sizes="(max-width: 407px) 100vw, 407px" /> </figure> 

选择4.16及以下版本即可,我选择本地存在的4.15及输入22等待安装完成.

安装完成后出现

`添加wine到IgnorePkg? [y/n]`

输入y将wine添加到IgnorePkg,防止后续被更新.

## 链接

<a href="https://github.com/countstarlight/deepin-wine-wechat-arch#%E5%85%BC%E5%AE%B9%E6%80%A7%E8%AE%B0%E5%BD%95" target="_blank" rel="noreferrer noopener" aria-label="https://github.com/countstarlight/deepin-wine-wechat-arch#%E5%85%BC%E5%AE%B9%E6%80%A7%E8%AE%B0%E5%BD%95（在新窗口打开）">https://github.com/countstarlight/deepin-wine-wechat-arch#%E5%85%BC%E5%AE%B9%E6%80%A7%E8%AE%B0%E5%BD%95</a>