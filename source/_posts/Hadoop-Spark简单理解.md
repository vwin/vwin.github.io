---
title: Hadoop Spark简单理解
toc: true
date: 2018-12-24 01:19:17
tags: [大数据,hadoop,spark]
categories: [大数据]
description:
---
hadoop和Spark是两种不同的大数据处理框架，他们的组件都非常多。
下面是两种框架使用到的一些组件整理
![](https://ws4.sinaimg.cn/large/006tNbRwly1fyhtbtncdrj30k00ctq5z.jpg)
蓝色部分，是Hadoop生态系统组件，黄色部分是Spark生态组件。
虽然他们是两种不同的大数据处理框架，但它们不是互斥的，Spark与hadoop 中的MapReduce是一种相互共生的关系。
Hadoop提供了Spark许多没有的功能，比如分布式文件系统，而Spark 提供了实时内存计算，速度非常快。有一点大家要注意，Spark并不是一定要依附于Hadoop才能生存，除了Hadoop的HDFS，还可以基于其他的云平台，当然啦，大家一致认为Spark与Hadoop配合默契最好罢了。
<!--more-->
- Hadoop包括HDFS、Mapreduce、YARN三大核心组件，Spark主要解决计算问题，也就是主要用来替代Mapreduce的功能，底层存储和资源调度仍然使用HDFS、YARN来承载.
- <font color=red><b>Hadoop = HDFS + YARN + MapReduce</b></font>
- HDFS负责存储，已然成为业内的分布式存储的标配，算是行业标准了。
- YARN负责资源调度，依然发挥重要作用，不可获取的重要组件之一，Spark也可以跑在它上面，相比Mesos（C++），YARN是Hadoop自带的，用起来比较方便。
- MapReduce计算框架，在Spark面前已失去性能及速度优势，基本面临淘汰。



参考：
- https://www.zhihu.com/question/26568496/answer/41608400