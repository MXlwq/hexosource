---
title: Android Volley
date: 2016-07-13 09:17:06
tags:
- Android
- 网络
categories: 移动开发
thumbnail: https://avatars0.githubusercontent.com/u/1342004?v=3&s=200---
---

> 0715更新添加ImageLoader

# 前言
出来混，迟早是要还的。在学校学的时候一直对网络编有些恐惧，总感觉抓不住要领，看书看了好多遍，代码还是总忘。今天在真实的工程中，用到了一些，感觉很无力，但是又不得不去学习，经过了大概一天的高强度“捣鼓”，现在再看稍好些，特此记录。
# 正题

## 参考链接
> [郭霖的博客](http://blog.csdn.net/guolin_blog/article/details/17482095)介绍的很详细，以下部分内容摘自其文章，感谢~

## 什么是Volley？
在开发Android应用时用到网络技术，多数情况下应用程序都会使用HTTP协议来发送和接收网络数据。Android系统中主要提供了两种方式来进行HTTP通信，HttpURLConnection和HttpClient。Android 6.0(api 23) SDK删除HttpClient的相关类的解决方法，[解决方法](https://developer.android.com/preview/behavior-changes.html)：
android studio中在相应的module下的build.gradle中加入：
```grovy
android {
    useLibrary 'org.apache.http.legacy'
}
```
<!--more-->
，几乎在任何项目的代码中我们都能看到这两个类的身影，使用率非常高。
不过HttpURLConnection和HttpClient的用法还是稍微有些复杂的，如果不进行适当封装的话，很容易就会写出不少重复代码。于是，一些Android网络通信框架也就应运而生，比如说AsyncHttpClient，它把HTTP所有的通信细节全部封装在了内部，我们只需要简单调用几行代码就可以完成通信操作了。再比如Universal-Image-Loader，它使得在界面上显示网络图片的操作变得极度简单，开发者不用关心如何从网络上获取图片，也不用关心开启线程、回收图片资源等细节，它已经把一切都做好了。
Android开发团队也是意识到了有必要将HTTP的通信操作再进行简单化，在2013年Google I/O大会上推出了一个新的网络通信框架——Volley。Volley可是说是把AsyncHttpClient和Universal-Image-Loader的优点集于了一身，既可以像AsyncHttpClient一样非常简单地进行HTTP通信，也可以像Universal-Image-Loader一样轻松加载网络上的图片。它的设计目标就是**非常适合去进行数据量不大，但通信频繁的网络操作**，而对于大数据量的网络操作，比如说下载文件等，Volley的表现就会非常糟糕。


## 怎么使用
### 准备
[Github上项目的地址](https://github.com/mcxiaoke/android-volley),下载完之后导入到Eclipse之后导出为jar包即可，也可以直接下载网上打包好的[最新的jar包(20160712)]
在Android项目，将Volley.jar文件复制到libs目录下，在Module中的依赖管理中加入该Volley.jar
Gradle管理
```grovy
compile 'com.mcxiaoke.volley:library:1.0.19'
```
### 流程
1. 创建RequestQueue对象。

2. 创建Request对象。

3. 将Request对象添加到RequestQueue里

### RequestQueue和Request
* RequestQueue用来执行请求的请求队列
* Request用来构造一个请求对象

Request对象主要有以下几种类型:
1. StringRequest 响应的主体为**字符串**
2. JsonArrayRequest 发送和接收**JSON数组**
3. JsonObjectRequest 发送和接收**JSON对象**
4. ImageRequest 发送和接收**Image**

### StringRequest

* 首先需要获取到一个RequestQueue对象，可以调用如下方法获取到：
```java
RequestQueue mQueue = Volley.newRequestQueue(context);
```
RequestQueue是一个请求队列对象，可以缓存所有的HTTP请求，然后按照一定的算法并发地发出这些请求。RequestQueue内部的设计非常合适高并发的，**不必为每一次HTTP请求都创建一个RequestQueue对象，基本上在每一个需要和网络交互的Activity中创建一个RequestQueue对象就足够了**。
* 创建一个StringRequest对象发出一条HTTP请求
```java
StringRequest stringRequest = new StringRequest("http://www.baidu.com", new Response.Listener<String>() {
        @Override
        public void onResponse(String response) {
            Log.d("TAG", response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.e("TAG", error.getMessage(), error);
        }
    });
```
StringRequest的构造函数:三个参数
1. 参数就是目标服务器的URL地址
2. 参数是服务器响应成功的回调
3. 参数是服务器响应失败的回调

* 将这个StringRequest对象添加到RequestQueue里面就可以了，如下所示：
```java
mQueue.add(stringRequest);  
```
* AndroidManifest.xml中添加如下权限
```xml
<uses-permission android:name="android.permission.INTERNET" />  
```
### GET和POST
四个参数的构造函数
```java
    StringRequest stringRequest = new StringRequest(Method.POST, url, listener, errorListener);
```
在StringRequest的匿名类中重写getParams()方法
```java
StringRequest stringRequest = new StringRequest(Method.POST, url,  listener, errorListener) {  
    @Override  
    protected Map<String, String> getParams() throws AuthFailureError {  
        Map<String, String> map = new HashMap<String, String>();  
        map.put("params1", "value1");  
        map.put("params2", "value2");  
        return map;  
    }  
};  
```

### ImageRequest
```java
RequestQueue mQueue = Volley.newRequestQueue(context);

    ImageRequest imageRequest = new ImageRequest("http://developer.android.com/images/home/aw_dac.png", new Response.Listener<Bitmap>() {
        @Override
        public void onResponse(Bitmap response) {
            imageView.setImageBitmap(response);
        }
    }, 0, 0, Config.RGB_565, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            imageView.setImageResource(R.drawable.default_image);
        }
    });
    mQueue.add(imageRequest);
```
ImageRequest的构造函数接收六个参数，第一个参数是图片的URL地址;第二个参数是图片请求成功的回调(例如可以把返回的Bitmap参数设置到ImageView中)。第三四个参数分别用于指定允许图片最大的宽度和高度，如果指定的网络图片的宽度或高度大于这里的最大值，则会对图片进行压缩，指定成0的则都不会进行压缩。第五个参数用于指定图片的颜色属性，Bitmap.Config下的几个常量都可以在这里使用，其中ARGB\_8888可以展示最好的颜色属性，每个图片像素占据4个字节的大小，而RGB_565则表示每个图片像素占据2个字节大小。第六个参数是图片请求失败的回调，这里我们当请求失败时在ImageView中显示一张默认图片。