---
title: Manjaro使用bbswitch关闭笔记本的nvidia独立显卡
author: xfans
type: post
date: 2017-09-29T11:33:43+00:00
url: /archives/6
views:
  - 1943
bigfa_ding:
  - 1
categories:
  - Linux
tags:
  - linux

---
Manjaro关闭独立显卡

### 安装bbswitch

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">pacman -S bbswitch
</code></pre>

### 在/etc/modules-load.d下新建bbswitch.conf，并修改为如下内容。这样每次启动都会加载bbswitch模块了

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">bbswitch
</code></pre>

### 在/etc/modprobe.d/下新建bbswitch.conf，并修改为如下内容。这是bbswitch加载的参数，我们让其默认关闭

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">options bbswitch load_state=0
</code></pre>

### 在/etc/modprobe.d/下新建nouveau_blacklist.conf，并修改为如下内容。有时候bbswitch加载了，但是不能关闭显卡，因为有些模块正在占用着，因此要禁掉。

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">blacklist nouveau  
blacklist nvidiafb
</code></pre>

### 在/usr/lib/systemd/system-shutdown/下新建nvidia\_card\_enable.sh，并修改为以下内容，记得加运行权限。每次reboot的时候，显卡都是关闭的，不管是重启到windows还是linux，都会找不到设备，必须彻底关机才行。解决该问题的办法就是每次重启都启用显卡

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">#!/bin/bash  
case "$1" in  
    reboot)  
        echo "Enabling NVIDIA GPU"  
        echo ON &gt; /proc/acpi/bbswitch  
    ;;  
    *)  
esac
</code></pre>

### 查看状态

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">sudo cat /proc/acpi/bbswitch
</code></pre>

### 原文

http://blog.csdn.net/gt945/article/details/10922263