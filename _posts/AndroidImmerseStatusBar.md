---
title: Android沉浸顶栏
date: 2016-04-30 23:44:45
tags:
- Android
- Intent
categories: 移动开发
---
沉浸式状态栏
## 效果
看图：
Android4.4
![](http://7xruee.com1.z0.glb.clouddn.com/chenjin1.jpg)
Android5.0
![](http://7xruee.com1.z0.glb.clouddn.com/chenjin2.jpg)
只有Android版本大于等于4.4版本支持这个半透明状态栏的效果，但是4.4和5.0的显示效果有一定的差异

## 实现方法
### 定义颜色res/values/color.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="primary">#FF03A9F4</color>
    <color name="primary_dark">#FF0288D1</color>
    <color name="status_bar_color">@color/primary_dark</color>
</resources>
```
<!--more-->
### 定义styles.xml

```xml
<resources>
    <style name="BaseAppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/primary</item>
        <item name="colorPrimaryDark">@color/primary_dark</item>
        <item name="colorAccent">#FF4081</item>
    </style>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="@style/BaseAppTheme">
    </style>
</resources>
```
### values-v19或者values-v21

```xml
<resources>

    <style name="AppTheme" parent="@style/BaseAppTheme">
        <item name="android:windowTranslucentStatus">true</item>
    </style>
</resources>
```

### 布局文件
**android:fitsSystemWindows属性,通过调整当前设置这个属性的view的padding为status_bar留下空间**

```xml

<LinearLayout android:id="@+id/id_main_content"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <android.support.v7.widget.Toolbar
        android:id="@+id/chat_toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        android:fitsSystemWindows="true"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
    <...></...>
</LinearLayout>

```

**注意Manifests中Activity的theme属性设置**