---
title: requestWindowFeature方法
date: 2016-06-09 22:54:51
tags: 
- Android
- View
categories: 移动开发
---

Android开发中经常会在setContentView(R.layout.XXX); 之前设置requestWindowFeature(XXXX)。
可以实现**软件全屏显示、自定义标题**（使用按钮等控件）和其他的需求
首先介绍一个重要方法那就是requestWindowFeature(featrueId),它的功能是启用窗体的扩展特性。参数是Window类中定义的常量。

# 枚举常量

1.DEFAULT_FEATURES：系统默认状态，一般不需要指定

2.FEATURE_CONTEXT_MENU：启用ContextMenu，默认该项已启用，一般无需指定

3.FEATURE_CUSTOM_TITLE：自定义标题。当需要自定义标题时必须指定。如：标题是一个按钮时

4.FEATURE_INDETERMINATE_PROGRESS：不确定的进度

5.FEATURE_LEFT_ICON：标题栏左侧的图标

6.FEATURE_NO_TITLE：没有标题

7.FEATURE_OPTIONS_PANEL：启用“选项面板”功能，默认已启用。

8.FEATURE_PROGRESS：进度指示器功能

9.FEATURE_RIGHT_ICON:标题栏右侧的图标
<!--more-->
# 详解

默认显示状态

## FEATURE_CUSTOM_TITLE详解

```java
this.requestWindowFeature(Window.FEATURE_CUSTOM_TITLE);  
setContentView(R.layout.main);  
```
## FEATURE_INDETERMINATE_PROGRESS详解

可以用来表示一个进程正在运行

```java
this.requestWindowFeature(Window.FEATURE_INDETERMINATE_PROGRESS);  
setContentView(R.layout.main);  
getWindow().setFeatureInt(Window.FEATURE_INDETERMINATE_PROGRESS, R.layout.progress);  
setProgressBarIndeterminateVisibility(true);  
```
## FEATURE_LEFT_ICON和FEATURE_RIGHT_ICON详解

```java
requestWindowFeature(Window.FEATURE_RIGHT_ICON);  
setContentView(R.layout.main);      
getWindow().setFeatureDrawableResource(Window.FEATURE_RIGHT_ICON,R.drawable.ic_launcher);  
```

```java
requestWindowFeature(Window.FEATURE_LEFT_ICON);  
setContentView(R.layout.main);          
getWindow().setFeatureDrawableResource(Window.FEATURE_LEFT_ICON,R.drawable.ic_launcher);  
```
## FEATURE_NO_TITLE详解

```java
this.requestWindowFeature(Window.FEATURE_NO_TITLE);  
setContentView(R.layout.main);  


getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);  
```