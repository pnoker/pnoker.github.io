---
layout: post
title: 问：Tomcat并发量多少？
date: 2017-03-21
tag: Tomcat
---
### maxThreads
对tomcat来说，每一个进来的请求(request)都需要一个线程，直到该请求结束。如果同时进来的请求多于当前可用的请求处理线程数，额外的线程就会被创建，直到到达配置的最大线程数( **maxThreads** 属性值)。

### acceptCount
如果仍就同时接收到更多请求，这些来不及处理的请求就会在Connector创建的ServerSocket中堆积起来，直到到达最大的配置值( **acceptCount** 属性值)。至此，任何再来的请求将会收到connection refused错误，直到有可用的资源来处理它们。

### maxConnections
这里我们关心的是tomcat能同时处理的请求数和请求响应时间，显然Connector元素的maxThreads和acceptCount属性对其有直接的影响。无论acceptCount值为多少，maxThreads直接决定了实际可同时处理的请求数。而不管maxThreads如何，acceptCount则决定了有多少请求可等待处理。然而，不管是可立即处理请求还是需要放入等待区，都需要tomcat先接受该请求(即接受client的连接请求，建立socketchannel)，那么tomcat同时可建立的连接数( **maxConnections** 属性值)也会影响可同时处理的请求数。

### connectionTimeout
我们可把tomcat想象成一家医院，你来到医院大厅挂号看病，如果人家接受了，就相当于client和server建立socket连接了。接着你来到相应的科室，科室里每位医生都有一间诊室，这就相当于处理请求的线程；如果所有诊室都有病人，科室的调度护士会让你在科室小厅中耐心等待，直到他们通知你去几号诊室就诊；如果有空闲医生，你就可以立即就诊。

有的病人到医院很仓促，结果轮到他挂号或者就诊了，他还在包里翻找病例本和医保卡，如果超过了护士或医生心里可承受的等待时间，他们就会让病人到旁边找去，先服务下位。这种情形跟Connector元素的 **connectionTimeout** 属性所起的作用很相像。如果当前连接器(Connector)在接受连接后，等待了指定的时间但仍未接收到requestURI line，就会抛出超时异常。

### connector模式
tomcat的http connector有三种：bio、nio、apr。从上面的属性描述中可以看出对于不同的connector实现，相同的属性可能会有不同的默认值和不同的处理策略，所以在调整配置前，要先弄清楚各种实现之间的不同，以及当前部署容器使用的是哪种connector。

[Tomcat7 http connector 配置文档](http://tomcat.apache.org/tomcat-7.0-doc/config/http.html)

[修改Tomcat Connector运行模式，优化Tomcat运行性能 文档](http://www.365mini.com/page/tomcat-connector-mode.htm)

```txt
Tomcat 6.x版本从6.0.32开始就默认支持apr。
Tomcat 7.x版本从7.0.30开始就默认支持apr。
```
web server允许的最大连接数还受制于操作系统的内核参数设置，通常Windows是2000个左右，Linux是1000个左右。

### 如何加大Tomcat可使用的内存
tomcat默认可以使用的内存为128MB，在较大型的应用项目中，这点内存是不够的，需要调大。
Unix下，在文件{tomcat_home}/bin/catalina.sh的前面，增加如下设置：
```shell
JAVA_OPTS='-Xms【初始化内存大小】 -Xmx【可以使用的最大内存】'
```
需要把这个两个参数值调大。例如：
```shell
JAVA_OPTS='-Xms256m -Xmx512m'
```
表示初始化内存为256MB，可以使用的最大内存为512MB

### 其他因素
一台机器允许的连接数、线程数、内存大小，都会限制一台tomcat的实际并发数。

### 资料参考
1.[修改Tomcat Connector运行模式，优化Tomcat运行性能](http://www.365mini.com/page/tomcat-connector-mode.htm)

2.[对tomcat来说，每一个进来的请求(request)都需要一个线程，直到该请求结](http://www.cnblogs.com/softidea/p/5750791.html)