---
layout: post
title: Centos 7 安装&卸载 Mysql5.7
date: 2018-07-13
tag: Mysql
---

## 安装Mysql5.7

### 添加Mysql5.7 yum 源

```txt
wget http://repo.mysql.com/mysql57-community-release-el7-9.noarch.rpm
sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm
```

### 更新yum源

```txt
yum check-update
```

### 更新系统

```txt
yum update
```

### 安装Mysql

```txt
yum install mysql mysql-server
```

### 启动&关闭&查询 Mysql

```txt
systemctl start mysqld
systemctl stop mysqld
systemctl status mysqld
```

### 查看初始密码

```txt
grep 'temporary password' /var/log/mysqld.log
```

- 说明：如果无法查看初始密码，可进行以下操作进行密码重置

```txt
修改MySQL的登录设置： 
# vim /etc/my.cnf
在[mysqld]的段中加上一句：skip-grant-tables
例如：
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
skip-grant-tables
保存并且退出vi
```

```txt
重启MySQL
systemctl restart mysqld
```

```txt
登录并修改MySQL的root密码 
# mysql
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 3 to server version: 3.23.56
Type ‘help;’ or ‘\h’ for help. Type ‘\c’ to clear the buffer.
mysql> USE mysql ;
Database changed
mysql> UPDATE user SET authentication_string = password ( 'new-password' ) WHERE User = 'root' ;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 2 Changed: 0 Warnings: 0
mysql> flush privileges ;
Query OK, 0 rows affected (0.01 sec)
mysql> quit
```

```txt
将MySQL的登录设置修改回来 
# vim /etc/my.cnf
将刚才在[mysqld]的段中加上的skip-grant-tables删除
保存并且退出vim
```

```txt
重启MySQL
systemctl restart mysqld
```

### 修改编码

在 `/etc/my.cnf` 中设置默认的编码

```txt
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci #不区分大小写
collation-server =  utf8_bin #区分大小写
collation-server = utf8_unicode_ci #比 utf8_general_ci 更准确
```

### 创建数据库和用户

```txt
CREATE DATABASE <datebasename> CHARACTER SET utf8;
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
GRANT privileges ON databasename.tablename TO 'username'@'host';
SHOW GRANTS FOR 'username'@'host';
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
DROP USER 'username'@'host';
```

- `username`：你将创建的用户名
- host：指定该用户在哪个主机上可以登陆，如果是本地用户可用`localhost`，如果想让该用户可以从任意远程主机登陆，可以使用通配符 `%`
- `password`：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
- `privileges`：用户的操作权限，如 `ELECT，INSERT，UPDATE` 等，如果要授予所的权限则使用ALL
- `databasename`：数据库名
- `tablename`：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用 `*` 表示，如 `*.*`

## 卸载 Mysql 5.7

- 防止重装

```txt
查看yum是否安装过mysql
yum list installed mysql*
```

根据列表上的名字

```txt
yum remove mysql-community-client mysql-community-common mysql-community-libs mysql-community-libs-compat mysql-community-server mysql57-community-release
rm -rf /var/lib/mysql  
rm /etc/my.cnf
```

- rpm查看安装

```txt
rpm -qa | grep -i mysql
```

```txt
rpm -e mysql57-community-release-el7-9.noarch
rpm -e mysql-community-server-5.7.17-1.el7.x86_64
rpm -e mysql-community-libs-5.7.17-1.el7.x86_64
rpm -e mysql-community-libs-compat-5.7.17-1.el7.x86_64
rpm -e mysql-community-common-5.7.17-1.el7.x86_64
rpm -e mysql-community-client-5.7.17-1.el7.x86_64
cd /var/lib/  
rm -rf mysql/
```

- 清除余项

```txt
whereis mysql
mysql: /usr/bin/mysql /usr/lib64/mysql /usr/local/mysql /usr/share/mysql /usr/share/man/man1/mysql.1.gz
#删除上面的文件夹
rm -rf /usr/bin/mysql
```

-删除配置

```txt
rm –rf /usr/my.cnf
rm -rf /root/.mysql_sercret

chkconfig --list | grep -i mysql
chkconfig --del mysqld
```
