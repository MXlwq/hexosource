---
title: Gradle和Gradle Wapper的区别
date: 2017-02-11 16:21:19
tags: 
- Android
- Gradle
categories: 移动开发
---
# Android Studio中多处gradle的配置

## Project-level中的gradle配置
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'
    }
}
## gradle-wapper.properties最后一行中的配置
distributionUrl=https\://services.gradle.org/distributions/gradle-xxx-all.zip

gradle wrapper task利用gradle-wrapper.properties来下载指定的gradle

build.gradle中
classpath 'com.android.tools.build:gradle:2.2.3'的2.2.3是android plugin库
中gradle插件的版本

而gradle-xxx-all.zip是去https://services.gradle.org/distributions下载gradle

# 配置Grale环境变量
## MAC
1. 终端中键入 open .bash_profile

2. 加入export GRADLE_HOME=/Applications/Android\ Studio.app/Contents/gradle/gradle-2.14.1
(注意在Android Studio中间加入\转义后面的空格)

3. 终端中键入 source .bash_profile

4. 终端中键入 gradle -v
(查看gradle版本--有版本输出则表明环境变量配置好)