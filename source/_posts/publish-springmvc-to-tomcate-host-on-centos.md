---
title: 发布JAVA SpringMVC站点到Centos7
date: 2018-01-04 16:05:53
categories: 编程
tags: [java,springmvc,centos,tomcat]
---

本文主要记录发布`SpringMVC`站点到 `CentOS 7` 中的`tomcat`过程中出现的一些问题.

## Tomcat 安装

官网线下常用的Tomcat版本, 一般现在生产环境机器采用的版本为8.x, 所以使用8.x进行安装.

<!--more-->

### 下载安装

访问[Apache Tomcat](https://tomcat.apache.org/download-80.cgi)官网,

找到 tar包地址并复制,

```bash
# 切换常用目录
cd /usr/local/

# 下载指定的tomcat版本
wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz

>>>
# 解压tar包
tar -zxvf apache-tomcat-8.0.26.tar.gz

# 删除原始包
rm -rf apache-tomcat-8.0.26.tar.gz.tar.gz
# 重命名
mv apache-tomcat-8.0.26 tomcat

```
### 启动

```bash
# 启动tomcat
/usr/local/tomcat/bin/startup.sh

Using CATALINA_BASE:  /usr/local/tomcat
Using CATALINA_HOME:  /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/java/jdk1.8.0_60
Using CLASSPATH:      /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Tomcat started.
```

### 运行

通过以下地址查看tomcat是否运行正常:

http://192.168.11.52:8080/

看到tomcat系统界面，说明安装成功！

### 停止

```bash
# 停止tomcat
/usr/local/tomcat/bin/shutdown.sh
```
### 常见问题

#### JAVA路径不正确

```
Neither the JAVA_HOME nor the JRE_HOME environment variable is defined
At least one of these environment variable is needed to run this program
```
则要注意提前设置java路径

在`apache-tomcat-8.0.26/bin/setclasspath.sh`中添加一下内容
```bash
export JAVA_HOME=/usr/java/jdk1.8.0_60  
export JRE_HOME=/usr/java/jdk1.8.0_60/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
#### 防火墙禁止端口

防火墙开放8080端口,增加8080端口到防火墙配置中，执行以下操作：

```bash
vi /etc/sysconfig/iptables
# 增加以下代码
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```

重启防火墙 
```bash
# service iptables restart
```

## Tomcat 配置


----- 未完 ----