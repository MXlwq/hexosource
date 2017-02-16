---
title: gravity/layout_gravity;padding/margin
date: 2016-03-29 19:05:43
tags: Android
categories:
- 移动开发
---

LinearLayout两个非常相似的属性：
android:gravity与android:layout_gravity
## 区别
 
android:gravity属性是对该view中内容的限定．比如一个button 上面的text. 你可以设置该text 相对于view的靠左，靠右等位置．

android:layout_gravity是用来设置该view相对与父view 的位置．比如一个button 在Linearlayout里，你想把该button放在linearlayout里靠左、靠右等位置就可以通过该属性设置． 
 
即android:gravity用于设置View中内容相对于View组件的对齐方式，而android:layout_gravity用于设置View组件相对于Container的对齐方式。
 
## 类似
android:paddingLeft
android:layout_marginLeft
如果在按钮上同时设置这两个属性，比如：
android:paddingLeft="10px"  按钮上设置的内容离按钮左边边界10个像素
android:layout_marginLeft="10px"  整个按钮离左边设置的内容10个像素