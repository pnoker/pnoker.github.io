---
layout: post
title: Socket详解(一):Java Socket简介
date: 2018-06-05
tag: Java
---

### 概念

`网络进程间通讯` 两个进程唯一确定，网络层的 `Ip` 可以唯一标识网络主机，传输层的 `Ip+Port` 可以唯一确定网络间两进程。

标识网络两进程后，进行通讯一般是利用socket。

socket源于unix，延续了 “一切皆文件” 的概念，都使用 “打开->读写->关闭” 文件操作的方式。

### 通讯过程

Server ： `socket`->`bind`->`listen`->`accept`->`receive`->`close`

flow
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: get_hotel_ids|past
op2=>operation: get_proxy|current
sub1=>subroutine: get_proxy|current
op3=>operation: save_comment|current
op4=>operation: set_sentiment|current
op5=>operation: set_record|current
cond1=>condition: ids_remain空?
cond2=>condition: proxy_list空?
cond3=>condition: ids_got空?
cond4=>condition: 爬取成功??
cond5=>condition: ids_remain空?
io1=>inputoutput: ids-remain
io2=>inputoutput: proxy_list
io3=>inputoutput: ids-got
st->op1(right)->io1->cond1
cond1(yes)->sub1->io2->cond2
cond2(no)->op3
cond2(yes)->sub1
cond1(no)->op3->cond4
cond4(yes)->io3->cond3
cond4(no)->io1
cond3(no)->op4
cond3(yes, right)->cond5
cond5(yes)->op5
cond5(no)->cond3
op5->e


Client ： `socket`->`connect`->`send`->`close`



server端需要获取ip和绑定端口号，让client调用，所以需要bind和listern

client的端口号在connect的时候绑定端口号
