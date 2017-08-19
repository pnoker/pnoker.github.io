---
layout: post
title: Centos修改系统DNS
date: 2017-04-15
tag: Linux
---

### 查看系统DNS服务器

```bash
sudo cat /etc/resolv.conf

#
nameserver 8.8.8.8
nameserver 61.144.56.100
nameserver 192.168.1.1
```

### 修改系统DNS服务器

```bash
sudo vim /etc/resolv.conf

#添加nameserver,可以添加多个
#lg.
nameserver 8.8.8.8
```

### 重新启动网络服务

```bash
sudo service network restart
```

