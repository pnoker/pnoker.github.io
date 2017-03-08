---
layout: post
title: 修改 Tomcat Connector 运行模式
date: 2017-03-08
tag: Tomcat
---

Tomcat是一个小型的轻量级应用服务器，也是JavaEE开发人员最常用的服务器之一。不过，许多开发人员不知道的是，Tomcat Connector(Tomcat连接器)有 **bio、nio、apr** 三种运行模式，那么这三种运行模式有什么区别呢，我们又如何修改Tomcat Connector的运行模式来提高Tomcat的运行性能呢？

下面，我们先大致了解Tomcat Connector的三种运行模式。

### bio
bio(blocking I/O)，顾名思义，即阻塞式I/O操作，表示Tomcat使用的是传统的Java I/O操作(即java.io包及其子包)。Tomcat在默认情况下，就是以bio模式运行的。遗憾的是，就一般而言，bio模式是三种运行模式中性能最低的一种。我们可以通过Tomcat Manager来查看服务器的当前状态。
[查看Tomcat Manager用户配置](http://www.365mini.com/page/tomcat-manager-user-configuration.htm)

![](/images/posts/tomcat/tomcat-status-bio.png)


### nio
nio(new I/O)，是Java SE 1.4及后续版本提供的一种新的I/O操作方式(即java.nio包及其子包)。Java nio是一个基于缓冲区、并能提供非阻塞I/O操作的Java API，因此nio也被看成是non-blocking I/O的缩写。它拥有比传统I/O操作(bio)更好的并发运行性能。要让Tomcat以nio模式来运行也比较简单，我们只需要在Tomcat安装目录/conf/server.xml文件中将如下配置：
```xml
<Connector port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443" />
```
中的protocol属性值改为```org.apache.coyote.http11.Http11NioProtocol```即可：
```xml
<Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"
connectionTimeout="20000"
redirectPort="8443" />
```
此时，我们就可以在Tomcat Manager中看到当前服务器状态页面的HTTP协议的Connector运行模式已经从```http-bio-8080```变成了```http-nio-8080```。
![](/images/posts/tomcat/tomcat-status-nio.png)


### apr
apr(Apache Portable Runtime/Apache可移植运行时)，是Apache HTTP服务器的支持库。你可以简单地理解为，Tomcat将以JNI的形式调用Apache HTTP服务器的核心动态链接库来处理文件读取或网络传输操作，从而大大地提高Tomcat对静态文件的处理性能。 Tomcat apr也是在Tomcat上运行高并发应用的首选模式。如果我们的Tomcat不是在apr模式下运行，在启动Tomcat的时候，我们可以在日志信息中看到类似如下信息：
```log
2013-8-6 16:17:49 org.apache.catalina.core.AprLifecycleListener init
信息: The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: xxx/xxx(这里是路径信息)
```
Tomcat apr运行模式的配置是三种运行模式之中相对比较麻烦的一种。据官方文档所述，Tomcat apr需要以下三个组件的支持：

>- APR library[APR库]

>- JNI wrappers for APR used by Tomcat (libtcnative)[简单地说，如果是在Windows操作系统上，就是一个名为tcnative-1.dll的动态链接库文件]

>- OpenSSL libraries[OpenSSL库]

此外，与配置nio运行模式一样，也需要将对应的Connector节点的protocol属性值改为org.apache.coyote.http11.Http11AprProtocol。 不过，上述繁琐的操作都是Tomcat 7.0.30之前的版本才需要这样配置，从Tomcat 7.0.30版本开始，Tomcat已经自带了tcnative-1.dll等文件，并且默认就是在Tomcat apr模式下运行，因此我们只需要下载最新版本的Tomcat直接使用即可。
![](/images/posts/tomcat/tomcat-status-apr.png)

此外，即使不使用Tomcat Manager，我们也可以区分出Tomcat当前的运行模式。如果以不同的Connector模式启动，在Tomcat的启动日志信息中一般会包含类似如下的不同内容，我们只需要根据这些信息即可判断出当前Tomcat的运行模式：
>- bio

信息: Starting ProtocolHandler ["http-bio-8080"] 2013-8-6 16:17:50 org.apache.coyote.AbstractProtocol start
>- nio

信息: Starting ProtocolHandler ["http-nio-8080"] 2013-8-6 16:59:53 org.apache.coyote.AbstractProtocol start
>- apr

信息: Starting ProtocolHandler ["http-apr-8080"] 2013-8-6 17:03:07 org.apache.coyote.AbstractProtocol start