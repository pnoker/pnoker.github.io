---
layout: post
title: 修改 Tomcat Connector 运行模式
date: 2017-03-08
tag: Tomcat
---

Tomcat是一个小型的轻量级应用服务器，也是JavaEE开发人员最常用的服务器之一。不过，许多开发人员不知道的是，Tomcat Connector(Tomcat连接器)有 **bio、nio、apr** 三种运行模式，那么这三种运行模式有什么区别呢，我们又如何修改Tomcat Connector的运行模式来提高Tomcat的运行性能呢？

下面，我们先大致了解Tomcat Connector的三种运行模式。

### bio
bio(blocking I/O)，顾名思义，即阻塞式I/O操作，表示Tomcat使用的是传统的Java I/O操作(即java.io包及其子包)。Tomcat在默认情况下，就是以bio模式运行的。遗憾的是，就一般而言，bio模式是三种运行模式中性能最低的一种。我们可以通过Tomcat Manager来查看服务器的当前状态。[查看Tomcat Manager用户配置](http://www.365mini.com/page/tomcat-manager-user-configuration.htm)
![](/images/posts/tomcat/tomcat-status-bio.png)

### nio
