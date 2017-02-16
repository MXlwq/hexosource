---
title: Java反射机制
date: 2016-03-29 20:27:16
tags: 
- Java
- 反射机制
- Android
categories:
- 语言基础
---

今天看到Android中的Intent的方法原形，突然想起了Java的反射机制，记录一下吧
首先说一下什么是反射：
其实很容易明白，条件反射大家都很熟悉，而**Java中的反射就是对于一个类，能够知道类的方法和属性，对于任意一个对象，都能调用它的任意一个方法，这种动态获取的信息以及动态调用对象的功能成为Java的反射机制。**
<!--more-->
## Java的反射机制提供的功能：
在运行时判断任意一个对象所属的类，在运行时构造任意一个类的对象，在运行时判断一个类所具有的成员变量个方法，在运行时调用任意一个对象的方法。
## Java的反射的三种方式：
### 方式一：
利用Object类的getClass()方法，但是显然要求先产生对象才行；
```java
Date date=new Date();
Class<?>cls=date.getClass();
```

### 方式二：
利用"类.class"的形式取得Class类的对象，在Hibernate上使用；
```java
Class<?>cls=java.util.Date.class;
```

### 方式三：
Class类提供的方法，在系统构架中使用。
```java
Class<?>cls=Class.forName("java.util.Date");
```

## Intent
Intent用于Component[1]和操作系统之间进行通信的媒介工具，
### Intent的方法原形；
```java
public Intent(Context packageContext,Class<?> cls)
```
Activity启动另一个Activity是通过startActivity的
### startActivity方法原形：
```java
public void startActivity(Intent intent);
```
很明显，这并不是一个类方法，一个Activity在调用时实际上是给操作系统的ActivityManager发送请求的。


实际在启动是这样写的：
```java
Intent intent=new Intent(Main.this,AnotherAty.class);
startActivity(intent);
```
这里的AnotherAty.class用的就是Java的反射机制啦~

注：
[1]:Android中的Component包含四种，分别是：
Activity
Service
Content Provider
Broadcast receiver