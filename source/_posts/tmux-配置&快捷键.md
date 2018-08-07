---
title: tmux 配置&快捷键
toc: true
date: 2018-07-17 10:52:52
tags: [tmux]
categories: [tool]
description: tmux 配置整理
---
tmux基本配置&快捷键

## Tmux基本配置

```python
set -g prefix C-a #
unbind C-b # C-b即Ctrl+b键，unbind意味着解除绑定
bind C-a send-prefix # 绑定Ctrl+a为新的指令前缀

# 从tmux v1.6版起，支持设置第二个指令前缀
set-option -g prefix2 ` # 设置一个不常用的`键作为指令前缀，按键更快些
set-option -g mouse on # 等同于以上4个指令的效果
unbind '"'
bind - splitw -v -c '#{pane_current_path}' # 垂直方向新增面板，默认进入当前目录
unbind %
bind | splitw -h -c '#{pane_current_path}' # 水平方向新增面板，默认进入当前目录
# 绑定快捷键为r,读取配置文件
bind r source-file ~/.tmux.conf \; display-message "Config reloaded.."
```

## Tmux快捷键
启动新会话：
```shell
tmux [new -s 会话名 -n 窗口名]
```

恢复会话：
```shell
tmux at [-t 会话名]
```

列出所有会话：
```shell
tmux ls
```

关闭会话：
```shell
tmux kill-session -t 会话名
```

关闭所有会话：
```shell
tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill
```