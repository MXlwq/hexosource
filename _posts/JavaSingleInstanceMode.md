---
title: Java单例设计模式
date: 2016-06-07 22:54
tags: 
- Java 
- 设计模式
description: Java单例设计模式
categories: 语言基础
---

> 20160607更新

# 分类
## 懒汉式
```java
//所谓“懒汉式”是在你真正用到的时候才去建这个单例对象：
public class Single{ 
    private Single(){};//构造函数私有化，防止其他类创建对象
    private static Single s = null; 
    public static Single getInstance(){//对外提供一个获取对象的方法
    	if(s == null)
    	    s = new Single();  //懒汉式做法 
    	return s;
    }
}
```
## 饿汉式


```java
//所谓“饿汉式”是在不管你用的用不上，一开始就建立这个单例对象：比如：有个单例对象
public class Single{ 
    private Single(){};
    private static Single s = new Single();  //建立对象
    public static Single getInstance(){
        return s;//直接返回单例对象    
    }
}
```
<!--more-->
----
## 区别
“懒汉式”与“饿汉式”的区别，是在与建立单例对象的时间的不同。
它有以下几个要素：
 1. 私有的构造方法
 2. 指向自己实例的私有静态引用
 3. 以自己实例为返回值的静态的公有的方法
 4. 一般开发用饿汉式，因为在多线程并发时，有可能无法保证对象的唯一性，存在隐患

## 单例设计模式的同步

* 在getInstance方法上加同步符号（synchronized）

```java
public static synchronized Single getInstance(){//对外提供一个获取对象的方法
 	if(s == null)
 	    s = new Single();  //懒汉式做法 
 	return s;
}
```

* 双重检查锁定

```java
public static Single getInstance() {
    if(s == null) {  
        synchronized (Single.class) {  
           	if (s == null) {  
              	s = new Single(); 
           	}  
        }  
    }  
    return s; 
}
```

* 静态内部类

```java
public class Single {  
    private static class LazyHolder {  
       private static final Single INSTANCE = new Single();  
    }  
    private Single (){};
    public static final Single getInstance() {  
       return LazyHolder.INSTANCE;  
    }  
}  
```
这种实现方法比前两种都要好