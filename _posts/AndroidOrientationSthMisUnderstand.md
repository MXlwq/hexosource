---
title: 关于AndroidOrientation的一些误解
date: 2016-09-08 11:58:17
tags: 
- View
- Android
categories: 移动开发
---
# 误解
ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE
自己的理解：默认是Landscape，且能够识别重力传感器的屏幕方向，可以变成Portrait。
事实：**锁定为Landscape**，且能够识别重力传感器的屏幕方向，左右LandScape切换。

# 相关
## 屏幕方向参数：
```java
ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED,//未指定，此为默认值。由Android系统自己选择合适的方向。
ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE,//横屏
ActivityInfo.SCREEN_ORIENTATION_PORTRAIT,//竖屏
ActivityInfo.SCREEN_ORIENTATION_USER,//用户当前的首选方向
ActivityInfo.SCREEN_ORIENTATION_BEHIND,//继承Activity堆栈中当前Activity下面的那个Activity的方向
ActivityInfo.SCREEN_ORIENTATION_SENSOR,//由物理感应器决定显示方向
ActivityInfo.SCREEN_ORIENTATION_NOSENSOR,//忽略物理感应器——即显示方向与物理感应器无关
ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE,
ActivityInfo.SCREEN_ORIENTATION_SENSOR_PORTRAIT,
ActivityInfo.SCREEN_ORIENTATION_REVERSE_LANDSCAPE,
ActivityInfo.SCREEN_ORIENTATION_REVERSE_PORTRAIT,
ActivityInfo.SCREEN_ORIENTATION_FULL_SENSOR,
```
<!--more-->
## 设置屏幕方向

setRequestedOrientation(屏幕方向参数);

示例代码：
```java
requestWindowFeature(Window.FEATURE_NO_TITLE);//隐藏标题
getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);//设置成全屏模式

setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE););//强制为横屏
setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);//竖屏
```
## 动态更改屏幕方向
```java 
if(getRequestedOrientation() == ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE)  
{  
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);  
}  
//如果是横排,则改为竖排
else if(getRequestedOrientation() == ActivityInfo.SCREEN_ORIENTATION_PORTRAIT)  
{  
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);  
}
```
## 在AndroidManifest.xml文件里设置默认方向
```xml
<activity android:name="***"  
    android:screenOrientation="portrait">
```
screenOrientation可选值(landscape|portrait|sensor|...)

## Java代码中设置屏幕方向
一般在Activity中的onCreate、onDestroy方法中控制
Java代码：
public void onCreate(Bundle savedInstanceState) {     
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);    
}   
 
也可以通过配置属性：android:configChanges="orientation|keyboardHidden|navigation"防止旋屏后重新执行onCreate。
之后在activity中重载onConfigurationChanged方法，根据不同旋转方向做其他动作，如下：
```java
    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
       //更新屏幕布局，而不执行onCreate()方法            
    }
```
## 获取屏幕的分辨率
```java
// 通过WindowManager获取   
DisplayMetrics dm = new DisplayMetrics();   
getWindowManager().getDefaultDisplay().getMetrics(dm);   
System.out.println("heigth : " + dm.heightPixels);   
System.out.println("width : " + dm.widthPixels);   
// 通过Resources获取           
DisplayMetrics dm2 = getResources().getDisplayMetrics();   
System.out.println("heigth2 : " + dm2.heightPixels);   
System.out.println("width2 : " + dm2.widthPixels);     
// 获取屏幕的默认分辨率   
Display display = getWindowManager().getDefaultDisplay();   
System.out.println("width-display :" + display.getWidth());   
System.out.println("heigth-display :" + display.getHeight());  
```
DisplayMetrics取得的是手机默认情况下，及没有旋转的情况下的分辨率，该值不会变化。
Display取得的是手机的当前分辨率，及根据当前屏幕的方向来取得宽和高.