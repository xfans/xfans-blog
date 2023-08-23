---
title: Manjaro Linux安装步骤
author: xfans
type: post
date: 2018-01-26T12:11:37+00:00
url: /archives/91
views:
  - 534
bigfa_ding:
  - 1
categories:
  - Linux
tags:
  - Manjaro

---
## 介绍

### 优点：

1.基于Arch，但不用折腾

2.滚动更新，但比Arch慢，比Arch更稳定

3.支持AUR仓库，软件非常多，比如deepin qq，sogou输入法等，一行命令就可以安装

4.Wiki非常丰富

### 缺点：

1.滚动更新，容易滚挂

## 安装步骤

### 下载

https://mirrors.tuna.tsinghua.edu.cn/manjaro-cd/

### usb安装

`df -h 查看你的u盘`

`sudo umount /dev/sdb 卸载U盘`

`dd if=xxx.iso of=/dev/sdb bs=1M 制作启动盘`

note: /dev/sdb为u盘

### 添加国内源

##### 编辑

`vi /etc/pacman-mirrors.conf`

##### 修改

`OnlyCountry=China`

##### 根据速度排序

`pacman-mirrors -g`

### 添加Arch源

##### 编辑

`vi /etc/pacman.conf`

##### 在末尾添加

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null"> [archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
</code></pre>

##### 同步

`sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring`

### 安装zsh

##### 执行

`sudo pacman -S zsh`

`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

`chsh -s /bin/zsh`

### 安装输入法

`sudo pacman -S fcitx-im # 全部安装`

`sudo pacman -S fcitx-configtool # 图形化配置工具`

##### 编辑

`vi ~/.xprofile`

##### 在末尾添加

<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
</code></pre>

### 安装chrome

`pacman -S google-chrome`

### 滚系系统

`sudo pacman -Syyu`

### 其他一些软件

`sudo pacman -S visual-studio-code-bin`

`sudo pacman -S filezilla`

`yaourt -S deepin-wine-tim`

`sudo pacman -S fcitx-sougoupinyin`

`sudo pacman -S proxychains-ng`

`XnView MP`

## 官网

https://www.manjaro.org/