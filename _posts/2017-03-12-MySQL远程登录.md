---
layout: post
title: MySQL远程登录
date: 2017-03-12
tag: MySQL
---

### 安装MySQL

在Ubuntu上安装mysql5.6为例

```shell
sudo apt-get install mysql-client-5.6
sudo apt-get install musql-server-5.6
```
安装过程中会提示输入密码，请务必记住密码，比如本人密码都是123456，你也可以根据个人喜好进行设置密码。

### 查看&启动&关闭&重启MySQL服务

查看MySQL服务

```shell
ps -ef | grep mysql

##################

mysql    12939     1  0 09:01 ?        00:00:00 /usr/sbin/mysqld
intel    13522  5679  0 09:09 pts/0    00:00:00 grep --color=auto mysql
```

启动MySQL服务

```shell
sudo service mysql start
```

关闭MySQL服务

```shell
sudo service mysql stop
```

重启MySQL服务

```shell
sudo service mysql restart
```

### 本地登录MySQL

前提是确保MySQL服务正常启动了

```shell
sudo mysql -u root -p

#提示输入密码，密码为安装时填写的密码，我的是123456
```

### 基本操作命令

查询数据库（Databases）

```shell
mysql> show databases;
```

查询有表（Tables）

```shell
mysql> show tables;
```

选择表

```shell
mysql> use table;
```

### 新建一个能远程登录的用户

```shell
mysql> use user;
mysql> insert into user values('%','username',password('password'), 'y','y','y','y','y','y',
'y','y','y','y','y','y','y','y')
mysql> grant all privileges on *.* to username@% identified  with grant option;
mysql> flush privileges;
```

### 开启3306端口

查看端口是否打开

```shell
netstat -an|grep 3306
#####################
tcp6       0      0 :::3306                 :::*                    LISTEN     
tcp6       0      0 172.16.1.23:3306        192.168.158.10:59251    ESTABLISHED
tcp6       0      0 172.16.1.23:3306        192.168.158.10:59250    ESTABLISHED
```

修改MySQL配置文件，以MySQL-5.6为例

```shell
sudo vim /etc/mysql/my.cnf
```

注释掉bind-address

```shell
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
#bind-address		= 127.0.0.1
#
# * Fine Tuning
```

重启MySQL服务

```shell
sudo service mysql restart
```

现在使用MySQL WorkBench远程登录试一试，OK!

