---
title: Android代码混淆
date: 2016-07-12 13:25:47
tags:
- Android
- Gradle
categories: 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/android.jpg
---

# 配置

app的bulid.gradle中的bulidTypes中设置minifyEnabled为true


## 哪些禁止混淆

proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

在proguard-rules.pro文件中配置
如：
```
-keep class com.growingio.android.sdk.** {
  public *;
}
-dontwarn com.growingio.android.sdk.**

#Update包下的类都不混淆，涉及GSON解析
-keep class com.dianshijia.tvlive.update.** {*;}

#友盟
-keepclassmembers class * {
   public <init>(org.json.JSONObject);
}
-keep public class com.dianshijia.tvlive.R$*{
public static final int *;
}
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}
-dontwarn org.apache.**
-dontwarn com.umeng.**
```