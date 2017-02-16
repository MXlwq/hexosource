---
title: AndroidStudio引入so文件
date: 2016-10-12 20:18:55
tags: 
- JNI
- Android
categories: 
- 移动开发
---
# 切入Project视图
将.so文件复制到libs目录下

# 切入Android视图
进入对应的module的build.gradle的android{...}中添加
```
sourceSets {
    main {
        jniLibs.srcDirs = ['libs']
    }
}
```
即可