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

### 测试计划
Test Plan (测试计划)：用来描述一个性能测试，包含与本次性能测试所有相关的功能。也就说本的性能测试的所有内容是于基于一个计划的。下面看一下一个计划下面都有哪些主要的功能模块（右键单击“测试计划”弹出菜单）。
![测试计划](https://ws2.sinaimg.cn/large/0069RVTdly1fu1dlm6u0wj30w00k00ve.jpg)
虽然有三个添加线程组的选项，名字不一样， 创建之后，其界面是完全一样的。之前的版本只有一个线程组的名字。现在多一个setUp theread Group 与terDown Thread Group 
1. setup thread group  
一种特殊类型的ThreadGroup的，可用于执行预测试操作。这些线程的行为完全像一个正常的线程组元件。不同的是，这些类型的线程执行测试前进行定期线程组的执行。
2. teardown thread group.  
一种特殊类型的ThreadGroup的，可用于执行测试后动作。这些线程的行为完全像一个正常的线程组元件。不同的是，这些类型的线程执行测试结束后执行定期的线程组。
3. thread group(线程组).
这个就是我们通常添加运行的线程。通俗的讲一个线程组,，可以看做一个虚拟用户组，线程组中的每个线程都可以理解为一个虚拟用户。线程组中包含的线程数量在测试执行过程中是不会发生改变的。

### 测试片段
测试片段元素是控制器上的一个种特殊的线程组，它在测试树上与线程组处于一个层级。它与线程组有所不同，因为它不被执行，除非它是一个模块控制器或者是被控制器所引用时才会被执行。
![测试片段](https://ws4.sinaimg.cn/large/0069RVTdly1fu1dp4x6jjj30hb0a0755.jpg)

### 控制器
JMeter有两种类型的控制器：取样器（sample）和逻辑控制器（Logic Controller），用这些原件来驱动处理一个测试。
#### 采样器
取样器（Sample）是性能测试中向服务器发送请求，记录响应信息，记录响应时间的最小单元，JMeter 原生支持多种不同的sampler ，如 HTTP Request Sampler 、 FTP  Request Sample 、TCP  Request Sample 、JDBC Request Sampler 等，每一种不同类型的 sampler 可以根据设置的参数向服务器发出不同类型的请求。
![采样器](https://ws2.sinaimg.cn/large/0069RVTdly1fu1ds3unxyj30lh0fnacb.jpg)

#### 逻辑控制器（Logic Controller）
逻辑控制器，包括两类无件，一类是用于控制test plan 中 sampler 节点发送请求的逻辑顺序的控制器，常用的有 如果（If）控制器 、switch Controller 、Runtime Controller、循环控制器等。另一类是用来组织可控制 sampler 来节点的，如 事务控制器、吞吐量控制器。
![逻辑控制器](https://ws2.sinaimg.cn/large/0069RVTdly1fu1dukjfaqj30w00k0dir.jpg)

### 配置元件
配置元件（config element）用于提供对静态数据配置的支持。CSV Data Set config 可以将本地数据文件形成数据池（Data Pool），而对应于HTTP Request Sampler和 TCP Request Sampler等类型的配制无件则可以修改Sampler的默认数据。（例如，HTTP Cookie Manager 可以用于对 HTTP Request Sampler 的cookie 进行管理）
![配置元件](https://ws4.sinaimg.cn/large/0069RVTdly1fu1dvqv6w8j30w00k041t.jpg)

### 定时器
定时器（Timer）用于操作之间设置等待时间，等待时间是性能测试中常用的控制客户端QPS的手端。类似于LoadRunner里面的“思考时间”。JMeter 定义了Bean Shell Timer、Constant Throughput Timer、固定定时器等不同类型的Timer。
![定时器](https://ws2.sinaimg.cn/large/0069RVTdly1fu1dwdrmb8j30w00k0n03.jpg)

### 前置处理器（Per Processors）
用于在实际的请求发出之前对即将发出的请求进行特殊处理。例如，HTTP URL重写修复符则可以实现URL重写，当RUL中有sessionID 一类的session信息时，可以通过该处理器填充发出请求的实际的sessionID 。
![前置处理器](https://ws3.sinaimg.cn/large/0069RVTdly1fu1dxfqrxxj30w00k0ju8.jpg)

### 后置处理器（Post Processors）
用于对Sampler 发出请求后得到的服务器响应进行处理。一般用来提取响应中的特定数据（类似LoadRunner测试工具中的关联概念）。例如，XPath  Extractor 则可以用于提取响应数据中通过给定XPath 值获得的数据。
![后置处理器](https://ws3.sinaimg.cn/large/0069RVTdly1fu1dy72ixkj30w00k0q5x.jpg)

### 断言（Assertions）
 断言用于检查测试中得到的相应数据等是否符合预期，断言一般用来设置检查点，用以保证性能测试过程中的数据交互是否与预期一致。
 ![断言](https://ws1.sinaimg.cn/large/0069RVTdly1fu1dywlw7ej30w00k0q5w.jpg)

### 监听器（Listener）
这个监听器可不是用来监听系统资源的元件。它是用来对测试结果数据进行处理和可视化展示的一系列元件。 图行结果、查看结果树、聚合报告。都是我们经常用到的元件。

![监听器](https://ws4.sinaimg.cn/large/0069RVTdly1fu1dzp8i85j30w00k0jul.jpg)