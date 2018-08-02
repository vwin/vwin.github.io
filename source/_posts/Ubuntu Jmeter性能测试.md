---
title: Ubuntu Jmeter性能测试
toc: true
date: 2018-08-01 15:22:52
tags: [ubuntu,jmeter,jdk,性能测试]
categories: [性能测试]
description: ubuntu安装jmeter进行性能测试
---
## Ubuntu安装JDK
1. 更新软件包列表
```shell
sudo apt-get update
```
2. 安装openjdk-8-jdk
```shell
sudo apt-get install openjdk-8-jdk
```
3. 查看Java版本
```shell
java -version
```
## Mac安装Java
### JRE和JDK的区别
- JRE： Java Runtime Environment
- JDK：Java Development Kit 
- JRE顾名思义是java运行时环境，包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的。
- JDK顾名思义是java开发工具包，是程序员使用java语言编写java程序所需的开发工具包，是提供给程序员使用的。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。所以JDK的包比JRE大很多。
- 如果你需要运行java程序，只需安装JRE就可以了
- 如果你需要编写java程序，需要安装JDK。
- JRE根据不同操作系统（如：windows，linux等）和不同JRE提供商（IBM,ORACLE等）有很多版本，最常用的是Oracle公司收购SUN公司的JRE版本。

### 安装JRE/JDK
- JRE下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html
- JDK下载地址：
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
Mac下载对应的包，下载完成后直接安装即可

### 环境变量配置
1. JRE:如果安装的是JRE，那么在Mac上默认的路径是
```
/Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
```
- 配置环境变量（vi ~/.bash_profile）：
```
JAVA_HOME=/Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME
export PATH
```
2. JDK:如果安装的是JDK，那么在Mac上默认的路径是
```
/Library/Java/JavaVirtualMachines/
```
- 配置环境变量（vi ~/.bash_profile）：
```
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
PATH=$JAVA_HOME/bin:$PATH:.
CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export JAVA_HOME
export PATH
export CLASSPATH
```
## 安装Jmeter
1. 下载Jmeter4.0
```shell
wget -c http://www-us.apache.org/dist//jmeter/binaries/apache-jmeter-4.0.tgz
```
2. 解压
```shell
tar -xf apache-jmeter-4.0.tgz
```
3. 运行sample
```shell
apache-jmeter-4.0/bin/./jmeter -n -t apache-jmeter-4.0/extras/Test.jmx
```
## Jmeter基础

### Jmeter环境变量配置
- MAC版本,具体路径请根据自己本地路径进行替换
```shell
export JMETER_HOME=/Users/xxx/apache-jmeter-4.0
export CLASSPATH=.:$JMETER_HOME/lib/ext/ApacheJMeter_core.jar:$JMETER_HOME/lib/jorphan.jar
export PATH=$JMETER_HOME/bin:$PATH
```