---
title: Gradle基础
date: 2016-05-23 20:01:28
tags:
- Android
- Gradle
categories:
- 移动开发
---

> [转载自stormzhang](http://stormzhang.com/devtools/2014/12/18/android-studio-tutorial4/)


# Gradle 基本概念

Gradle是一种依赖管理工具，基于Groovy语言，面向Java应用为主，它抛弃了基于XML的各种繁琐配置，取而代之的是一种基于Groovy的内部领域特定(DSL)语言。

下面就以我的开源项目9GAG来详细讲解下和Gradle相关的知识, 和Gradle相关的几个文件一般有如下几个：
![Gradle](http://7xruee.com1.z0.glb.clouddn.com/gradle1.png)
红色标记部分从上到下咱们来一步步分析：
<!--more-->
## 9GAG/app/build.gradle
这个文件是app文件夹下这个Module的gradle配置文件，也可以算是整个项目最主要的gradle配置文件，我们来看下这个文件的内容：

```gradle
// 声明是Android程序
apply plugin: 'com.android.application'
android {
    // 编译SDK的版本
    compileSdkVersion 21
    // build tools的版本
    buildToolsVersion "21.1.1"

    defaultConfig {
    	// 应用的包名
        applicationId "me.storm.ninegag"
        minSdkVersion 14
        targetSdkVersion 21
        versionCode 1
        versionName "1.0.0"
    }

    // java版本
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
    
    buildTypes {
        debug {
            // debug模式
        }
        
        release {
            // 是否进行混淆
            minifyEnabled false
            // 混淆文件的位置
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
    
    // 移除lint检查的error
    lintOptions {
      abortOnError false
    }
}

dependencies {
    // 编译libs目录下的所有jar包
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:support-v4:21.0.2'
    compile 'com.etsy.android.grid:library:1.0.5'
    compile 'com.alexvasilkov:foldable-layout:1.0.1'
    // 编译extras目录下的ShimmerAndroid模块
    compile project(':extras:ShimmerAndroid')
}
```
这里需要说明几点：

文件开头apply plugin是最新gradle版本的写法，以前的写法是apply plugin: ‘android’, 如果还是以前的写法，请改正过来。

buildToolsVersion这个需要你本地安装该版本才行，很多人导入新的第三方库，失败的原因之一是build version的版本不对，这个可以手动更改成你本地已有的版本或者打开 SDK Manager 去下载对应版本。

applicationId代表应用的包名，也是最新的写法，这里就不在多说了。

android 5.0开始默认安装jdk1.7才能编译，但是由于mac系统自带jdk的版本是1.6，所以需要手动下载jdk1.7并配置下，具体可以见我这篇博客Mac下安装和管理Java

minifyEnabled也是最新的语法，很早之前是runProguard,这个也需要更新下。

proguardFiles这部分有两段，前一部分代表系统默认的android程序的混淆文件，该文件已经包含了基本的混淆声明，免去了我们很多事，这个文件的目录在 **/tools/proguard/proguard-android.txt** , 后一部分是我们项目里的自定义的混淆文件，目录就在 **app/proguard-rules.txt** , 如果你用Studio 1.0创建的新项目默认生成的文件名是 **proguard-rules.pro** , 这个名字没关系，在这个文件里你可以声明一些第三方依赖的一些混淆规则，由于是开源项目，9GAG里并未进行混淆，具体混淆的语法也不是本篇博客讨论的范围。最终混淆的结果是这两部分文件共同作用的。

compile project(‘:extras:ShimmerAndroid’)这一行是因为9GAG中存在其他Module，不知道Module的概念可以看下这篇博客Android Studio系列教程二–基本设置与运行, 总之你可以理解成Android Library，由于Gradle的普及以及远程仓库的完善，这种依赖渐渐的会变得非常不常见，但是你需要知道有这种依赖的。

以上文件里的内容只是基本配置，其实还有很多自定义部分，如自动打包debug，release，beta等环境，签名，多渠道打包等，后续会单独拿出来讲解。

## 9GAG/extras/ShimmerAndroid/build.gradle
每一个Module都需要有一个gradle配置文件，语法都是一样，唯一不同的是开头声明的是 apply plugin: ‘com.android.library’

## 9GAG/gradle
这个目录下有个 wrapper 文件夹，里面可以看到有两个文件，我们主要看下 gradle-wrapper.properties 这个文件的内容：

```gradle
Thu Dec 18 16:02:24 CST 2014
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.2.1-all.zip
```
可以看到里面声明了gradle的目录与下载路径以及当前项目使用的gradle版本，这些默认的路径我们一般不会更改的，这个文件里指明的gradle版本不对也是很多导包不成功的原因之一。

## 9GAG/build.gradle
这个文件是整个项目的gradle基础配置文件,我们来看看这里面的内容
```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.0.0'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}
```
内容主要包含了两个方面：一个是声明仓库的源，这里可以看到是指明的jcenter(), 之前版本则是mavenCentral(), jcenter可以理解成是一个新的中央远程仓库，兼容maven中心仓库，而且性能更优。另一个是声明了android gradle plugin的版本，android studio 1.0正式版必须要求支持gradle plugin 1.0的版本。

## 9GAG/settings.gradle
这个文件是全局的项目配置文件，里面主要声明一些需要加入gradle的module，我们来看看9GAG该文件的内容：
```gradle
include ':app', ':extras:ShimmerAndroid'
```
文件中的 app, extras:ShimmerAndroid 都是module，如果还有其他module都需要按照如上格式加进去。

# 总结
关于gradle的基础知识就介绍到这里，接下来会介绍一种我常用的快速方便的编译查看第三方开源项目的方法，如何导入Android Studio，Gradle常用基本命令，多渠道打包配置等。有疑问或者发现错误欢迎大家直接博客留言。