---
title: Ubuntu 16.04安装mysql
toc: true
date: 2018-12-12 02:48:25
tags: [ubuntu,mysql]
categories: [Mysql]
description:
---

Ubuntu 16.04安装MySQL

<!--more-->
# 安装
```shell
sudo apt-get install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```

# 测试
```shell
sudo netstat -tap | grep mysql
```
出现 ![](https://ws2.sinaimg.cn/large/006tNbRwly1fy40h11flzj30j8023wez.jpg) 则表示安装成功

# 进入
```shell
mysql -uroot -p你的密码
```

# 运行远程访问
```shell
编辑mysqld.cnf文件
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
注释掉：bind-address:127.0.0.1
![](https://ws1.sinaimg.cn/large/006tNbRwly1fy40iks7b6j30kf0blmyw.jpg)

```shell
mysql -uroot -p
grant all on *.* to root@'%' identified by '你的密码' with grant option;
flush privileges;
```
然后执行quit命令退出mysql服务，执行如下命令重启mysql：
```shell
sudo service mysql restart
```
