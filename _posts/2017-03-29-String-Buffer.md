---
layout: post
title: String、StringBuffer与StringBuilder之间区别
date: 2017-03-29
tag: Java
---

### 速度

`StringBuilder` >  `StringBuffer`  >  `String`

### String

字符串常亮

String类型是不可变的对象,当我们操作String时,其实是不断创建新的String对象,而原先的则当做垃圾被GC回收了,所以速度可想而知的慢.

### StringBuffer

字符串创变量,线程安全

StringBuffer是字符串变量,是可以改变的对象,每当我们用它对字符串进行操作时,实际上是对上一个对象进行操作,而不像String那样创建一些额外的对象,因此速度很快.

### StringBuilder

字符串创变量,线程不安全

StringBuilder和StringBuffer一样,只是一个是线程安全一个不安全,但StringBuilder的速度比StringBuffer快,在单线程中可以选择使用StringBuilder.

### 小结

1.如果要操作少量的数据用 = String

2.单线程操作字符串缓冲区 下操作大量数据 = StringBuilder

3.多线程操作字符串缓冲区 下操作大量数据 = StringBuffer
