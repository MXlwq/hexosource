---
title: PackageInfo获取应用安装信息
date: 2016-09-28 14:57:19
tags: 
- Android
categories: 
- 移动开发
---

# 效果
判断app安装的路径(新安装or升级覆盖安装)
# 实现
## Android系统方法
```java
PackageManager pm = context.getPackageManager();
PackageInfo pi;
try {
    pi = pm.getPackageInfo(context.getPackageName(), 0);
    return pi.firstInstallTime == pi.lastUpdateTime;
} catch (PackageManager.NameNotFoundException e) {
    e.printStackTrace();
}
return true;
```
* firstInstallTime为应用首次安装的时间
* lastUpdateTime为应用最后一次修改的时间
(覆盖安装会改变该时间，清除数据不会影响，由Android系统管理)
getPackageInfo方法中第一个参数为应用的包名，可以通过getPackageName得到，或者通过在gradle中配置后调用BuildConfig.APPLICATION_ID
