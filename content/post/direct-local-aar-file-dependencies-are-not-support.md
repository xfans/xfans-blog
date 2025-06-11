---
title: "Direct local .aar file dependencies are not supported子模块中本地AAR依赖的问题"
description: "Direct local .aar file dependencies are not supported子模块中本地AAR依赖的问题"
date: "2025-06-11T09:46:51+08:00"
thumbnail: ""
categories:
  - "Android开发"
tags:
  - "Android"
  - "Gradle"
  - "AAR"

在Android开发中，当子模块中引用本地aar包的时候，release包会打包失败，提示Direct local .aar file dependencies are not supported。而且Android Studio的新版本中已经找不到直接依赖本地AAR文件的选项了。

## 解决方案

我采用的解决方案是在构建过程开始前，将这些AAR文件发布到本地Maven仓库中。下面是具体实现方法：

### 1. 配置构建脚本

首先，在封装模块的build.gradle文件中添加必要的插件和配置：

```groovy
plugins {
    id 'com.android.library'
    id 'kotlin-android'
    id 'maven-publish'
}

// 确保所有项目都能访问本地Maven仓库
parent.allprojects {
    repositories {
        mavenLocal()
    }
}
```

### 2. 配置发布任务

接下来，配置发布任务将AAR文件发布到本地Maven仓库：

```groovy
publishing {
    publications {
        libOne(MavenPublication) {
            groupId 'com.example.library'
            artifactId 'module-one'
            version '1.0'
            artifact("$libsDirName/module-one.aar")
        }
        libTwo(MavenPublication) {
            groupId 'com.example.library'
            artifactId 'module-two'
            version '1.0'
            artifact("$libsDirName/module-two.aar")
        }
        libThree(MavenPublication) {
            groupId 'com.example.library'
            artifactId 'module-three'
            version '1.0'
            artifact("$libsDirName/module-three.aar")
        }
    }
}
```

### 3. 配置构建依赖

确保在构建前执行发布任务：

```groovy
afterEvaluate {
    tasks.clean.dependsOn("publishToMavenLocal")
    tasks.preBuild.dependsOn("publishToMavenLocal")
}
```

### 4. 添加依赖

最后，在dependencies块中添加对已发布库的依赖：

```groovy
dependencies {
    implementation "com.example.library:module-one:1.0"
    implementation "com.example.library:module-two:1.0"
    implementation "com.example.library:module-three:1.0"
    // 同时需要添加这些AAR的传递依赖
}
```

## 注意事项

1. **首次同步问题**：首次同步项目时依赖可能找不到，但在执行clean或assemble任务后，依赖会被正确发布和使用。

2. **代码组织建议**：可以将这些配置提取到单独的文件中，避免build.gradle文件过于臃肿。

3. **适用范围**：这个方案适用于开发环境，如果要将模块作为库发布，需要采用其他方式。

4. **CI环境**：在持续集成环境中同样适用，只需确保先执行clean任务即可。