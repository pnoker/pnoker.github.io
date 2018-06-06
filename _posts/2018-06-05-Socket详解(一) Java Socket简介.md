---
layout: post
title: Socket详解(一):Java Socket简介
date: 2018-06-05
tag: Java
---

### 概念

`网络进程间通讯` 两个进程唯一确定，网络层的 `Ip` 可以唯一标识网络主机，传输层的 `Ip+Port` 可以唯一确定网络间两进程。

标识网络两进程后，进行通讯一般是利用socket。

socket源于unix，延续了 “一切皆文件” 的概念，都使用 “打开->读写->关闭” 文件操作的方式。

### 通讯过程

Server ： `socket`->`bind`->`listen`->`accept`->`receive`->`close`

Client ： `socket`->`connect`->`send`->`close`


server端需要获取ip和绑定端口号，让client调用，所以需要bind和listern

client的端口号在connect的时候绑定端口号

### 建立连接过程

同TCP/IP三次握手建立连接

### 释放过程

四次释放释放连接

### Demo

[Java Socket Demo](/2018/06/Socket%E8%AF%A6%E8%A7%A3(%E4%B8%80)-Java-Socket%E7%AE%80%E4%BB%8B/)