---
title: Python 升级所有包
toc: true
date: 2018-10-09 10:45:08
tags: [python,pip]
categories: [Python]
description: Python 升级所有包
---

Python升级所有已安装的包
<!--more-->

pip 当前内建命令并不支持升级所有已安装的Python模块。

列出当前安装的包：

```shell
pip list
```

列出可升级的包：

```shell
pip list --outdate
```

升级一个包：
```shell
pip install --upgrade requests  // mac,linux,unix 在命令前加 sudo -H
```

升级所有可升级的包：

```shell
pip freeze --local | grep -v '^-e' | cut -d = -f 1  | xargs -n1 pip install -U
```

```shell
for i in `pip list -o --format legacy|awk '{print $1}'` ; do pip install --upgrade $i; done
```

pip默认源由于墙，所以速度很慢，可使用第三源提高速度：

```shell
vim ~/.pip/pip.conf
```

```shell
[global]
trusted-host = mirrors.aliyun.com
index-url = http://mirrors.aliyun.com/pypi/simple
```
