---
title: Linux下安装网络环境模拟工具augmented-traffic-control
author: xfans
type: post
date: 2017-11-23T12:10:48+00:00
url: /archives/56
views:
  - 861
categories:
  - Linux
tags:
  - android
  - linux
  - 网络环境

---
## augmented-traffic-control

A tool to simulate network conditions,Facebook提供的一款网络环境模拟工具，可以使用他来测试App在各种网络环境下的问题。

## 安装环境要求

Python2.7、Django1.10.+  
以下在Manjora下的安装命令

### [切换Python环境到Python2.7][1]

### 安装pip

`sudo pacman -S python2-pip`

### 安装Django1.10.+

`sudo pip install Django==1.10.8`

## 安装ATC

### 安装lib

`sudo pacman -S pip install atc_thrift atcd django-atc-api django-atc-demo-ui django-atc-profile-storage`

### Django配置

`django-admin startproject atcui`

`cd atcui`

#### 修改atcui/settings.py中的INSTALLED_APPS

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-bash">    INSTALLED_APPS = (
        ...
        # Django ATC API
        'rest_framework',
        'atc_api',
        # Django ATC Demo UI
        'bootstrap_themes',
        'django_static_jquery',
        'atc_demo_ui',
        # Django ATC Profile Storage
        'atc_profile_storage',
    )
</code></pre>

#### 修改atcui/urls.py中的urlpatterns及引用

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-bash">    ...
    ...
    from django.views.generic.base import RedirectView
    from django.conf.urls import include

    urlpatterns = [
        ...
        # Django ATC API
        url(r'^api/v1/', include('atc_api.urls')),
        # Django ATC Demo UI
        url(r'^atc_demo_ui/', include('atc_demo_ui.urls')),
        # Django ATC profile storage
        url(r'^api/v1/profiles/', include('atc_profile_storage.urls')),
        url(r'^$', RedirectView.as_view(url='/atc_demo_ui/', permanent=False)),
    ]
</code></pre>

#### 更新数据库

`python manage.py migrate`

### 创建虚拟Ap

[Linux开启虚拟AP][2]

### 启动ATC

`sudo atcd --atcd-wan wlp3s0 --atcd-lan ap0`

注意：wlp3s0是我的无线网卡，ap0是我创建的虚拟ap网卡

设置错误可能出现：https://github.com/facebook/augmented-traffic-control/issues/239

### 启动ATCUI

`python manage.py runserver 0.0.0.0:8000`

### 导入默认配置

#### 下载该路径下的文件https://github.com/facebook/augmented-traffic-control/tree/master/utils

#### 运行导入命令

`./restore-profiles.sh localhost:8000`

### 手机使用ATC

使用Android、iOS 的浏览器访问

ip:8000 ip为你电脑的ip

如图：  
![][3] 

点击Profiles中相应的Select选择不同环境，然后点击上边的Update Shaping更新设置。

### 相关链接

https://github.com/facebook/augmented-traffic-control

 [1]: http://xfans.me/archives/12 "切换Python环境到Python2.7"
 [2]: http://xfans.me/archives/42 "Linux开启虚拟AP"
 [3]: http://xfans.me/wp-content/uploads/2017/11/Screenshot_20171123-200456-169x300.png