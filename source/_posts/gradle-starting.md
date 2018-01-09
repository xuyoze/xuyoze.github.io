---
title:  Gradle 使用小记
date: 2018-01-09 15:10:55
categories: 编程
tags: gradle
thumbnailImage: gradle-01.png
---
Maven和Gradle 是Java生态下常用的两个项目构建工具。
其中，Maven多用于Java项目构建，而Gradle在Android的世界中被广泛使用， 特别是在Google放弃Eclipse转用AS作为首选IDE之后，Gradle又得到了相当大范围的推广。
<!-- more-->

## Gradle的安装

**Window下Gradle安装**

前提: 本机已安装JDK

下载 Gradle4.4.1 zip包[下载地址](https://services.gradle.org/distributions/gradle-4.4.1-all.zip)

解压到本机磁盘:`c:\gradle`

配置本机PATH: 添加`C:\gradle\gradle-4.4.1\bin`到PATH值

测试:
```
gradle -v
-----------------
Gradle 4.4.1
-----------------
```

## Gradle基本概念

- 项目:指我们的构建产物（比如Jar包）或实施产物（将应用程序部署到生产环境）。**一个项目包含一个或多个任务。**
- 任务:指不可分的最小工作单元，执行构建工作（比如编译项目或执行测试）。

每一构建都包含一个或多个项目.
{% asset_img gradle-01.png 关系 %}


## Gradle常用命令


## 参考
- [Gradle实战](https://lippiouyang.gitbooks.io/gradle-in-action-cn/content/)
- [用户指南2.0](http://gradledoc.qiniudn.com/2.0/userguide/userguide.html)
- [用户指南2.0](https://waylau.gitbooks.io/gradle-2-user-guide/)
- [用户指南3.0](https://waylau.gitbooks.io/gradle-3-user-guide/)
