---
title: PendingIntent与Intent
date: 2016-04-10 22:12:55
tags:
- Android
- Intent
categories:
- 移动开发
---
## 含义
Intent表示意图，Pending表示即将发生的事情。 

Intent是及时启动，Intent 随所在的activity 消失而消失。
PendingIntent用于处理即将发生的事情。比如在通知Notification中用于跳转页面，但不是马上跳转。
PendingIntent是一个可以在满足一定条件下执行的Intent，它相比于Intent的优势在于自己携带有Context对象，这样他就不必依赖于某个activity才可以存在。

PendingIntent可以看作是对Intent的包装，通常通过getActivity,getBroadcast,getService来得到PendingIntent的实例，当前activity并不能马上启动它所包含的Intent,而是在外部执行PendingIntent时，调用Intent的。正由于PendingIntent中保存有当前App的Context，使它赋予外部App一种能力，使得外部App可以如同当前App一样的执行pendingintent里的 Intent，就算在执行时当前App已经不存在了，也能通过存在PendingIntent里的Context照样执行Intent。另外还可以处理Intent执行后的操作。

PendingIntent常和AlarmManager 和NotificationManager一起使用，可以理解为延迟执行的Intent，PendingIntent是对Intent一个包装。 

----

## PendingIntent getBroadcast方法
```java

public static PendingIntent getBroadcast(Context context, int requestCode,Intent intent, @Flags int flags) 
{
    return getBroadcastAsUser(context, requestCode, intent, flags,new UserHandle(UserHandle.myUserId()));
}
```
注意在AlarmManager中可以使用requestCode的不同(可以用闹钟的ID)，让不同闹钟不会覆盖掉。
AlarmManager是Android中的一种系统级别的提醒服务，它会为我们在特定的时刻广播一个指定的Intent。而使用Intent的时候，我们还需要它执行一个动作，如startActivity，startService，startBroadcast，才能使Intent有用。通常我们使用PendingIntent，它可以理解为对Intent的封装，包含了指定的动作。
我们可以通过PendingIntent的静态方法得到一个PendingIntent对象，如下：
```java
PendingIntent pi = PendingIntent.getBroadcast(context, 0, intent, 0); 

```

使用PendingIntent的getBroadcast (Context context, int requestCode, Intent intent, int flags)方法可以得到一个发送广播动作的PendingIntent对象。其中getBroadcast的第4个参数可以为以下4个常量或其他支持使用Intent.fillIn()来控制它的变量：
FLAG_CANCEL_CURRENT:如果描述的PendingIntent对象已经存在时，会先取消当前的PendingIntent对象再生成新的。
FLAG_NO_CREATE:如果描述的PendingIntent对象不存在，它会返回null而不是去创建它。
FLAG_ONE_SHOT:创建的PendingIntent对象只使用一次。
FLAG_UPDATE_CURRENT:如果描述的PendingIntent对象存在，则保留它，并将新的PendingIntent对象的数据替换进去。

使用PendingIntent的getBroadcast (Context context, int requestCode, Intent intent, int flags)方法可以得到一个发送广播动作的PendingIntent对象。其中getBroadcast的第4个参数可以为以下4个常量或其他支持使用Intent.fillIn()来控制它的变量：
> FLAG_CANCEL_CURRENT
如果描述的PendingIntent对象已经存在时，会先取消当前的PendingIntent对象再生成新的。
> FLAG_NO_CREATE
如果描述的PendingIntent对象不存在，它会返回null而不是去创建它。
> FLAG_ONE_SHOT
创建的PendingIntent对象只使用一次。
> FLAG_UPDATE_CURRENT
如果描述的PendingIntent对象存在，则保留它，并将新的PendingIntent对象的数据替换进去。

接下来看AlarmManager，我们通过以下代码来取得AlarmManager对象。
```java

AlarmManager am = (AlarmManager) context.getSystemService(Context.ALARM_SERVICE); 

```

## AlarmManager的常用方法

### set(int type,long startTime,PendingIntent pi)
该方法用于设置一次性闹钟。
第一个参数int type指定定时服务的类型，该参数接受如下值：
ELAPSED_REALTIME： 在指定的延时过后，发送广播，但不唤醒设备（闹钟在睡眠状态下不可用）。如果在系统休眠时闹钟触发，它将不会被传递，直到下一次设备唤醒。
ELAPSED_REALTIME_WAKEUP： 在指定的延时过后，发送广播，并唤醒设备（即使关机也会执行operation所对应的组件） 。
延时是要把系统启动的时间SystemClock.elapsedRealtime()算进去的，具体用法看代码。
RTC： 指定当系统调用System.currentTimeMillis()方法返回的值与triggerAtTime相等时启动operation所对应的设备（在指定的时刻，发送广播，但不唤醒设备）。如果在系统休眠时闹钟触发，它将不会被传递，直到下一次设备唤醒（闹钟在睡眠状态下不可用）。
RTC_WAKEUP： 指定当系统调用System.currentTimeMillis()方法返回的值与triggerAtTime相等时启动operation所对应的设备（在指定的时刻，发送广播，并唤醒设备）。即使系统关机也会执行 operation所对应的组件。
第二个参数表示闹钟执行时间。
第三个参数PendingIntent pi表示闹钟响应动作：
PendingIntent pi：是闹钟的执行动作，比如发送一个广播、给出提示等等。PendingIntent是Intent的封装类。需要注意的是，如果是通过启动服务来实现闹钟提示的话，PendingIntent对象的获取就应该采用Pending.getService(Context c,int i,Intentintent,int j)方法；如果是通过广播来实现闹钟提示的话，PendingIntent对象的获取就应该采用PendingIntent.getBroadcast(Context c,inti,Intent intent,int j)方法；如果是采用Activity的方式来实现闹钟提示的话，PendingIntent对象的获取就应该采用PendingIntent.getActivity(Context c,inti,Intent intent,int j)方法。如果这三种方法错用了的话，虽然不会报错，但是看不到闹钟提示效果。
<!--more-->
### setRepeating(int type,long startTime,long intervalTime,PendingIntent pi)
设置一个周期性执行的定时服务。第一个参数表示闹钟类型，第二个参数表示闹钟首次执行时间，第三个参数表示闹钟两次执行的间隔时间，第三个参数表示闹钟响应动作。

### setInexactRepeating(int type,long startTime,long intervalTime,PendingIntent pi)
该方法也用于设置重复闹钟，与第二个方法相似，不过其两个闹钟执行的间隔时间不是固定的而已。它相对而言更省电（power-efficient）一些，因为系统可能会将几个差不多的闹钟合并为一个来执行，减少设备的唤醒次数。第三个参数intervalTime为闹钟间隔，内置的几个变量如下：
AlarmManager.
INTERVAL_DAY：      设置闹钟，间隔一天
INTERVAL_HALF_DAY：  设置闹钟，间隔半天
INTERVAL_FIFTEEN_MINUTES：设置闹钟，间隔15分钟
INTERVAL_HALF_HOUR：     设置闹钟，间隔半个小时
INTERVAL_HOUR：  设置闹钟，间隔一个小时

如果我们设定的是发送广播的闹钟，我们还需要写一个广播接收器，并对其进行注册，它才会在闹钟开始的时候接收到广播。
如果要设定启动Activity或Service的闹钟，则在创建PendingIntent的时候，首先Intent对象需设定指定的Activity或Service的class对象，然后对应的调用PendingIntent.getActivity()或PendingIntent.getService()方法。
下面以设置发送广播的闹钟代码实例来看AlarmManager的使用：
首先设定一个闹钟：

```java

Intent intent = new Intent("pw.msdx.ACTION_SEND");  
PendingIntent sendIntent = PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);  
AlarmManager am = (AlarmManager) context.getSystemService(Context.ALARM_SERVICE);  
am.cancel(sendIntent);  
am.setRepeating(AlarmManager.RTC_WAKEUP, System.currentTimeMillis(), 60 * 10 * 1000, sendIntent); 

```
在上面的例子中，就会从当前的时间开始，每10分钟启动一次闹钟提醒。需要注意的是，如果设定的开始时间已经过去，它会马上启动闹钟提醒。

我们还需要注册这个广播接收器，这样它才能接收到广播。这里采用的是静态注册的方法，**在AndroidManifest里进行注册**：
```xml
<receiver  
    android:name=".SendReceiver"  
    android:enabled="true"  
    android:exported="true"  
     >  
    <intent-filter>  
        <action android:name="pw.msdx.ACTION_SEND"/>  
    </intent-filter>  
</receiver>
```  