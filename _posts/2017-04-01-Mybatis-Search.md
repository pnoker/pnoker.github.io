---
layout: post
title: Mybatis组合查询小结
date: 2017-04-01
tag: Mybatis
---

### 前言

在使用Mybatis生成的Example进行查询时可以发现Critical类只提供了andXXXExample的方法，而在Example中却提供了`or`方法，因此可以使用`or`进行简单的组合查询了。

### 查询条件

`a = ? and ( b = ? or c = ? )`    不支持

`( a = ? and b = ? ) or ( c = ? and d = ? )`    支持

`( a= ? ) or ( b = ? )`    支持

### 测试

方法一：

```java
UserExample example = new UserExample(start, limit);
UserExample.Criteria criteria1 = example.createCriteria();
criteria1.andNameLike("%" + content + "%");
UserExample.Criteria criteria2 = example.createCriteria();
criteria2.andUsernameLike("%" + content + "%");
example.or(criteria2);
List<User> list = userService.countByExample(example);
```

生成的SQL语句为：

```sql
select count(*) from user WHERE ( name like ? ) or( username like ? )
```

方法二:

```java
UserExample example = new UserExample(start, limit);
example.or().andNameLike("%" + content + "%");
example.or().andUsernameLike("%" + content + "%");
List<User> list = userService.countByExample(example);
```

生成的SQL语句为：

```sql
select count(*) from user WHERE ( name like ? ) or( username like ? )
```