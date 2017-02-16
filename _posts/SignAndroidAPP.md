---
title: Android应用签名
date: 2016-07-26 18:36:05
tags: 
- Android
categories: 
- 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/android.jpg
---

# 为什么要签名
签名的作用：
1. 确定发布者的身份。应用开发者可以用过相同包名来替换已经安装的程序。
2. 确保应用的完整性。签名会对应用包中的每个文件进行处理，从而确保程序要中的文件不会被替换。
# 两种途径
## AS对Android应用签名
1. 菜单中Bulid->Gernerate Signed APK...,弹出一下对话框
2. 如果没有数字签名证书，则点击Create new,按照如下图填写数字证书的路径和密码等信息
3. 如果已经有签名证书，则1中点击OK，并使用证书
4. 单击Next，指定生成APK的路径。点击Finish即可。

<!--more-->
## 使用命名对APK签名
1. 创建Key Store库。（使用JDK提供的keytool.exe来生成）
```
keytool -genkeypair -alias 别名 -keyalg RSA -validity 有效时间 -keystore 数字证书存储路径
```
2. 在AS中通过Bulid->Make Project即可生成未签名的APK安装包。在AS的默认目录（app\bulid\outputs\apk）中即可找到未签名的文件。

3. 使用jarsigner.exe（JDK中的bin目录中提供的工具）对未签名的APK安装包进行签名。

在命令行中输入如下命令：
```
jarsigner -verbose -keystore 数字证书路径 -signedjar 签名包名 未签名包 数字证书别名
```
