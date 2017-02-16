---
title: mac Alias命令
date: 2016-11-16 17:49:59
tags:
- Mac
- 终端
categories: 
- 技能树
---

# 设置.bash_profile
## 打开终端Terminal

## 输入命令cd ~ 切换到用户主目录

## 生成一个新文件(如果没有.bash_profile)
```bash
touch .bash_profile
```
## ## 使用喜欢的方式编辑.bash_profile文件(如果有)
```bash
open .bash_profile
```
也可以加-e参数，使用TextEdit打开文件
```bash
open -e .bash_profile
```
<!--more-->
## 更新内建命令
```bash
source .bash_profile 
```
# 在.bash_profile中增加命令别名
```bash
alias restart_network=/Users/mac/.command/RestartNetwork.sh
```

# git命令Alias
```bash
vi .gitconfig
```
或者
```bash
open .gitconfig
```
添加
```bash
[alias]
	co = checkout
	ci = commit
	st = status
	pl = pull
	ps = push
	dt = difftool
	l = log --stat
	cp = cherry-pick
	ca = commit -a
	b = branch
```