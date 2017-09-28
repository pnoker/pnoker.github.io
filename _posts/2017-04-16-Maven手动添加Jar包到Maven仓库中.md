---
layout: post
title: Maven手动添加Jar包到Maven仓库中
date: 2017-04-16
tag: Linux
---

### 安装命令

```bash
mvn install:install-file -Dfile=jar包的位置 -DgroupId=上面的groupId -DartifactId=上面的artifactId -Dversion=上面的version -Dpackaging=jar

----
-Dfile : jar包位置
-DgroupId : groupId号
-DartifactId : artifactId号
-Dversion : 版本号
-Dpackaging : 包类型，填jar 

```

### 例如

我下载的这个 jar 包是放到了 D:\mvn 目录下(D:\mvn\spring-context-support-3.1.0.RELEASE.jar)
那么我在 cmd 中敲入的命令就应该是：

```bash
mvn install:install-file -Dfile=D:\mvn\spring-context-support-3.1.0.RELEASE.jar -DgroupId=org.springframework -DartifactId=spring-context-support -Dversion=3.1.0.RELEASE -Dpackaging=jar
```



