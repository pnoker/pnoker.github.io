---
layout: post
title: Http请求中的Get和Post浅谈
date: 2016-12-22
tag: Hadoop
---

### 前言

Http定义了与服务器交互的不同方法，最基本的有4中，分别是Put、Delete、Post、Get。
URL全称是资源描述符，我们可以这样认为：一个URL地址，它用于描述一个网络上的资源，而HTTP中的Put、Delete、Post、Get就对应着对这个资源的增、删、改、查4个操作。其中，GET一般用于获取/查询资源信息，而POST一般用于更新资源信息。

### GET

Http规范约定，Get用于信息的获取，而且是安全的和幂等的。


### POST

Http规范约定，Post表示可能修改服务器上的资源请求。

### 总结

1. get是从服务器上获取数据，post是向服务器传送数据。

2. get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。

3. 对于get方式，服务器端用Request.QueryString获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据。

4. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，IIS4中最大量为80KB，IIS5中为100KB。

5. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。

6. get方式的安全性较Post方式要差些，包含机密信息的话，建议用Post数据提交方式。

7. 在做数据查询时，建议用Get方式；而在做数据添加、修改或删除时，建议用Post方式



