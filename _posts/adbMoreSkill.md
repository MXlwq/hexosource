---
title: adb技巧
date: 2016-09-19 18:16:07
tags: 
- ADB
- Android
categories: 
- 移动开发
---
# 什么是ADB
ADB，即 Android Debug Bridge，熟练使用ADB命令将会大大提升我们的开发效率，ADB命令很多,下面总结一下在开发常用到的一些ADB命令。
## log输出重定向
```bash
adb logcat -d > logcat.txt
```
## 查看ADB版本
```bash
adb version
```
## 查看ADB版本
```bash
adb devices
```
## 查看ADB版本
```bash
adb version
```
## adb连接和断开设备
```bash
adb connect xxx.xxx.xxx.xxx
adb disconnect
```
## 安装应用
```bash
adb install <apkfile>
例如: adb install demo.apk(默认目录)

adb install /Users/storm/temp/demo.apk(绝对路径)
```
<!--more-->
保留数据和缓存文件，重新安装apk：
```bash
adb install -r demo.apk
```
安装apk到sd卡：
```bash
adb install -s demo.apk
```
## 卸载应用
直接卸载
```bash
adb uninstall <package>
// 如：adb uninstall com.stormzhang.demo
```
卸载 app 但保留数据和缓存文件：
```bash
adb uninstall -k com.stormzhang.demo
```
## 多个设备
卸载应用
```bash
adb -s ip:port(设备号) uninstall com.xxx.xxx
```

# 其他有用的命令
## 包管理
列出手机装的所有app的包名：
```bash
adb shell pm list packages
```
## 列出系统应用的所有包名：
```bash
adb shell pm list packages -s
```
## 列出除了系统应用的第三方应用包名：
```bash
adb shell pm list packages -3
```
## 使用 grep 命令来过滤：
```bash
adb shell pm list packages | grep qq
```
## 清除应用数据与缓存
有些时候我们测试需要清除数据与缓存，则需要用到如下命令：
```bash
adb shell pm clear <packagename>
// 如：adb shell pm clear com.stormzhang.demo
```
## 启动应用
如果我们想要通过 adb 来启动应用
```bash
adb shell am start -n com.stormzhang.demo/.ui.SplashActivity
```
## 强制停止应用
有些时候应用卡死了，需要强制停止，则执行以下命令：
```bash
adb shell am force-stop <packagename>
// 如：adb shell am force-stop com.stormzhang.demo
```
## 查看日志
```bash
adb logcat
```
## 重启
```bash
adb reboot
```
## 获取序列号
```bash
adb get-serialno
```
## 获取 MAC 地址
```bash
adb shell  cat /sys/class/net/wlan0/address
```
## 查看设备型号
```bash
adb shell getprop ro.product.model
```
## 查看 Android 系统版本
```bash
adb shell getprop ro.build.version.release
```
## 查看屏幕分辨率
```bash
adb shell wm size
```
## 查看屏幕密度
```bash
adb shell wm density
```