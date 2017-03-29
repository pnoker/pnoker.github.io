---
layout: post
title: Ubuntu 配置网络
date: 2016-12-24
tag: Linux
---

### 查看网卡信息

```shell
ifconfig

#console
eth0      Link encap:Ethernet  HWaddr 00:0c:29:b9:6e:6a  
          inet addr:192.168.182.128  Bcast:192.168.182.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:feb9:6e6a/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6122 errors:0 dropped:0 overruns:0 frame:0
          TX packets:3893 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:7093968 (7.0 MB)  TX bytes:219851 (219.8 KB)
          Interrupt:19 Base address:0x2000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:179 errors:0 dropped:0 overruns:0 frame:0
          TX packets:179 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:13783 (13.7 KB)  TX bytes:13783 (13.7 KB)
```

### 配置网卡

```shell
sudo ifconfig eth0 192.168.1.1 netmask 255.255.255.0
```

### 重启网络

```shell
sudo /etc/init.d/networking restart

#或者单独重启部分网卡，这样其他网卡不受影响
ifdown eth0
ifup eth0
```
