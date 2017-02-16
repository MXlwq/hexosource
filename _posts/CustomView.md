---
title: 自定义View
date: 2016-08-07 19:38:25
tags: 
- Android
- View
categories: 移动开发
---

# 三种方式

## 继承View来派生自定义组件

## 继承ViewGroup或者其子类来实现组装


三个构造函数
<!--more-->
```java
public SetView(Context context) {
	this.SetView(context,null);
	...
}
public SetView(Context context, AttributeSet attrs, int defStyle) {
	this.SetView(context,null,0);
	...
}
public SetView(Context context, AttributeSet attrs, int defStyle) {
	super.SetView(context, attrs, defStyle);
	...
}
```

* 待补充