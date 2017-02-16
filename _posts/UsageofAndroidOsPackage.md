---
title: android.os包中一些类的使用
date: 2016-10-09 17:37:20
tags: 
- Android
categories: 
- 移动开发
---
# android.os.Build 
```java
Build.BOARD // 主板  
Build.BRAND // android系统定制商  
Build.CPU_ABI // cpu指令集  
Build.DEVICE // 设备参数  
Build.DISPLAY // 显示屏参数  
Build.FINGERPRINT // 硬件名称  
Build.HOST  
Build.ID // 修订版本列表  
Build.MANUFACTURER // 硬件制造商  
Build.MODEL // 版本  
Build.PRODUCT // 手机制造商  
Build.TAGS // 描述build的标签  
Build.TIME  
Build.TYPE // builder类型  
Build.USER 
```
# Build.VERSION 
<!--more-->
```java
// 当前开发代号  
Build.VERSION.CODENAME  
// 源码控制版本号  
Build.VERSION.INCREMENTAL  
// 版本字符串  
Build.VERSION.RELEASE  
// 版本号  
Build.VERSION.SDK  
// 版本号  
Build.VERSION.SDK_INT
```