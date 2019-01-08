---
title: Github ssh
toc: true
date: 2019-01-08 15:27:57
tags: [github,ssh]
categories: [工具]
description:
---
本地连接GitHub远程仓库，操作步骤
1. 本地生成ssh公钥
```shell
ssh-keygen
```
2. 添加本地公钥到github上
3. 本地执行
```shell
ssh-agent -s
ssh-add id_rsa文件路径
如果出现：Could not open a connection to your authentication agent. 那么执行
eval `ssh-agent -s`
然后再执行：ssh-add id_rsa文件路径
验证是否添加成功：
ssh -T git@github.com
or
ssh -T -v git@github.com
```