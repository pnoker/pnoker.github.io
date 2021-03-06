---
layout: post
title: MySQL中int、bigint、smallint和tingint的区别
date: 2017-03-16
tag: MySQL
---

MySQL中多种**数字**类型，其中主要有`int`、`bigint`、`smallint`和`tingint`。

### bigint
从 -2^63 (-9223372036854775808) 到 2^63-1 (9223372036854775807) 的整型数据（所有数字）。存储大小为 8 个字节。 
P.S. bigint已经有长度了，在mysql建表中的length，只是用于显示的位数 

### int
从 -2^31 (-2,147,483,648) 到 2^31 – 1 (2,147,483,647) 的整型数据（所有数字）。存储大小为 4 个字节。int 的 SQL-92 同义字为 integer。 
smallint 
从 -2^15 (-32,768) 到 2^15 – 1 (32,767) 的整型数据。存储大小为 2 个字节。 

### tinyint
从 0 到 255 的整型数据。存储大小为 1 字节。 

### 注释 
```txt
在支持整数值的地方支持 bigint 数据类型。
但是，bigint 用于某些特殊的情况，当整数值超过 int 数据类型支持的范围时，就可以采用 bigint。
在 SQL Server 中，int 数据类型是主要的整数数据类型。 
在数据类型优先次序表中，bigint 位于 smallmoney 和 int 之间。 
只有当参数表达式是 bigint 数据类型时，函数才返回 bigint。
SQL Server 不会自动将其它整数数据类型（tinyint、smallint 和 int）提升为 bigint。 
int(M) 在 integer 数据类型中，M 表示最大显示宽度。
在 int(M) 中，M 的值跟 int(M) 所占多少存储空间并无任何关系。
和数字位数也无关系 int(3)、int(4)、int(8) 在磁盘上都是占用 4 btyes 的存储空间。
```