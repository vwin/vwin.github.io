---
title: Linux安装Samba
toc: true
date: 2018-08-09 14:19:56
tags: [linux,samba]
categories: [工具]
description: Linux安装samba
---
linux 安装Samba，Mac远程连接Linux
<!--more-->
## 问题
需要在Mac上远程连接一台Linux服务器，管理一些文件。不仅需要进行常规的本地文件操作，还需要上传、下载、编辑。

## Samba简介
samba，是一个基于GPL协议的自由软件。它重新实现了SMB/CIFS协议，可以在各个平台共享文件和打印机。

1991年，还是大学生的Andrew Tridgwell，有三台机器，分别是Microsoft的DOS系统、DEC的Digital Unix系统、以及Sun的Unix系统。当时的技术无法让三者共享文件。为此，他开发了samba并将其开源。

本来改名为smbserver，但是一家商业公司注册了SMBServer商标。他被告知不能使用。于是执行了grep -i '^s.*m.*b' /usr/share/dict/words，从中选择了samba这个词。

## Samba安装

### 安装
以Ubuntu为例
```shell
sudo apt-get install samba
```

### 创建共享文件夹
也可以用现有的文件夹，chmod改变一下权限即可
```shell
mkdir /home/USER_NAME/shared_directory
sudo chmod 777 /home/USER_NAME/shared_directory
```

### 配置smb.conf
直接修改/etc/samba/smb.conf，在文件末尾添加：
```shell
[share]
      path = /home/USER_NAME/shared_directory
      available = yes
      browseable = yes
      public = yes
      writable = yes
```

### 添加samba账户
```shell
sudo touch /etc/samba/smbpasswd
sudo smbpasswd -a USER_NAME
```
USER_NAME就是你需要添加的用户名。然后会提示输入两次密码。

## Mac连接

1. 打开Finder（或在桌面），CMD + k，可以得到以下页面：
在`smb://`后面，输入你的服务器地址或域名
![](https://ws3.sinaimg.cn/large/0069RVTdly1fu3fjrdat5j30dg06daaj.jpg)
2. 输入前面设置的username 和 密码
![](https://ws1.sinaimg.cn/large/0069RVTdly1fu3fkaxk02j30bq05m0t1.jpg)

## .DS_Store安全隐患
由于Finder自带的.DS_Store包含了太多信息，如果在服务器产生.DS_Store会造成安全隐患。如果没有特殊配置，你用Finder管理远程的文件夹会自动产生.DS_Store。

在云端检查你的共享文件夹，如果发现.DS_Store，立即删除！

```shell
ls -a /home/USER_NAME/shared_directory
```
如何让Finder不在远程连接时产生.DS_Store？
打开Mac的Terminal，输入:
```shell
defaults write com.apple.desktopservices DSDontWriteNetworkStores true
```
然后重启Mac，再试试远程连接。
