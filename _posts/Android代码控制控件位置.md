---
title: Android代码控制控件位置
date: 2017-01-18 23:55:10
tags:
---
分为两点，一个是在用Java代码写界面时，一个是移动控件的位置
# Java代码写界面
## 控制控件大小

## 控制空间相对父容器的位置(居中...)
```java
FrameLayout.LayoutParams layoutParams = new FrameLayout.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);
layoutParams.gravity = Gravity.CENTER_VERTICAL;
***Layout.addView(view, layoutParams);

//或者只控制大小
***Layout.addView(textView, ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
```
# 移动控件位置