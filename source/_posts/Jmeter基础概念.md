---
title: Jmeter基础--概念
toc: true
date: 2018-08-07 15:53:24
tags: [jmeter]
categories: [性能测试]
description:
---
Jmeter基础概念
<!-- more -->
## Jmeter介绍
一个优秀的开源性能测试工具
官网地址：http://jmeter.apache.org/download_jmeter.cgi 

## Jmeter包含组件

Jmeter工具和其他性能工具在原理上完全一致，工具包含4个部分：

（1）负载发生器：用于产生负载，通常以多线程或是多进程的方式模拟用户行为。

（2）用户运行器：通常是一个脚本运行引擎，用户运行器附加在线程或进程上，根据脚本要求模拟指定的用户行为。

（3）资源生成器：用于生成测试过程中服务器、负载机的资源数据。

（4）报表生成器：根据测试中霍地的数据生成报表，提供可视化的数据显示方式。

### Jmeter设置中文
1. jmeter.properties中配置Jmeter界面语言
/apache-jmeter-4.0/bin/jmeter.properties中language=en默认屏蔽，取消屏蔽后显示英文界面，language=zh强制显示简体中文界面。
2. GUI界面配置：
![Jmeter语言选择](https://github.com/vwin/markdownpic/raw/master/hexo-blog/jmeter-lang.png)

### Jmeter测试计划元件
Test Plan (测试计划)：用来描述一个性能测试，包含与本次性能测试所有相关的功能。也就说本的性能测试的所有内容是于基于一个计划的。下面看一下一个计划下面都有哪些主要的功能模块（右键单击“测试计划”弹出菜单）。

