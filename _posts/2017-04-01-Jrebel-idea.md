---
layout: post
title: Jrebel破解 Intellij idea
date: 2017-04-01
tag: Tools
---

### 下载Jrebel插件

以Intellij Idea 2017最新版为例子演示:

进入`Intellij Idea` - > `Preferences` - > `Plugins` - > `Browse Repositories` - > 搜索 `jrebel`, 下载当前最新版本,比如 7.0 版本,重启机器即可

### 文件替换

点击链接下载破解文件:

[jrebel破解文件](/images/jrebel/jrebel.tar.gz)

```shell
#解压
tar zxvf jrebel.tar.gz
```

**1.插件替换**

```shell
rm -r /Users/pnoker/Library/Application Support/IntelliJIdea2017.1/jr-ide-idea/lib/*
cp /Users/pnoker/Downloads/jrebel/*  /Users/pnoker/Library/Application Support/IntelliJIdea2017.1/jr-ide-idea/lib/
```

**2.授权文件替换**

```shell
cd /Users/pnoker/.jrebel
cp /Users/pnoker/Downloads/jrebel/jrebel.lic ./
```

### 重启

![](/images/jrebel/jrebel.png)

### 小结

`pnoker` 为当前系统用户名,视实际情况而定.

