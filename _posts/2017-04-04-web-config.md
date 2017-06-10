---
layout: post
title: Java Web 环境配置
date: 2017-04-04
tag: web
---

之前一直没时间整理这方面的资料，今天难得有时间进行整理，方便以后自己快速搭建一套Java Web开发和运行环境。

### Java 安装

官网下载合适的jdk,本人使用的是`jdk-7u79-linux-x64.tar.gz`,接下来就以该版本的jdk为例，进行Java环境变量配置

```bash
cd /usr/local
#创建java文件夹
sudo mkdir java
#解压
tar zxvf jdk-7u79-linux-x64.tar.gz
#编辑环境变量配置文件
cd /etc
vim profile

#java config
export JAVA_HOME=/usr/local/java/jdk1.7.0_79
export JRE_HOME=/usr/local/java/jdk1.7.0_79/jre
export PATH=$PATH:/usr/local/java/jdk1.7.0_79/bin
export CLASSPATH=./:/usr/local/java/jdk1.7.0_79/lib:/usr/local/jdk7/jdk1.7.0_79/jre/lib
#刷新配置文件
source /etc/profile
```

### Tomcat 安装

```bash
#新建tomcat目录
cd /usr/local
mkdir tomcat
#解压
cd tomcat
tar zxvf apache-tomcat-7.0.73.tar.gz
```

### Maven 安装

```bash
#新建目录
cd /usr/local
mkdir maven
#解压
cd maven
tar zxvf apache-maven-3.3.9-bin.tar.gz
#编辑环境变量
cd /etc
vim profile
#Maven config
export MAVEN_HOME=/usr/local/maven/apache-maven-3.3.9
export PATH=${PATH}:${MAVEN_HOME}/bin
#刷新环境变量
source /etc/profile
```

### Git 安装

```bash
sudo aptitude install git
```

### Mysql 安装

```bash
sudo aptitude install mysql-server
会提示设置密码，视情况而定
```

### 配置 Tomcat 随系统自启

```bash
#编辑环境变量
cd /etc/init.d/
vim tomcat

#!/bin/bash
# tomcat 开机自动启动
# chkconfig: 2345 10 90  
# description: Starts and Stops the Tomcat daemon. 

##################路径改变，需要修改######################
JAVA_HOME=/usr/local/java/jdk1.7.0_79
export JAVA_HOME
PATH=$JAVA_HOME/bin:$PATH
export PATH 

##################路径改变,需要修改########################
CATALINA_HOME=/usr/local/tomcat/apache-tomcat-7.0.73
case $1 in
start)
sh $CATALINA_HOME/bin/startup.sh
;;
stop)
sh $CATALINA_HOME/bin/shutdown.sh
;;
restart)
sh $CATALINA_HOME/bin/shutdown.sh
sh $CATALINA_HOME/bin/startup.sh
;;
esac
exit 0
```

### 修改 Maven 下载源

编辑config文件，settings.xml,修改mirrors为国内阿里云仓库

```xml
<mirrors>
        <!-- 阿里云仓库 -->
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
    </mirrors>
```

### 配置 git

```bash
git config --global user.name username
git config --global user.email your@mail.com
```

### 问题1：卸载系统自带的openjdk

```bash
rpm -qa | grep java
rpm -e --nodeps java-xxx
```

### 问题2：生成sshKey

```bash
ssh-keygen -t rsa
#复制到远程机器
ssh-copy-id username@hostname

[注]
username:远程主机用户名，例intel
hostname:远程主机host，例192.168.1.10/hadoop-1
ssh-copy-id intel@hadoop-1
```

### 问题3：文件远程复制

```bash
#文件传输
scp -r hbase intel@hadoop-1:/usr/local
```