---
title: Jenkins自动构建发布SpringBoot项目
date: 2018-03-07 17:15:36
categories: 编程
tags: [java,centos,docker,jenkins,tomcat]
thumbnailImage: jenkins.png
---
手头在做的java项目,希望通过Jenkins进行构建并发布到测试环境机器,从而避免手动发布的繁琐步骤.

由于自己从前没有部署,配置过jenkins发布项目,使用过程中遇到了不少问题,再次记录下备忘!

<!--nore-->

### Jenkins的安装

Jenkins的安装步骤,其实还蛮简单的,网上有不少的教程,现在Jenkins同样提供Docker版的镜像. 网络上有大把的教程.如果不熟悉可以下载来看一下.
我之前也写过一篇关于Jenkins Docker版本在CentOS下安装的简单小文章,您也可以作为参考.

### 1. 项目简单描述
为了说明Jenkins的配置过程,我创建了一个简单的Java项目:
{% asset_img 1.png project %}

- demo-api 用于多站点演示
- demo-web 用于多站点演示
- demo-svc 用户服务演示

同时, 我们会在Git上创建两个分支,master,develop,从而演示多分支,多环境构建.



### 2. 使用pipeline创建多分支任务

#### 2.1 分支拉取

#### 2.2 Jenkinsfile

### 3. Web项目的构建

#### 3.1 Tomcat Manager配置
#### 3.2 Jenkins中Tomcate Mannager凭据添加

### 4. 服务项目的构建
#### 4.1 jar包发布
#### 4.2 SSH配置
#### 4.3 服务编写


