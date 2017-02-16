---
title: Android自定义渐变背景
date: 2016-04-04 11:41:00
tags:
- Android
---


## 工程目录->res->drawable->新建*.xml文件如下：

```xml

<?xml version="1.0" encoding="utf-8"?>  
<shape xmlns:android="http://schemas.android.com/apk/res/android"  
    android:shape="rectangle">  
    <!--颜色和方向-->
  <gradient  
      android:startColor = "#666666"  
      android:centerColor="#000FFF"  
      android:endColor = "#666666"    
      android:angle = "270"/>
  <!--圆角半径-->
  <corners   
      android:radius="4dip"/>  
    
</shape>  

```

<gradient/>
startColor起始颜色
centerColor中间颜色
endColor最终颜色
angle方向
<!--more-->
0表示从左到右渐变
90表示从下到上渐变
180表示从右到左渐变
依次类推，就像坐标系的方向一样。