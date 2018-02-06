---
title: 在CentOS7上使用Docker部署Jenkins
date: 2018-02-06 17:07:53
categories: 编程
tags: [centos,docker,jenkins]
thumbnailImage: 55c9c3cd17e71.jpg
---

> 公司使用了Jenkins进行项目构建,为了熟悉整个流程, 我决定自己搭建一个Jenkins环境练练手.

### Jenkins是啥

[Jenkins](http://baike.sogou.com/v51626369.htm?fromTitle=Jenkins)，之前叫做Hudson，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，包括：

1、持续的软件版本发布/测试项目。

2、监控外部调用执行的工作。

<!-- more -->

### 安装
使用Jenkins官网提供的dock版本, 所以我们首先要在centos中安装并配置好docker.
着涉及到docker的安装,可以参考网络上相关的教程.

另外dock版的jenkins需要jdk8及以上, 也需要确保已经正确安装.
如果一切就绪,我们就可以从dock库中拉取`jenkins-blueocean` 镜像了.

```
docker pull jenkinsci/blueocean
```
如果网速不快的话,可能要等一会.如果不发忍受下载过慢.可以尝试修改docker镜像源到国内的第三方服务器.或者直接从国内镜像服务器拉取镜像.

参考[Jenkins官网提供的基本教程](https://jenkins.io/doc/book/installing/), 我们只需要输入简单的docker命令即可运行Jenkins, 以下是官网提供的运行命令,同时官网提供了每个参数的详细说明!!
```bash
docker run \
  -u root \
  --rm \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
```

{% alert info %}
文档中同时存在`-d` 和 `--rm` . 但是提示 `-d` 和 `--rm` 相互冲突 
其容器中的`/var/jenkins_home` 被挂载到了宿主机的 `/var/lib/docker/volumes/jenkins-data/_data` 目录下.

如果端口8080已经被占用,则需要修改相关端口, 否则命令会执行失败!

{% endalert %}

如果一切正常,我们可以看到一个运行中的docker容器:
{% asset_img 1.png docker %}

通过使用如下命令查看dock运行相关信息.
```bash
## 进入Jenkins容器执行命令
docker exec -it <容器id> bash
## 查看容器输出的日志
docker logs <容器id> [-f(滚动的)]
```

### 运行,改密

接下来我们就可以访问已运行起来的Jenkins页面了.

第一次访问,需要使用Jenkins随机生成的密码.官王中有详细的运行步骤. 也可以根据页面提示,得到Jenkins初始密码.
{% asset_img 3.jpg unlock %}

到此我们就完成了Jenkins的安装, 哇 好傻瓜..
