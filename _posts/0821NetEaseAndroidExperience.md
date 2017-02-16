---
title: 0821网易面试
date: 2015-08-21 20:27:43
tags: 
- 面试
- Android
categories: 记录
---

很累，只记一些自己回答的不确定的问题

中午十二点半出发，两点四十到，三点半轮到我面试；
# 一面
## Android
* Android中Activity，Service，BroadcastReceiver中如何进行耗时操作
* LinearLayout的weight在两个横向排布的控件都是match_parent的情况下宽度比例是怎样的？（正好相反）
* 项目的下载文件是怎么执行的？
* 怎么回到LauncherActivity？
* MediaPlayer的一些问题
## Java
* 排序算法 10^7个数用什么方法排
* 如何阻塞一个线程

<!--more-->
# 二面
## Android
* View的绘制流程
* Http的post和get请求
## Java
final关键字修饰类或者方法
### Java final 修饰符知识点总结
#### final修饰类:

final修饰类即表示此类已经是“最后的、最终的”含义。因此，用final修饰的类不能被继承，即不能拥有自己的子类。

如果视图对一个已经用final修饰的类进行继承，在编译期间或发生错误。

#### final修饰方法：

final修饰的方法表示此方法已经是“最后的、最终的”含义，亦即此方法不能被重写（可以重载多个final修饰的方法）。

此处需要注意的一点是：因为重写的前提是子类可以从父类中继承此方法，如果父类中final修饰的方法同时访问控制权限为private，

将会导致子类中不能直接继承到此方法，因此，此时可以在子类中定义相同的方法名和参数，此时不再产生重写与final的矛盾，而是

在子类中重新定义了新的方法。
final 修饰变量：

final修饰的变量表示此变量是“最后的、最终的”含义。一旦定义了final变量并在首次为其显示初始化后，final修饰的变量值不可被改变。


# 挂