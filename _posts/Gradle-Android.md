---
title: Android Gradle总结
date: 2016-07-01 09:17:21
tags: 
- Android
- Gradle
categories: 移动开发
thumbnail: http://cms.csdnimg.cn/article/201310/14/525b2d06d3776.jpg
---

> [部分内容来源地址Stormzhang](http://mp.weixin.qq.com/s?__biz=MzA4NTQwNDcyMA==&mid=2650661971&idx=1&sn=3fb69537bbc5fbb14d152ba6381c3b83&scene=4#wechat_redirect)

## 什么是构建工具？
大家都知道Gradle是一种构建工具，那么什么是构建工具呢？    

   
以Android开发为例。    

   
以前开发都是用Eclipse，而 Eclipse最初是用来做Java开发的，而Android的应用层软件是基于 Java 语言开发的，所以最初 Google 还是希望 Android 能在 Eclipse 上进行开发，为了满足这个需求，Google 开发了一个叫 ADT （Android Developer Tools）的东西，正是因为有了 ADT ，从此我们只需要码好代码，然后直接在 Eclipse 上进行编译、运行、签名、打包等一系列流程，而这背后的工作都是 ADT 的功劳。某种意义上 ADT 就是我们的构建工具。    

   
而自 Google 推出 Android Studio 以来，就宣布默认使用 Gradle 来作为构建工具，并且之后放弃更新 ADT ，从此 Gradle 走入 Android 开发者的视野。
   
一般来说，构建工具除了以上提到的编译、运行、签名、打包等，还具备依赖管理的功能，什么是依赖管理呢？以前在用Eclipse做Java开发，如果需要用到第三方库的时候，一般都是先下载 jar 文件，然后把 jar 文件添加到 libs 目录，bulidpath中配置，然后项目中就可以引用了。但是这种管理方式，假设第三方库有更新，需要下载最新的 Jar 文件，然后替换掉原来的，引用的库少还好，一旦引用的第三方库多，就会很麻烦，可以说这种方式只有依赖，而没有管理。    
   
现在大家不陌生的 Gradle 引用第三方库方式是这样的：    
```grovy   
compile 'com.android.support:support-v4:24.0.1'    
```
<!--more-->
类似这样的依赖方式，很方便,很直观，直接可以看到源地址，升级的话直接改下版本号就可以了，这就是所谓的依赖管理。    

所以构建工具就是对你的项目进行编译、运行、签名、打包、依赖管理等一系列功能的合集，传统的构建工具有 Make、Ant、Maven、Ivy等，而 Gradle 是新一代的自动化构建工具。   

## Gradle
Gradle 是新一代的自动化构建工具，它是一个独立的项目，跟 AS、Android 无关，官方网站：https://gradle.org/ , 类似 Ant、Maven这类构建工具都是基于 xml 来进行描述的，很臃肿，而 Gradle 采用的是一种叫做 Groovy 的语言，语法跟 Java 语法很像，但是是一种动态语言，而且在 Java 基础上做了不少改进，用起来更加简洁、灵活，而且 Gradle 完全兼容 Maven、Ivy，这点基本上宣布了 Maven、Ivy 可以被抛弃了，Gradle 的推出主要以 Java 应用为主，当然目前还支持 Android、C、C++。  

## Gradle&Android Studio
上面也提到，Gradle 跟 Android Studio 其实没有关系，但是 Gradle 官方还是很看重 Android 开发的，Google 在推出 AS 的时候选中了 Gradle 作为构建工具，为了支持 Gradle 能在 AS 上使用，Google 做了个 AS 的插件叫 Android Gradle Plugin  ，所以我们能在 AS 上使用 Gradle 完全是因为这个插件的原因。在项目的根目录有个 build.gradle 文件，里面有这么一句代码：    

   
classpath 'com.android.tools.build:gradle:2.1.2'    

   
这个就是依赖 gradle 插件的代码，后面的版本号代表的是 android gradle plugin 的版本，而不是 Gradle 的版本，这个是 Google 定的，跟 Gradle 官方没关系。关于 android gradle plugin 的更多信息可以到这里查看，这里列举了 android gradle plugin 每个版本的具体变化与具体功能：    

   
http://tools.android.com/tech-docs/new-build-system

## Gradle Wrapper 

现在默认新建一个项目，然后点击 AS 上的运行，默认就会直接帮你安装 Gradle ，我们不需要额外的安装 Gradle 了，但是其实这个 Gradle 不是真正的 Gradle ，他叫 Gradle Wrapper ，意为 Gradle 的包装，什么意思呢？假设我们本地有多个项目，一个是比较老的项目，还用着 Gradle 1.0 的版本，一个是比较新的项目用了 Gradle 2.0 的版本，但是你两个项目肯定都想要同时运行的，如果你只装了 Gradle 1.0 的话那肯定不行，所以为了解决这个问题，Google 推出了 Gradle Wrapper 的概念，就是他在你每个项目都配置了一个指定版本的 Gradle ，你可以理解为每个 Android 项目本地都有一个小型的 Gradle ，通过这个每个项目你可以支持用不同的 Gradle 版本来构建项目。    

   
理解了 Gradle Wrapper 的概念就好办了，以下的所有操作都是基于 Gradle Wrapper 的。    

   
默认我们在 AS 上第一次创建项目会自动下载 Gradle 的，这个过程很漫长，出奇的慢，但是第一次之后就ok了，接下来就是教大家用命令行测试下，请大家在终端或者 AS 带的终端上切换到所在项目的目录，然后输入 ./gradlew -v (win用户直接输入 gradlew -v) ，即可以查看当前项目所用的 gradle 的版本，gradlew 即为 gradle wrapper 的缩写，如果你是第一次执行命令行，那么会出现一个下载的提示，紧接着会打印一个个的点，这个过程很漫长，依赖你的网速，时间几分钟到几十分钟不等。    

   
有人有疑问，我 AS 上明明已经可以正常运行该项目的，说明 Gradle 已经下载过了，为什么命令行还要再下载一次？我也一直有这个疑问，理论上是不该再下载的，但是事实他就是要重新下载一次，我猜测可能是bug吧。    

   
如果下载完成输入 ./gradlew -v 出现如下结果，证明你的项目是ok的，否则就是你的项目配置有问题了。
![](http://7xruee.com1.z0.glb.clouddn.com/gradle01.png)

## Android 项目包含的 Gradle 配置文件
以GitHub 开源的[9GAG项目](https://github.com/stormzhang/9GAG)为例，来稍微介绍下一个完整的 Android 项目包含的基本 Gradle 相关的配置文件： 
![](http://7xruee.com1.z0.glb.clouddn.com/gradle02.png)
红色标记部分从上到下咱们来一步步分析：    

   
9GAG/app/build.gradle    
这个文件是 app 文件夹下这个 Module 的 gradle 配置文件，也可以算是整个项目最主要的 gradle 配置文件，具体里面的配置以后再介绍。    

   
9GAG/extras/ShimmerAndroid/build.gradle    
每一个 Module 都需要有一个 gradle 配置文件，语法都是一样，唯一不同的是开头声明的是    
apply plugin: ‘com.android.library’    

   
9GAG/gradle    
这个目录下有个 wrapper 文件夹，里面可以看到有两个文件，我们主要看下 gradle-wrapper.properties 这个文件的内容：
![](http://7xruee.com1.z0.glb.clouddn.com/gradle03.png)
可以看到里面声明了 gradle 的目录与下载路径以及当前项目使用的 gradle 版本，这些默认的路径我们一般不会更改的，这个文件里指明的 gradle 版本不对也是很多导包不成功的原因之一。    

   
9GAG/build.gradle    
这个文件是整个项目的 gradle 基础配置文件，默认的内容就是声明了 android gradle plugin 的版本。    

   
9GAG/settings.gradle    
这个文件是全局的项目配置文件，里面主要声明一些需要加入 gradle 的 module，我们来看看 9GAG 该文件的内容： 
![](http://7xruee.com1.z0.glb.clouddn.com/gradle04.png)

## 如何正确导入下载的开源项目？
我们经常会在 GitHub 发现一些优秀的开源项目，然后想要下载学习，然而第一步一般都是把源码导入到 AS 里，然后运行起来看下效果，但是经常会运行失败，这里我来给大家说下导入开源项目的正确姿势：    

   
下载一个Demo，**先打开每个 module下的 gradle 文件，即 app 目录下的 build.gradle 以及各个 library 下的 build.gradle ,首先查看 compileSdkVersion 和 buildToolsVersion，因为有些时候你本地的版本和下载的版本不一致，那么就会导致失败。**    
然后就是**检查 gradle-wrapper**，Google 有些时候要求不同的 AS 支持不同的 gradle 版本。比如 AS 1.0 的时候要求必须使用 gradle 1.x 的版本，等到 AS 2.0 的时候，Google 不支持 gradle1.x 的版本，这个时候你必须手动更新下 android gradle plugin 的版本，然后重新同步下。    

   
检查以上两个地方基本就可以导入并运行了，如果还有其他问题，那可能就是环境或者项目本身的问题了。

## 几个命令

几个有用的 Gradle 命令了：    

   
./gradlew -v 版本号    

   
./gradlew clean 清除9GAG/app目录下的build文件夹    

   
./gradlew build 检查依赖并编译打包    

   
这里注意的是 ./gradlew build 命令把 debug、release 环境的包都打出来，如果正式发布只需要打 Release 的包，该怎么办呢，下面介绍一个很有用的命令 assemble , 如    

   
./gradlew assembleDebug 编译并打Debug包    

   
./gradlew assembleRelease 编译并打Release的包    

   
值得注意的是，以上所有命令都是在终端里执行，并且必须要切换到所在项目的根目录下执行，win系统直接执行 gradlew 。  