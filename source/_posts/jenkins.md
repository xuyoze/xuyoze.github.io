---
title: Jenkins自动构建发布SpringBoot项目
date: 2018-03-07 17:15:36
categories: 编程
tags: [java,centos,docker,jenkins,tomcat]
thumbnailImage: jenkins.png
---
手头在做的java项目,希望通过Jenkins进行构建并发布到测试环境机器,从而避免手动发布的繁琐步骤.

由于自己从前没有部署,配置过jenkins发布项目,使用过程中遇到了不少问题,再次记录下备忘!

<!--more-->
<!-- toc -->

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

#### 2.1 项目创建
{% asset_img 2-1.jpg creatnew %}

使用jenkein pipeline 模式,对项目的不同分支进行构建,因此我们在jenkins中选择多分支pipeline类型创建项目.
{% asset_img 2-2.jpg creatnew %}

拉取项目源码,我的代码是存放于github上的, 顾可以直接算则github源,如果是公司内建的git服务器,可以选择git.
github的插件应该是默认安装的, 如果没有请在插件管理中添加相应的github插件.

{% asset_img 0309-1648.jpg source %}

接下来修改Jenkinsfile名称
{% asset_img 0309-1649.jpg source %}

#### 2.2 Jenkinsfile
使用Jenkins Pipeline方式进行项目构建项目,需要熟悉Jenkinsfile的编写方法.
可以[参考官网](https://jenkins.io/doc/)给出的教程,进行相关学习.
这里贴一下演示使用的源码, 其中的某些地方会在后面进行说明:

```groovy
#!groovy
def deployToTomcat(localWarPath, warName) {
    def server = "192.168.0.76"
    def result = sh(
            script: "curl --upload-file ${localWarPath} 'http://${TOMCAT_ACCOUNT}@${server}/manager/text/deploy?update=true&path=/${warName}'",
            returnStdout: true
    ).trim()
    if (result.startsWith('OK')) {
        echo result
    } else {
        error result
    }
}

def deployService(localJarPath, remotePath) {
    sh "ssh 'root@localhost76' 'service demo-svc stop'"
    sh "scp ${localJarPath} root@localhost76:${remotePath}"
    sh "ssh 'root@localhost76' 'service demo-svc start'"
    sh "ssh 'root@localhost76' 'service demo-svc status'"
}

pipeline {
    agent any
    environment {
        TOMCAT_ACCOUNT = credentials('tomcat-account-test-env')
    }
    stages {
        stage('Build Test') {
            when {
                branch 'develop'
            }
            steps {
                sh 'mvn -B clean package -Dspecific -P dev'
            }
        }
        stage('Deploy Test') {
            when {
                branch 'develop'
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            parallel {
                stage('Deploy Test Api') {
                    steps {
                        deployToTomcat('demo-api/target/demo-api.war', 'demo-api')
                    }
                }
                stage('Deploy Test Web') {
                    steps {
                        deployToTomcat('demo-web/target/demo-web.war', 'demo-web')
                    }
                }
                stage('Deploy Test Svc') {
                    steps {
                        deployService('demo-svc/target/demo-svc.jar', '/var/services/demo-svc.jar')
                    }
                }
            }
        }
    }
}
```

### 3. Tomcat Manager配置

在Tomcat中开启Manager要经过来两个步骤处理.

**1. 授权**

修改Tomcat管理站点中的相关配置
```shell
# 转到manager站点所在目录
cd /usr/local/tomcat/webapps/manager/META-INF
vim context.xml
```
修改配置文件`content.xml`
```xml
<Context antiResourceLocking="false" privileged="true" >
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
    <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```
`Value`节点的`allow`属性中添加对应的Jenkins服务器IP地址.

如果是测试环境,则可以直接注释掉 `Value` 和 `Manager` 两个节点.

正式环境,可别这么干~~

**2. 添加用户**

修改tomcat的`tomcat-user.xml`
```bash
cd /usr/local/tomcat/conf/
vim tomcat-users.xml
```
在最后添加相应的访问用户:
```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="admin-gui"/>
<role rolename="admin-script"/>
<role rolename="tomcat-gui"/>
<user username="tomcat" password="tomcat" roles="manager-script,admin-script,admin-gui,tomcat,manager-gui"/>
```
指定用户名 username 以及密码 password

已定义的角色可以查看tomcat官方文档,查看具体作用.

**3. 在Jenkins中添加TomcatManager凭据**
这里我在jenkens上添加名称为`tomcat-account-test-env`的manager访问凭据,上面的Jenkinsfiles用到该凭据.

### 4. 服务项目的构建

如果web站点项目打包为War包的方式,则可以通过TomcatManager进行部署.

如果是springboot项目并打包为jar包进行部署,接下来会描述相关设置

#### 4.1 jar包发布
对于jar包的发布,可以使用ssh拷贝的方式.

如下定义了一个方法执行发布的各个步骤:
```groovy
def deployService(localJarPath, remotePath) {
    sh "ssh 'root@localhost76' 'service demo-svc stop'"
    sh "scp ${localJarPath} root@localhost76:${remotePath}"
    sh "ssh 'root@localhost76' 'service demo-svc start'"
    sh "ssh 'root@localhost76' 'service demo-svc status'"
}
```
两台机器之间通过SSH互信方式进行通信,SSH的原理以及其他应用可参考 [SSH原理与运用](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)

#### 4.3 服务编写
在Centos7.+ 版本中,可以通过编写shell脚本的方式将jar包做成服务.

以`demo-svc.jar`部署为例,编写相应的shell脚本

1. 启动脚本`demo-svc-start`
```bash
#!/bin/sh
export JAVA_HOME=/opt/java
export PATH=$JAVA_HOME/bin:$PATH # 指定JDK的目录
java -jar /var/services/demo-svc/demo-svc.jar > /var/services/demo-svc/logs/demo-svc.log &
echo $! > /var/run/demo-svc.pid
```
2. 停止脚本`demo-svc-stop`
```bash
#!/bin/sh
PID=$(cat /var/run/demo-svc.pid)
kill -9 $PID
```
3. 创建服务`robot-svc.service`
```bash
###CentOS 7.x ##
[Unit]
Description=service for demo
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
ExecStart=/var/services/demo-svc/demo-svc-start
ExecStop= /var/services/demo-svc/demo-svc-stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

4. 部署服务

将`robot-svc.service`拷贝到`/usr/lib/systemd/system`

执行命令启动服务,添加服务到开机启动
```bash
systemctl enable robot-svc #开机自启动
systemctl start robot-svc #启动
systemctl stop robot-svc #停止
```

(完)
