---
title: Android自定义ToggleButton
date: 2016-04-03 22:06:07
tags:
- Android
- View
categories: 移动开发
---


## 准备资源

toggleButton是普通png格式的，我们可以使用工具将其改为9.png的,使用9patch工具时有个小技巧，那就是将show patches勾选上，方便我们画点。

## res下新建一个选择器

新建一个drawable目录，新建一个选择器:
toggle_selector.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" >
    <item android:state_checked="true" android:drawable="@drawable/toggle_button_on"></item>
    <item android:drawable="@drawable/toggle_button_off"></item>
</selector>
```
<!--more-->
## 使用控件
布局上设置togglebutton的背景
```xml
<ToggleButton 
    android:background="@drawable/toggle_selector"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textOn="***"
    android:textOff="***"
    android:checked="true"
    />
```