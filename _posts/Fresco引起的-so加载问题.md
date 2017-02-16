---
title: Fresco引起的.so加载问题
date: 2016-12-14 14:08:46
tags: 
- Android
- 框架
categories: 移动开发
---
# 问题描述
java.lang.UnsatisfiedLinkError: dalvik.system.PathClassLoader[DexPathList[[zip file "/data/app/com.dianshijia.tvlive-1/base.apk"],nativeLibraryDirectories=[/data/app/com.dianshijia.tvlive-1/lib/arm64, /data/app/com.dianshijia.tvlive-1/base.apk!/lib/arm64-v8a, /vendor/lib64, /system/lib64]]] couldn't find "xxxxxx.so"

# 解决方案

```gradle
splits {
    abi {
        enable true
        reset()
        include 'x86', 'x86_64', 'armeabi-v7a', 'armeabi'
        universalApk false
    }
}
```

# 参考资料

** http://frescolib.org/docs/multiple-apks.html#_