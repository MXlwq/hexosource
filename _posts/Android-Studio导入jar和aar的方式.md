---
title: Android-Studio导入jar和aar的方式
date: 2017-02-13 20:05:18
tags: 
- Android
categories: 移动开发
---

## jar
Android Studio中，导入jar，要先将jar放在libs目录下，如果没有libs，手动创建一个Android Resource ,注意这个目录是和代码的src同级的。顺便说一句，assets目录是和res目录同级的，都在src/main或者src/其他flavor下；
在build.gradle的dependencies标签添加：
compile files('libs/***.jar')
或者直接在jar包右键Add as Library 并选择Module
导入Jar文件
当libs文件夹下面有多个文件时，可以用一句代码包含这些包:
compile fileTree(dir: 'libs', include: ['*.jar'])
当有文件不需要被包含时，可以这样:
compile fileTree(dir: 'libs', exclude: ['android-support*.jar'], include: ['*.jar'])
+表示一个字符，*表示０到多个字符。

如果有so，需要加入
```groovy
android{
	sourceSets{
		main{
    		//指定jni目录位置
			jniLibs.srcDirs = ['libs']
		}
		flavor{
			...
		}
	}
}
```
## aar
导入aar：将aar拷贝到libs文件夹，在module的build.gradle文件根节点增加
```groovy
repositories {
    flatDir() {
    	//指定目录位置
        dirs 'libs'
    }
}
```
在build.gradle的dependencies标签添加：
compile(name:'aar包名不带扩展名', ext:'aar')