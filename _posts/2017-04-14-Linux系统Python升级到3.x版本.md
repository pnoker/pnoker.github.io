---
layout: post
title: Centos7升级系统自带的Python到3.x版本
date: 2017-04-14
tag: Linux
---

安装完Centos7之后系统自带的Python是2.x版本，由于某些项目需要使用3.x版本的Python，那么就需要升级系统的Python。

### 查看Python版本号

```bash
python --version
Python 2.6.6 (r266:84292, Nov 22 2013, 12:16:22)   
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2  
Type "help", "copyright", "credits" or "license" for more information.  
>>>   
```

### 安装

#### 下载地址

[Python mirrors.sohu](http://mirrors.sohu.com/python)

选择合适版本号进行下载，本人下载的是3.5.1版本 `Python-3.5.1.tgz`

#### 解压

```bash
tar zxvf Python-3.5.1.tgz
```

#### 安装升级所需的依赖

```bash
#centos
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```

#### 正式安装

```bash
cd Python-3.5.1
./configure --prefix=/usr/local/python3.5 
make 
sudo make install 
sudo mv /usr/bin/python /usr/bin/python.bak  
sudo ln -s /usr/local/python3.5/bin/python3.5 /usr/bin/python 
```

