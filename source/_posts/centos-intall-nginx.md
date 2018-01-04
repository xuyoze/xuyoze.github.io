---
title: CentOS 安装Nginx步骤
date: 2018-01-04 12:20:58
categories: 转载
tags: [centos,nginx]
thumbnailImage: nginx.png
---
CentOS 下安装Nginx相关步骤,以及一些常见的错误.

{% alert info %}
本篇文章内容为转载内容，原文为博客园博主[hafiz](http://www.cnblogs.com/hafiz/),查看原文请移步[Centos7安装Nginx实战](http://www.cnblogs.com/hafiz/p/6891458.html)
{% endalert %}
<!--- more -->

<!--toc-->

## 一、背景
最近在写一些自己的项目，用到了nginx，所以自己动手来在Centos7上安装nginx,以下是安装步骤。

## 二、基本概念以及应用场景

### 1.什么是nginx
Nginx是一款使用C语言开发的高性能的http 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师Igor Sysoev所开发，官方测试nginx能够支支撑5万并发链接，并且cpu、内存等资源消耗却非常低，运行非常稳定。

### 2.Nginx的应用场景

1)、http服务器。Nginx是一个http服务可以独立提供http服务。可以做网页静态服务器。

2)、虚拟主机。可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机。

3)、反向代理，负载均衡。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况。

## 三、安装步骤

### 1.检查并安装所需的依赖软件

1).gcc:nginx编译依赖gcc环境
```
yum install gcc-c++
```
2).pcre:(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式.
```
yum install -y pcre pcre-devel
```
3).zlib：该库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip。
```
yum install -y zlib zlib-devel
```
4).openssl:一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。nginx不仅支持http协议，还支持https（即在ssl协议上传输http）.
```
yum install -y openssl openssl-devel
```
### 2.下载nginx源码包

下载命令：
```
wget http://nginx.org/download/nginx-1.12.0.tar.gz
```
### 3.解压缩源码包并进入
1).解压缩：tar -zxvf nginx-1.12.0.tar.gz
2).进入解压缩后文件夹：cd nginx-1.12.0

### 4.配置编译参数(可以使用./configure --help查询详细参数)
命令：
```bash
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
```
注：安装之前需要手动创建上面指定的nginx文件夹，即`/var/temp`、`/var/temp/nginx`、`/var/run/nginx/`文件夹，否则启动时报错

### 5.编译并安装
```
make && make install
```
可以进入`/usr/local/nginx`查看文件是否存在conf、sbin、html文件夹，若存在则安装成功

### 6.启动nginx

1).进入安装目录
```bash
whereis nginx
# /usr/local/nginx

cd /usr/local/nginx/sbin/
```
2).启动
```
./nginx
```
3).若报错：
```
[emerg] open() "/var/run/nginx/nginx.pid" failed (2: No such file or directory)
```
需要查看下是不是在/var/run文件夹下不存在nginx文件夹，不存在则新建

4).查看是否启动：`ps -ef | grep nginx`

如果有master和worker两个进程证明启动成功
{% asset_img 894443-20170522211619117-1585471045.png nginx %}


注意：执行`./nginx`启动nginx，这里可以 `-c` 指定加载的nginx配置文件，如下：
```
./nginx -c /usr/local/nginx/conf/nginx.conf
```
如果不指定 `-c`，nginx在启动时默认加载`conf/nginx.conf`文件，此文件的地址也可以在编译安装nginx时指定`./configure`的参数(--conf-path= 指向配置文件（nginx.conf）)

### 7.停止

1).暴利kill(不推荐使用)
```
kill -9 processId
```
2).快速停止
```
cd /usr/local/nginx/sbin && ./nginx -s stop
```
此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程

3).完整停止(建议使用)
```
cd /usr/local/nginx/sbin && ./nginx -s quit
```
此方式停止步骤是待nginx进程处理任务完毕进行停止

### 8.重启及重新加载配置

1.先停止再启动（建议使用）
```
./nginx -s quit && ./nginx
```
2.重新加载配置文件
```
./nginx -s reload
```
### 9.测试

nginx安装成功，启动nginx,即可通过ip地址来访问nginx:
{% asset_img 894443-20170522211459382-336608973.png nginx %}


## 问题汇总

### CentOS 防火墙

> CentOS 7.0默认使用的是firewall作为防火墙，这里改为iptables防火墙步骤。

1. 关闭firewall：
```bash
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
systemctl restart firewalld.service #重启firewall
firewall-cmd --query-port=80/tcp #查询端口是否开放
firewall-cmd --add-port=80/tcp #开启指定端口
```

2. iptables防火墙（这里iptables已经安装，下面进行配置）
```bash
vi /etc/sysconfig/iptables-config  #编辑防火墙配置文件
```
```yml
# sampleconfiguration for iptables service
# you can edit thismanually or use system-config-firewall
# please do not askus to add additional ports/services to this default configuration
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT[0:0]
:OUTPUT ACCEPT[0:0]
-A INPUT -m state--state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -jACCEPT
-A INPUT -i lo -jACCEPT
-A INPUT -p tcp -mstate --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -jACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8080-j ACCEPT
-A INPUT -j REJECT--reject-with icmp-host-prohibited
-A FORWARD -jREJECT --reject-with icmp-host-prohibited
COMMIT
```
```bash
:wq! #保存退出
```

{% alert warning %}
备注：这里使用80和8080端口为例。***部分一般添加到“-A INPUT -p tcp -m state --state NEW -m tcp--dport 22 -j ACCEPT”行的上面或者下面，切记不要添加到最后一行，否则防火墙重启后不生效。
{% endalert %}

```bash
systemctl restart iptables.service #最后重启防火墙使配置生效
systemctl enable iptables.service #设置防火墙开机启动
```