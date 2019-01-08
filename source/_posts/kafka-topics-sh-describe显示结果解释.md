---
title: kafka-topics.sh --describe显示结果解释
toc: true
date: 2018-10-23 04:23:29
tags: [kafka-topic.sh,describe]
categories: [Kafka]
description:
---
Kafka-topics.sh --describe 显示结果解释

<!--more-->
```shell
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic
Topic:my-replicated-topic	PartitionCount:1	ReplicationFactor:3	Configs:
	Topic: my-replicated-topic	Partition: 0	Leader: 1	Replicas: 1,2,0	Isr: 1,2,0
```

第一个行显示所有partitions的一个总结，以下每一行给出一个partition中的信息，如果我们只有一个partition，则只显示一行。

leader：是在给出的所有partitons中负责读写的节点，每个节点都有可能成为leader
replicas：显示给定partiton所有副本所存储节点的节点列表，不管该节点是否是leader或者是否存活。
isr：副本都已同步的的节点集合，这个集合中的所有节点都是存活状态，并且跟leader同步

```shell
[root@dx3 bin]# ./kafka-topics.sh --describe --zookeeper localhost:2183 --topic test
Topic:test    PartitionCount:3    ReplicationFactor:3    Configs:
    Topic: test    Partition: 0    Leader: 0    Replicas: 0,1,2    Isr: 0,2,1
    Topic: test    Partition: 1    Leader: 1    Replicas: 1,2,0    Isr: 1,2,0
    Topic: test    Partition: 2    Leader: 2    Replicas: 2,0,1    Isr: 2,0,1
```

解释：
测试Kafka集群一共三个节点，test这个Topic, 编号为0的Partition,Leader在broker.id=0这个节点上，副本在broker.id为0 1 2这个三个几点，并且所有副本都存活，并跟broker.id=0这个节点同步
