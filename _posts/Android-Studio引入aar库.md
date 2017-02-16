---
title: Android Studio引入aar库
date: 2017-01-10 21:20:28
tags: 
- Android
- 技巧
categories: 移动开发
---
# 什么是aar
AAR（Android Archive）包是一个Android库项目的二进制归档文件。
文件扩展名是.aar，但文件本身是具有以下条目的一个简单zip文件：
/AndroidManifest.xml (强制)
/classes.jar (强制)
/res/ (强制)
/R.txt (强制)
/assets/ (可选)
/libs/*.jar (可选)
/jni/<abi>/*.so (可选)
/proguard.txt (可选)
/lint.jar (可选)
# Android Studio导入
```groovy
repositories {  
    flatDir {  
        dirs 'libs'  
    }  
}  

dependencies {
    compile name: 'libname', ext: 'aar' 
}
```