---
layout: post
title: Ubuntu Server 16 安装Docker详解
date: 2017-04-03
tag: docker
---

### 安装 Ubuntu Server 16.04.2 LTS

下载地址[Ubuntu Server 16.04.2 LTS](https://www.ubuntu.com/download/server)

安装过程不赘述 ......

### 安装 软件环境

#### aptitude

aptitude和apt的区别可以Google，大牛们的推荐是使用aptitude，一会就会发现使用aptitude安装docker很方便，而使用apt安装docker会发现很多依赖包无法安装的问题。

```bash
sudo apt-get install aptitude
```

#### openssh-service

用于ssh远程登录

```bash
sudo aptitude install openssh-service
```

#### vim

好用的编辑器

```bash
sudo aptitude install vim
```

#### 配置国内下载源

```bash
#打开编辑sources
sudo vim /etc/apt/sources.list
#删除全部的内容
:,.$d
```

内容如下：

```list
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# 源码
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# Canonical 合作伙伴和附加
deb http://archive.canonical.com/ubuntu/ xenial partner
deb http://extras.ubuntu.com/ubuntu/ xenial main
```

更新源

```bash
sudo apt-get update
或者
sudo aptitude update
```

### 安装 Docker

#### 安装http和ca证书

```bash
sudo apt-get install apt-transport-https ca-certificates
```

#### 增加新的GPG 密钥

```bash
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

#### 新增或编辑source列表里的docker.list文件

```bash
sudo vim /etc/apt/sources.list.d/docker.list
#添加以下内容
deb https://apt.dockerproject.org/repo ubuntu-xenial main
#保存
:wq
sudo apt-get purge lxc-docker
```

#### 查看可使用版本

```bash
sudo apt-cache policy docker-engine
sudo apt-get install linux-image-extra-$(uname -r)
```

#### 正式安装

```bash
sudo apt-get update
sudo apt-get install docker-engine
sudo service docker start
sudo docker run hello-world
```

### 配置国内Docker源加速器

注册使用阿里Docker Hub，[开发者平台](https://dev.aliyun.com/search.html),注册成功之后去`管理中心`就能看见Docker Hub 镜像站点，系统会为你提供一个专属的加速地址，如：

```url
https://dl4gfdyn.mirror.aliyuncs.com
```

配置加速器,以下配置仅针对Ubuntu，其他系统请查看官网提示

```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://dl4gfdyn.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
