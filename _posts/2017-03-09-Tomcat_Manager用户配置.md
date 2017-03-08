---
layout: post
title: Tomcat Manager用户配置
date: 2017-03-09
tag: Tomcat
---

Tomcat Manager是Tomcat自带的、用于对Tomcat自身以及部署在Tomcat上的应用进行管理的web应用。Tomcat是Java领域使用最广泛的服务器之一，因此Tomcat Manager也成为了使用非常普遍的功能应用。

在默认情况下，Tomcat Manager是处于禁用状态的。准确地说，Tomcat Manager需要以用户角色进行登录并授权才能使用相应的功能，不过Tomcat并没有配置任何默认的用户，因此需要我们进行相应的用户配置之后才能使用Tomcat Manager。

Tomcat Manager的用户配置是在Tomcat安装目录```/conf/tomcat-users.xml```文件中进行管理的。

Tomcat Manager的用户配置非常简单，下面我们以一个具体的配置为例：
```xml
<tomcat-users>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
<user username="admin" password="123456" roles="manager-script"/>
</tomcat-users>
```

如上所示，我们只需要在`tomcat-users`节点中配置相应的`role`(角色/权限)和`user`(用户)即可。一个`user`节点表示单个用户，属性`username`和`password`分别表示登录的用户名和密码，属性`roles`表示该用户所具备的权限。

`user`节点的`roles`属性值与`role`节点的`rolename`属性值相对应，表示当前用户具备该`role`节点所表示的角色权限。当然，一个用户可以具备多种权限，因此属性`roles`的值可以是多个`rolename`，多个`rolename`之间以英文逗号隔开即可。

稍加思考，我们就应该猜测到，`rolename`的属性值并不是随意的内容，否则Tomcat怎么能够知道我们随便定义的`rolename`表示什么样的权限呢。实际上，Tomcat已经为我们定义了4种不同的角色——也就是4个`rolename`，我们只需要使用Tomcat为我们定义的这几种角色就足够满足我们的工作需要了。

以下是Tomcat Manager 4种角色的大致介绍(下面URL中的*为通配符)：

>- manager-gui
允许访问html接口(即URL路径为/manager/html/*)

>- manager-script
允许访问纯文本接口(即URL路径为/manager/text/*)

>- manager-jmx
允许访问JMX代理接口(即URL路径为/manager/jmxproxy/*)

>- manager-status
允许访问Tomcat只读状态页面(即URL路径为/manager/status/*)

从Tomcat Manager内部配置文件中可以得知，manager-gui、manager-script、manager-jmx均具备manager-status的权限，也就是说，manager-gui、manager-script、manager-jmx三种角色权限无需再额外添加manager-status权限，即可直接访问路径/manager/status/*。