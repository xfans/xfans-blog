---
title: 使用SVN下载GitHub单个目录内的文件
author: xfans
type: post
date: 2018-01-11T11:48:29+00:00
url: /archives/69
views:
  - 455
bigfa_ding:
  - 1
categories:
  - Android
tags:
  - github
  - svn

---
## 背景

有时候需要在GitHub上下载单个目录里面的文件，而不需要去克隆整个项目，太大的项目克隆起来太慢。

## 解决方案

GitHub是支持SVN访问的，所以可以使用SVN下载单个目录。

### 下载master分支

复制浏览器地址栏中的地址，如：

`https://github.com/bumptech/glide/tree/master/integration`

将`tree`改为`trunk`即可。

`https://github.com/bumptech/glide/trunk/integration`

### 下载其他分支

如：

`https://github.com/bumptech/glide/tree/3.0/integration`

将`tree`改为`branches`即可。

`https://github.com/bumptech/glide/branches/3.0/integration`

然后`svn checkout` 即可。