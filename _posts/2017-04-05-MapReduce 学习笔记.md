---
layout: post
title: MapReduce 学习笔记
date: 2017-04-05
tag: Hadoop
---

### MapReduce理解

MapReduce是一种编程模型，用于大规模数据集的分布式运算。

**理解例子：**

图书馆要清点图书数量，有10个书架，管理员为了加快统计速度，找来了10个同学，每个同学负责统计一个书架的图书数量。

张同学统计 书架1
王同学统计 书架2
刘同学统计 书架3
……

过了一会儿，10个同学陆续到管理员这汇报自己的统计数字，管理员把各个数字加起来，就得到了图书总数。

这个过程就可以理解为MapReduce的工作过程。


### Map

管理员分配哪个同学统计哪个书架，每个同学都进行相同的“统计”操作，这个过程就是map。

### Reduce

每个同学的结果进行汇总，这个过程是reduce。

### MapReduce 流程梳理

#### MapReduce工作流程大概如下：

- （1）maptask从目标文件中读取数据
- （2）mapper的map方法处理每一条数据，输出到文件中
- （3）reducer读取map的结果文件，进行分组，把每一组交给reduce方法进行处理，最后输出到指定路径。

![](/images/hadoop/mapreduce/001.jpg)

#### Maptask中的处理流程

![](/images/hadoop/mapreduce/002.jpg)

目标文件会被按照规划文件进行切分，inputformat调用RecordReader读取文件切片，RecordReader会生成key value对儿，传递给Mapper的mao方法。

从Mapper的map方法调用context.write之后，到形成结果数据文件这个过程是比较复杂的。

![](/images/hadoop/mapreduce/003.jpg)

context.write不是直接写入文件，而是把数据交给OutputCollector，OutputCollector把数据写入‘环形缓冲区’。‘环形缓冲区’中的数据会进行排序。

因为缓冲区的大小是有限制的，所以每当快满时（达到80%）就要把其中的数据写出去，这个过程叫做数据溢出。

溢出到一个文件中，溢出过程会对这批数据进行分组、比较操作，然后吸入文件，所以溢出文件中的数据是分好区的，并且是有序的。每次溢出都会产生一个溢出数据文件，所以会有多个。

当map处理完全数据后，就会对各个溢出数据文件进行合并，每个文件中相同区的数据放在一起，并再次排序，最后得到一个整体的结果文件，其中是分区且有序的。

这样就完成了map过程，读数据过程和写结果文件的过程联合起来如下图：

![](/images/hadoop/mapreduce/004.jpg)

#### Reducetask的处理流程

![](/images/hadoop/mapreduce/005.jpg)

reducetask去读每个maptask产生的结果文件中自己所负责的分区数据，读到自己本地。对多个数据文件进行合并排序，然后通过GroupingComparator进行分组，把相同key的数据放到一组。对每组数据调一次reduce方法，处理完成后写入目标路径文件。

![](/images/hadoop/mapreduce/006.jpg)