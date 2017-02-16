---
title: Android ANR trace
date: 2016-12-21 20:27:52
tags: 
- Android
- ANR
categories: 移动开发
---

## ANR
### 解释
ANR:Application Not Responding
### 三种类型
** KeyDispatchTimeout(5 seconds) --主要类型
按键或触摸事件在特定时间内无响应
** BroadcastTimeout(10 seconds)
BroadcastReceiver在特定时间内无法处理完成
** ServiceTimeout(20 seconds) --小概率类型
Service在特定的时间内无法处理完成

### 利用adb获取traces文件
```bash
adb pull /data/anr/traces.txt 文件目录：/文件名
```

### 分析方法
ANR问题，直接搜索“ANR ”关键词（ANR后面加个空格，可以过滤很多无效信息），快速定位到关键事件信息；
定位到关键事件信息后，如果信息不够明确的，再去搜索虚拟机信息，关键字：“Dalvik Thread”，查看具体的进程和线程跟踪的日志，来定位到代码。但有时log中没有虚拟机的信息；
接下来就是分析traces.txt，都是虚拟机线程信息，我们可以从中找到些关键信息；
ANR出现不一定是应用本身的问题，如果机器由于某个原因就已经很慢了，然后打开某个应用，之后该应用报个ANR，这个实属正常；