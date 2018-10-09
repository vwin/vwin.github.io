---
title: Ubuntu 修改默认Python版本
toc: true
date: 2018-09-12 18:42:28
tags: [Ubuntu,python]
categories: [工具]
description:
---

修改Ubuntu中默认的Python版本
<!--more-->

## 查看系统中有哪些Python
```shell
$ ls /usr/bin/python*

/usr/bin/python            /usr/bin/python2-config  /usr/bin/python3m
/usr/bin/python2           /usr/bin/python3         /usr/bin/python-config
/usr/bin/python2.7         /usr/bin/python3.5
/usr/bin/python2.7-config  /usr/bin/python3.5m
```

## 查系统默认Python版本
```shell
$ python --version

Python 2.7.12
```

## 用户级修改
为某个特定用户修改Python版本，只需要在其home目录下创建一个alias
```shell
打开该用户的~/.bashrc文件
vim ~/.bashrc

添加新的别名来修改默认Python版本
alias python='/usr/bin/python3.5'

重新登录或者重新加载.bashrc文件，使操作生效
$ . ~/.bashrc 

检查当前的Python版本
$ python --version

Python 3.5.2
```

## 系统级修改

### 基于软链接
```shell
先删除默认的Python软链接
sudo rm /usr/bin/python

然后创建一个新的软链接指向需要的Python版本
sudo ln -s /usr/bin/python3.5 /usr/bin/python
```

### update-alternatives
可以使用update-alternatives来为整个系统更改Python版本

#### 列出替代版本
首先列出所有可用的python替代版本信息
```shell
$ update-alternatives --list python

update-alternatives: 错误: 无 python 的候选项
```
如果出现以上所示的错误信息，表示update-alternatives没有添加Python的替代版本

#### 添加替代版本
将Python的替代版本添加进去
```shell
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1

update-alternatives: 使用 /usr/bin/python2.7 来在自动模式中提供 /usr/bin/python (python) 

$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2

update-alternatives: 使用 /usr/bin/python3.5 来在自动模式中提供 /usr/bin/python (python)

```
--install选项使用了多个参数用于创建符号链接。最后一个参数指定了此选项的优先级，如果我们没有手动来设置替代选项，那么具有最高优先级的选项就会被选中

这个例子中，我们为/usr/bin/python3.5设置的优先级为2，所以update-alternatives命令会自动将它设置为默认Python版本

```shell
$ python --version

Python 3.5.2
```

再列出可用的Python替代版本
```shell
$ update-alternatives --list python

/usr/bin/python2.7
/usr/bin/python3.5
```
现在就可以在列出的Python替代版本中任意切换


```shell
$ update-alternatives --config python

有 2 个候选项可用于替换 python (提供 /usr/bin/python)。

  选择       路径              优先级  状态
------------------------------------------------------------
* 0            /usr/bin/python3.5   2         自动模式
  1            /usr/bin/python2.7   1         手动模式
  2            /usr/bin/python3.5   2         手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：1

```
然后查看版本号

```shell
$ python --version

Python 2.7.12
```

### 删除替代版本
当系统不再存在某个Python替代版本时，我们可以将其从update-alternatives列表中删除掉

例如，可以将列表中的python2.7版本移除
```shell
$ sudo update-alternatives --remove python /usr/bin/python2.7 

$ update-alternatives --list python

/usr/bin/python3.5
```

## 升级后的错误
pip错误
更改Python默认版本之后可能会出现如下错误
```shell
$ pip --version

Traceback (most recent call last):
  File "/usr/local/bin/pip", line 7, in <module>
    from pip import main
ImportError: No module named 'pip'
```
解决方法是将pip版本更改为符合当前的Python版本
```shell
对于Python3
sudo apt-get install python3-pip

对于Python2
sudo apt-get install python-pip

安装pip之后，可能版本不是最新的，需要更新
pip install --upgrade pip
```
