---
title: DialogFragment
date: 2016-11-11 15:36:36
tags:
- Android
- View
categories: 
- 移动开发
---
# 记录
## 给DialogFragment设置宽高
在onActivityCreated()给dialog设置宽高
```java
Window window = getDialog().getWindow();
window.setLayout(ViewGroup.LayoutParams.MATCH_PARENT,ViewGroup.LayoutParams.MATCH_PARENT);
```
或者可以用
```java
Dialog dialog = getDialog();
if (dialog != null) {
    DisplayMetrics dm = new DisplayMetrics();
    getActivity().getWindowManager().getDefaultDisplay().getMetrics(dm);
    dialog.getWindow().setLayout((int) (dm.widthPixels * 0.75), (int) (dm.heightPixels * 0.8));
}
```
设置宽高为屏幕的百分比
## 设置对话框的Style
```java
xxxFragment.setStyle(DialogFragment.STYLE_NO_TITLE, R.style.DialogStyle);
```
第一个参数有四种
DialogFragment.STYLE_NO_TITLE，使用了该参数，就不需要在代码中的onCreateView添加getDialog().requestWindowFeature(Window.FEATURE_NO_TITLE);
DialogFragment.STYLE_NO_FRAME
DialogFragment.STYLE_NO_INPUT
DialogFragment.STYLE_NORMAL

DialogStyle.xml
```xml
<style name="DialogStyle" parent="@android:style/Theme.Dialog">
	<!--设置对话框背景透明-->
    <item name="android:windowBackground">@android:color/transparent</item>
    <!--设置显示对话框时背景不变灰-->
    <item name="android:backgroundDimEnabled">false</item>
</style>
```
也可以在代码中实现
```java
getDialog().getWindow().setBackgroundDrawableResource(android.R.color.transparent);
```