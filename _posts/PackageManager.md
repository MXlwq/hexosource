---
title: PackageManager
date: 2016-04-13 20:25:04
tags:
- Android
- Intent
categories: 移动开发

---

## PackageManager的功能：
 
1、安装，卸载应用 
2、查询permission相关信息 
3、查询Application相关信息(application，activity，receiver，service，provider及相应属性等） 
4、查询已安装应用 
5、增加，删除permission 
6、清除用户数据、缓存，代码段等 
 
 
 
## PackageManager相关类和方法介绍：
 
### PackageManager类
 
说明： 获得已安装的应用程序信息 。可以通过getPackageManager()方法获得。 
 
常用方法： 
public abstract PackageManager getPackageManager()  
功能：获得一个PackageManger对象  
 
public abstract Drawable getApplicationIcon(String packageName)
参数： packageName 包名
功能：返回给定包名的图标，否则返回null
 
public abstract ApplicationInfo getApplicationInfo(String packageName, int flags)
参数：
　　packagename 包名
　　flags 该ApplicationInfo是此flags标记，通常可以直接赋予常数0即可
功能：返回该ApplicationInfo对象
 
public abstract List<ApplicationInfo> getInstalledApplications(int flags)
参数：
　　flag为一般为GET_UNINSTALLED_PACKAGES，那么此时会返回所有ApplicationInfo。我们可以对ApplicationInfo
　　的flags过滤,得到我们需要的。
功能：返回给定条件的所有PackageInfo
 
public abstract List<PackageInfo> getInstalledPackages(int flags) 
参数如上
功能：返回给定条件的所有PackageInfo
 
public abstract ResolveInfo resolveActivity(Intent intent, int flags)
参数：  
　　intent 查寻条件，Activity所配置的action和category
　　flags： MATCH_DEFAULT_ONLY     ：Category必须带有CATEGORY_DEFAULT的Activity，才匹配
　　　　　　 GET_INTENT_FILTERS     ：匹配Intent条件即可
　　　　　　 GET_RESOLVED_FILTER    ：匹配Intent条件即可
功能 ：返回给定条件的ResolveInfo对象(本质上是Activity)
<!--more-->
public abstract List<ResolveInfo> queryIntentActivities(Intent intent, int flags)
参数同上
功能 ：返回给定条件的所有ResolveInfo对象(本质上是Activity)，集合对象
 
public abstract ResolveInfo resolveService(Intent intent, int flags)
参数同上
功能 ：返回给定条件的ResolveInfo对象(本质上是Service)
 
public abstract List<ResolveInfo> queryIntentServices(Intent intent, int flags)
参数同上
功能 ：返回给定条件的所有ResolveInfo对象(本质上是Service)，集合对象

### PackageItemInfo类
 
说明： AndroidManifest.xml文件中所有节点的基类，提供了这些节点的基本信息：label、icon、 meta-data。它并不直接使用，而是由子类继承然后调用相应方法。
### ApplicationInfo类 继承自 PackageItemInfo类
 

说明：获取一个特定引用程序中<application>节点的信息。
 
字段说明：
flags字段： FLAG_SYSTEM　系统应用程序
            FLAG_EXTERNAL_STORAGE　表示该应用安装在sdcard中
 
常用方法继承至PackageItemInfo类中的loadIcon()和loadLabel()

### ActivityInfo类 继承自 PackageItemInfo类
 
说明： 获得应用程序中<activity/>或者 <receiver/>节点的信息 。我们可以通过它来获取我们设置的任何属性，包括theme 、launchMode、launchmode等
 
常用方法继承至PackageItemInfo类中的loadIcon()和loadLabel()
### ServiceInfo类 继承自 PackageItemInfo类
 
说明：与ActivityInfo类似，代表<service>节点信息
### ResolveInfo类
 
说明：根据<intent>节点来获取其上一层目录的信息，通常是<activity>、<receiver>、<service>节点信息。
常用方法有loadIcon(PackageManager pm)和loadLabel(PackageManager pm)
