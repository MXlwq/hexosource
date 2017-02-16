---
title: Android应用启动流程
date: 2016-07-21 09:54:44
tags:
- Android
- Context
categories: 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/LaunchFlow.png
---

# 前言|背景
## 为什么要总结这个
1. 前一段时间面试被年哥问到了和这个问题相关的一个问题(Android中有没有类似于Java中main方法？)当时当然没有答案，就回答不知道。
2. 在看《Android开发艺术探索》的第一章节中从源码分析Android中Activity的生命周期。

## 《Android开发艺术探索》第一章里怎么讲
Activity的启动过程的源码相当复杂，涉及Instrumentation、ActivityThread和ActivityManagerService(AMS)。
这里不详细分析这一过程，简单理解，启动Activity的请求会由Instrumentation来处理，然后通过Binder向AMS发请求，AMS内部维护者一个ActivityStack并负责栈内的Activity的状态同步，AMS通过ActivityThread去同步Activity的状态从而完成生命周期方法的调用。

> 以下内容摘自[老罗的Android之旅中的一篇博文](http://blog.csdn.net/luoshengyang/article/details/6689748)
# 老生常谈 Activity启动
在Android系统中，有两种操作会引发Activity的启动，一种用户点击应用程序图标时，Launcher会为我们启动应用程序的主Activity；应用程序的默认Activity启动起来后，它又可以在内部通过调用startActvity接口启动新的Activity，依此类推，每一个Activity都可以在内部启动新的Activity。通过这种连锁反应，按需启动Activity，从而完成应用程序的功能。Activity的启动方式有两种，一种是显式的，一种是隐式的，隐式启动可以使得Activity之间的藕合性更加松散。

<!--more-->
# 流程（摘录，待理解）

* Step01 Launcher.startActivitySafely
应用程序是由Launcher启动起来（本身也是一个应用程序）其它的应用程序安装后，就会Launcher的界面上出现一个相应的图标，点击这个图标时，Launcher就会对应的应用程序启动起来。
Launcher部分源码

```java
public final class Launcher extends Activity  
        implements View.OnClickListener, OnLongClickListener, LauncherModel.Callbacks, AllAppsView.Watcher {  
  
    ......  
  
    /** 
    * Launches the intent referred by the clicked shortcut. 
    * 
    * @param v The view representing the clicked shortcut. 
    */  
    public void onClick(View v) {  
        Object tag = v.getTag();  
        if (tag instanceof ShortcutInfo) {  
            // Open shortcut  
            final Intent intent = ((ShortcutInfo) tag).intent;  
            int[] pos = new int[2];  
            v.getLocationOnScreen(pos);  
            intent.setSourceBounds(new Rect(pos[0], pos[1],  
                pos[0] + v.getWidth(), pos[1] + v.getHeight()));  
            startActivitySafely(intent, tag);  
        } else if (tag instanceof FolderInfo) {  
            ......  
        } else if (v == mHandleView) {  
            ......  
        }  
    }  
  
    void startActivitySafely(Intent intent, Object tag) {  
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
        try {  
            startActivity(intent);  
        } catch (ActivityNotFoundException e) {  
            ......  
        } catch (SecurityException e) {  
            ......  
        }  
    }  
  
    ......  
}  
```

* Step02 Activity.startActivity

Activity部分源码
```java
public class Activity extends ContextThemeWrapper  
        implements LayoutInflater.Factory,  
        Window.Callback, KeyEvent.Callback,  
        OnCreateContextMenuListener, ComponentCallbacks {  
  
    ......  
  
    @Override  
    public void startActivity(Intent intent) {  
        startActivityForResult(intent, -1);  
    }  
  
    ......  
  
}  

```

* Step03 Activity.startActivityForResult

startActivityForResult函数
```java
public void startActivityForResult(Intent intent, int requestCode) {  
        if (mParent == null) {  
            Instrumentation.ActivityResult ar =  
                mInstrumentation.execStartActivity(  
                this, mMainThread.getApplicationThread(), mToken, this,  
                intent, requestCode);  
            ......  
        } else {  
            ......  
        }  
} 
```
这里的mMainThread也是Activity类的成员变量，它的类型是ActivityThread，它代表的是应用程序的主线程。这里通过mMainThread.getApplicationThread获得它里面的ApplicationThread成员变量，它是一个Binder对象，后面我们会看到，ActivityManagerService会使用它来和ActivityThread来进行进程间通信。这里我们需注意的是，这里的mMainThread代表的是Launcher应用程序运行的进程。这里的mToken也是Activity类的成员变量，它是一个Binder对象的远程接口。

* Step04 Instrumentation.execStartActivity

Instrumentation类
```
public class Instrumentation {  
  
    ......  
  
    public ActivityResult execStartActivity(  
    Context who, IBinder contextThread, IBinder token, Activity target,  
    Intent intent, int requestCode) {  
        IApplicationThread whoThread = (IApplicationThread) contextThread;  
        if (mActivityMonitors != null) {  
            ......  
        }  
        try {  
            int result = ActivityManagerNative.getDefault()  
                .startActivity(whoThread, intent,  
                intent.resolveTypeIfNeeded(who.getContentResolver()),  
                null, 0, token, target != null ? target.mEmbeddedID : null,  
                requestCode, false, false);  
            ......  
        } catch (RemoteException e) {  
        }  
        return null;  
    }  
  
    ......  
  
}  
```

* Step05 ActivityManagerProxy.startActivity

* Step06 ActivityManagerService.startActivity

* Step07 ActivityStack.startActivityMayWait

* Step08 ActivityStack.startActivityLocked

* Step09 ActivityStack.startActivityUncheckedLocked

* Step10 Activity.resumeTopActivityLocked

* Step11 ActivityStack.startPausingLocked

* Step12 ApplicationThreadProxy.schedulePauseActivity

* Step13 ApplicationThread.schedulePauseActivity

* Step14 ActivityThread.queueOrSendMessage

* Step15 H.handleMessage

* Step16 ActivityThread.handlePauseActivity

* Step17 ActivityManagerProxy.activityPaused

* Step18 ActivityManagerService.activityPaused

* Step19 ActivityStack.activityPaused

* Step20 ActivityStack.completePauseLocked

* Step21 ActivityStack.resumeTopActivityLokced

* Step22 ActivityStack.startSpecificActivityLocked

* Step23 ActivityManagerService.startProcessLocked

* Step24 ActivityThread.main

* Step25 ActivityManagerProxy.attachApplication

* Step26 ActivityManagerService.attachApplication

* Step27 ActivityManagerService.attachApplicationLocked

* Step28 ActivityStack.realStartActivityLocked

* Step29 ApplicationThreadProxy.scheduleLaunchActivity

* Step30 ApplicationThread.scheduleLaunchActivity

* Step31 ActivityThread.queueOrSendMessage

* Step32 H.handleMessage

* Step33 ActivityThread.handleLaunchActivity

* Step34 ActivityThread.performLaunchActivity

函数前面是收集要启动的Activity的相关信息，主要package和component信息：
然后通过ClassLoader将shy.luo.activity.MainActivity类加载进来：
接下来是创建Application对象，这是根据AndroidManifest.xml配置文件中的Application标签的信息来创建的：
后面的代码主要创建Activity的上下文信息，并通过attach方法将这些上下文信息设置到MainActivity中去：
最后还要调用MainActivity的onCreate函数：

* Step35 MainActivity.onCreate

整个应用程序的启动过程要执行很多步骤，但是整体来看，主要分为以下五个阶段：

1. Step1 - Step 11：Launcher通过Binder进程间通信机制通知ActivityManagerService，它要启动一个Activity；

2. Step 12 - Step 16：ActivityManagerService通过Binder进程间通信机制通知Launcher进入Paused状态；

3. Step 17 - Step 24：Launcher通过Binder进程间通信机制通知ActivityManagerService，它已经准备就绪进入Paused状态，于是ActivityManagerService就创建一个新的进程，用来启动一个ActivityThread实例，即将要启动的Activity就是在这个ActivityThread实例中运行；

4. Step 25 - Step 27：ActivityThread通过Binder进程间通信机制将一个ApplicationThread类型的Binder对象传递给ActivityManagerService，以便以后ActivityManagerService能够通过这个Binder对象和它进行通信；

5. Step 28 - Step 35：ActivityManagerService通过Binder进程间通信机制通知ActivityThread，现在一切准备就绪，它可以真正执行Activity的启动操作了。