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
    def server = "192.168.87.76"
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

git
### 3. Web项目的构建

#### 3.1 Tomcat Manager配置
#### 3.2 Jenkins中Tomcate Mannager凭据添加

### 4. 服务项目的构建
#### 4.1 jar包发布
#### 4.2 SSH配置
#### 4.3 服务编写


