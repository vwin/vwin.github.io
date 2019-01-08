---
title: 重启php7-fpm和php5-fpm方法
toc: true
date: 2018-12-03 04:53:44
tags: [php7-fpm,php5-fpm,php]
categories: [PHP]
description:
---

各个平台上如何重启php7-fpm和php5-fpm
<!--more-->

PHP-FPM是一款简单好用的PHP FastCGI进程管理工具。
它可以和Apache、Nginx或其他服务器一起构建完整的PHP环境。
接下来我们看看在更改了php.ini 文件后，如何stop、restart或者reload PHP-FPM，以使修改生效。

# 修改php.ini或www.conf
修改php.ini文件：
```shell
$ php --ini                               # 确定php.ini文件的位置
$ sudo vi /etc/php.ini                    # 修改php.ini文件
```

修改php-fpm.conf文件：
```
$ sudo vi /etc/php-fpm/www.conf
```
编辑之后保存。

# CentOS/RHEL 7

```shell
$ sudo systemctl start php-fpm      # 启动php-fpm
$ sudo systemctl stop php-fpm       # 停止php-fpm
$ sudo systemctl reload php-fpm     # 重载php-fpm
$ sudo systemctl restart php-fpm    # 重启php-fpm
```

# CentOS/RHEL 6.x等旧版本

```shell
$ sudo service php-fpm start        # 启动php-fpm
$ sudo service php-fpm stop         # 停止php-fpm
$ sudo service php-fpm restart      # 重启php-fpm
$ sudo service php-fpm reload       # 重载php-fpm
```

# Ubuntu/Debian

```shell
$ sudo service php5-fpm start       # 启动
$ sudo service php5-fpm stop        # 停止
$ sudo service php5-fpm restart     # 重启
$ sudo service php5-fpm reload      # 重载
```

如果系统使用systemd，比如Ubuntu Linux 16.04+ LTS或者Debian Linux 8.x+，可以这样：

```shell
$ sudo systemctl start php5-fpm.service        # 启动
$ sudo systemctl stop php5-fpm.service         # 停止
$ sudo systemctl restart php5-fpm.service      # 重启
$ sudo systemctl reload php5-fpm.service       # 重载
```

# Ubuntu/Debian操作php7.0-fpm

```shell
$ sudo service php7.0-fpm start
$ sudo service php7.0-fpm stop
$ sudo service php7.0-fpm restart
$ sudo service php7.0-fpm reload
```

如果系统使用systemd，比如Ubuntu Linux 16.04+ LTS或者Debian Linux 8.x+，可以这样：

```shell
$ sudo systemctl start php7.0-fpm.service
$ sudo systemctl stop php7.0-fpm.service
$ sudo systemctl restart php7.0-fpm.service
$ sudo systemctl reload php7.0-fpm.service
```

# Alpine Linux

```shell
# /etc/init.d/php-fpm start
# /etc/init.d/php-fpm stop
# /etc/init.d/php-fpm restart
```

# FreeBSD Unix

```shell
# /usr/local/etc/rc.d/php-fpm start
# /usr/local/etc/rc.d/php-fpm stop
# /usr/local/etc/rc.d/php-fpm reload
# /usr/local/etc/rc.d/php-fpm restart
```

或者用service命令：

```shell
# service php-fpm start
# service php-fpm stop
# service php-fpm restart
# service php-fpm reload
```

[How to reload/restart php7.0-fpm / php5.0-fpm service](https://www.cyberciti.biz/faq/how-to-reload-restart-php7-0-fpm-service-linux-unix/)