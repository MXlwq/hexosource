---
title: ButterKnife配置和使用
date: 2016-07-26 18:09:17
tags: 
- Android
- 插件
categories: 
- 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/Butterknife.png
---

# 下载
* 以AS为例
> [github地址](https://github.com/JakeWharton/butterknife)

project-level build.gradle引入'android-apt' 插件
```
buildscript {
  repositories {
    mavenCentral()
   }
  dependencies {
    classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
  }
}
```
应用'android-apt'插件到module-level build.gradle并加入Butter Knife 依赖

```
apply plugin: 'android-apt'

android {
  ...
}

dependencies {
  compile 'com.jakewharton:butterknife:8.4.0'
  apt 'com.jakewharton:butterknife-compiler:8.4.0'
}
```
<!--more-->
# 应用
```java
class ExampleActivity extends Activity {
  @BindView(R.id.user) EditText username;
  @BindView(R.id.pass) EditText password;

  @BindString(R.string.login_error) String loginErrorMessage;

  @OnClick(R.id.submit) void submit() {
    // TODO call server...
  }

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    ButterKnife.bind(this);
    // TODO Use fields...
  }
}
```