---
title: Android Activity启动模式
date: 2016-04-04 14:15:29
tags:
- Android
- Activity
categories:
- 移动开发
---

在Android中的Activity实例化操作时，Activity的启动模式决定了Activity的启动运行方式。
在面试和笔试中也会问这种问题。
Activity启动模式设置方法：
在Android Manifest文件中的<activity></activity>标签中声明launchMode
如下：
```xml
<activity android:name=".MainActivity" android:launchMode="standard" />
```
Activity的四种启动模式：

## standard

每次激活Activity时都会创建该Activity的实例，并放入任务栈中。

## singleTop

如果在任务的栈顶正好存在该Activity的实例， 就重用该实例，否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例，只要不在栈顶，都会创建实例)。

## singleTask

如果在栈中已经有该Activity的实例，就重用该实例。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。 

## singleInstance

在一个新栈中创建该Activity实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity的实例存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例，其效果相当于多个应用程序共享一个应用，不管谁激活该Activity都会进入同一个应用中。