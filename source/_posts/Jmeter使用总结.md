---
title: Jmeter使用总结
toc: true
date: 2018-08-10 18:05:09
tags: [jmeter]
categories: [性能测试]
description: 
---
Jmeter使用总结
<!--more-->

## 多个线程组顺序执行和并行执行
在测试计划的设置中可以设置
![](https://ws2.sinaimg.cn/large/006tNbRwly1fu4rlcw08sj30nn0flab8.jpg)
1. 勾选 Run Thread Groups consecutively(i.e.one at time)，则表示顺序执行。顺序执行，指的是测试计划中存在多个线程组时，第一个线程组执行完后再执行下一个线程组。

2. 不勾选 Run Thread Groups consecutively(i.e.one at time)，则表示并行执行。并行执行，指的是指的是测试计划中存在多个线程组时，所有线程组都在同一时刻执行

