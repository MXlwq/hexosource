---
title: Android Context
date: 2016-07-08 09:36:34
tags:
- Android
- Context
categories: 移动开发
thumbnail: http://img.blog.csdn.net/20150104183450879
---
# 总结
![](http://img.blog.csdn.net/20150104183450879)
备注：
数字1: 启动Activity在这些类中是可以的，但是需要创建一个新的task。一般情况不推荐。
数字2: 在这些类中去layout inflate是合法的，但是会使用系统默认的主题样式，如果你自定义了某些样式可能不会被使用。
数字3: 在receiver为null时允许，在4.2或以上的版本中，用于获取黏性广播的当前值。（可以无视）
注: ContentProvider、BroadcastReceiver之所以在上述表格中，是因为在其内部方法中都有一个context用于使用。

# 说明几个问题

## 什么是Context
Context字面意思上下文，或者叫做场景，也就是用户与操作系统操作的一个过程，比如你打电话，场景包括电话程序对应的界面，以及隐藏在背后的数据。
Activity、Service、Application都是Context的子类；
也就是说，Android系统的角度来理解：Context是一个场景，代表与操作系统的交互的一种过程。从程序的角度上来理解：Context是个抽象类，而Activity、Service、Application等都是该类的一个实现。
在仔细看一下上图：Activity、Service、Application都是继承自ContextWrapper，而ContextWrapper内部会包含一个base context，由这个base context去实现了绝大多数的方法。
<!--more-->
## Activity与Application作为Context时的区别
如果是在Activity中，大多直接传this，当在匿名内部类的时候，因为this不能用，需要写XXXActivity.this。
XXXActivity.this和getApplicationContext返回的不是同一个对象，一个是当前Activity的实例，一个是项目的Application的实例。既然区别这么明显，那么各自的使用场景肯定不同，乱使用可能会带来一些问题。

## getApplication和getApplicationContext的区别
getApplication返回结果为Application，且不同的Activity和Service返回的Application均为同一个全局对象，在ActivityThread内部有一个列表专门用于维护所有应用的application
```java
final ArrayList<Application> mAllApplications  = new ArrayList<Application>();
```
getApplicationContext返回的也是Application对象，只不过返回类型为Context，看看它的实现
```java
@Override  
public Context getApplicationContext() {  
    return (mPackageInfo != null) ?  
            mPackageInfo.getApplication() : mMainThread.getApplication();  
}  
```
上面代码中mPackageInfo是包含当前应用的包信息、比如包名、应用的安装目录等，原则上来说，作为第三方应用，包信息mPackageInfo不可能为空，在这种情况下，getApplicationContext返回的对象和getApplication是同一个。但是对于系统应用，包信息有可能为空，具体就不深入研究了。从这种角度来说，对于第三方应用，一个应用只存在一个Application对象，且通过getApplication和getApplicationContext得到的是同一个对象，两者的区别仅仅是返回类型不同。
## 应用中Context的数量

一个应用中Context的数量等于Activity的个数 + Service的个数 + 1，这个1为Application。