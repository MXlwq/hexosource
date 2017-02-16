---
title: VideoView屏幕适配
date: 2016-09-13 19:20:05
tags: 
- View
- Android
categories: 
- 移动开发
---
# 方法
## setLayoutParams
### 全屏拉伸
VideoView由一个RelativeLayout包裹
```java
RelativeLayout.LayoutParams layoutParams=
new RelativeLayout.LayoutParams(RelativeLayout.LayoutParams.FILL_PARENT, RelativeLayout.LayoutParams.FILL_PARENT);
layoutParams.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);
layoutParams.addRule(RelativeLayout.ALIGN_PARENT_TOP);
layoutParams.addRule(RelativeLayout.ALIGN_PARENT_LEFT);
layoutParams.addRule(RelativeLayout.ALIGN_PARENT_RIGHT);
mVideoView.setLayoutParams(layoutParams);
```
### 智能全屏
什么是智能全屏?
<!--more-->
屏幕的宽高比(横屏下)比视频的宽高比小(电影类)则不拉伸y方向，只是以黑边填充(由外面的RelativeLayout来实现)
屏幕的宽高比(横屏下)比视频的宽高比大(古老电视剧4：3类)则全屏拉伸
```java
float videoRatio = (float) mVideoView.getMeasuredWidth() / (float) mVideoView.getMeasuredHeight();
if (videoRatio < getScreenRatio()) {
//全屏拉伸代码
...
}
```
```java
private float getScreenRatio() {
    Resources resources = this.getResources();
    DisplayMetrics dm = resources.getDisplayMetrics();
    float width = dm.widthPixels;
    float height = dm.heightPixels;
    return width / height;
}
```
### 原始比例
通过mVideoView.getMeasuredWidth()实现
```java
RelativeLayout.LayoutParams lp = new RelativeLayout.LayoutParams(mVideoView.getMeasuredWidth(), mVideoView.getMeasuredHeight());
lp.addRule(RelativeLayout.CENTER_IN_PARENT);
mVideoView.setLayoutParams(lp);
```
# 获取屏幕相关信息的方法

Android获取屏幕宽度的4种方法 
方法一：
```java
WindowManager wm = (WindowManager) this.getSystemService(Context.WINDOW_SERVICE);
int width = wm.getDefaultDisplay().getWidth();
int height = wm.getDefaultDisplay().getHeight();
```
方法二：
```java
WindowManager wm1 = this.getWindowManager();
int width1 = wm1.getDefaultDisplay().getWidth();
int height1 = wm1.getDefaultDisplay().getHeight();
```
方法一与方法二获取屏幕宽度的方法类似，只是获取WindowManager 对象时的途径不同。
方法三：
```java
WindowManager manager = this.getWindowManager();
DisplayMetrics outMetrics = new DisplayMetrics();
manager.getDefaultDisplay().getMetrics(outMetrics);
int width2 = outMetrics.widthPixels;
int height2 = outMetrics.heightPixels;
```
方法四：
```java
Resources resources = this.getResources();
DisplayMetrics dm = resources.getDisplayMetrics();
float density1 = dm.density;
int width3 = dm.widthPixels;
int height3 = dm.heightPixels;
```
方法三与方法四类似。

# 效果
![](http://7xruee.com1.z0.glb.clouddn.com/videoView.jpg)