---
title: Android Fragment生命周期
date: 2016-04-05 23:00:58
tags:
- Android
- Fragment
categories:
- 移动开发
---

Android在3.0版本引入了Fragment功能，它非常类似于Activity，可以像Activity一样包含布局。

动态添加Fragment主要分为4步：
1. 获取到FragmentManager，在Activity中可以直接通过getFragmentManager得到。
2. 开启一个事务，通过调用beginTransaction方法开启。
3. 向容器内加入Fragment，一般使用replace方法实现，需要传入容器的id和Fragment的实例。
4. 提交事务，调用commit方法提交。



除了和Activity的生命周期方法名相同的之外，Fragment还有如下独有的生命周期方法：

onAttach方法：Fragment和Activity建立关联的时候调用。
onCreateView方法：为Fragment加载布局时调用。
onActivityCreated方法：当Activity中的onCreate方法执行完后调用。
onDestroyView方法：Fragment中的布局被移除时调用。
onDetach方法：Fragment和Activity解除关联的时候调用。