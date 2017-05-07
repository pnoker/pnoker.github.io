---
layout: post
title: 设置MySQL中时间戳insert和update自动更新
date: 2017-03-17
tag: MySQL
---

设置时间的数据类型为：`TIMESTAMP`,然后设置他的默认值为：`CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`