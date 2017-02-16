---
title: Intent
date: 2016-04-12 20:12:42
tags:
- Android
- Intent
categories:
- 移动开发
---

## Intent简介
Android基本的设计理念是鼓励减少组件间的耦合，因此Android提供了Intent,它允许在应用程序与其它的应用程序间传递Intent来执行动作和产生事件。使用Intent可以激活Android应用的三个核心组件：活动、服务和广播接收器。还可以通过intent.putExtra()等方法携带数据。

## 两种Intent区别
显式Intent：通过指定Intent组件名称来实现的，它一般用在知道目标组件名称的前提下，去调用Intent.setComponent()\Intent.setClassName()或Intent.setClass()方法 或者在new Intent(A.this,B.class)指明需要转向到的Activity,显式意图明确指定了要激活的组件是哪个组件，一般是在相同的应用程序内部实现的。
隐式Intent：通过Intent Filter来实现的，它一般用在没有明确指出目标组件名称的前提下。Android系统会根据隐式意图中设置的动作(action)、类别(category)、数据（URI和数据类型）找到最合适的组件来处理这个意图。一般是用于在不同应用程序之间。
### 隐式Intent

我们在编写需要被隐式调用的界面的清单文件时，会指定一个Intent Filter(意图过滤器)，它是用来匹配隐式Intent的，当一个意图对象被一个意图过滤器进行匹配测试时，会有三个方面会被参考到：

动作(action)
数据(data)
类别(catagory)

### Intent Filter

Intent Filter是用来注册Activity Service和Broadcast Receiver具有能在某种数据上执行一个动作的能力。
使用Intent Filter,应用程序组件告诉 Android,它们能为其它程序的组件的动作请求提供服务,包括同一个程序的组
件、本地的或第三方的应用程序。

为了注册一个应用程序组件为 Intent 处理者，在组件的 manifest 节点添加一个 intent-filter 标签。

在 Intent Filter 节点里使用下面的标签（关联属性），你能指定组件支持的动作、种类和数据：
#### 动作测试
<intent-filter>元素中可以包括子元素<action>，比如：
```xml
<intent-filter >
<action android:name="com.example.project.SHOW_CURRENT" />
<action android:name="com.example.project.SHOW_RECENT" />
<action android:name="com.example.project.SHOW_PENDING" />
</intent-filter >

```

一条<intent-filter>元素至少应该包含一个<action>，否则任何Intent请求都不能和该<intent-filter>匹配。如果Intent
请求的Action和<intent-filter>中个某一条<action>匹配，那么该Intent就通过了这条<intent-filter>的动作测试。如果
Intent请求或<intent-filter>中没有说明具体的Action类型，那么会出现下面两种情况。
(1) 如果<intent-filter>中没有包含任何Action类型，那么无论什么Intent请求都无法和这条<intent-filter>匹配；
(2) 反之，如果Intent请求中没有设定Action类型，那么只要<intent-filter>中包含有Action类型，这个Intent请求就将顺
利地通过<intent-filter>的行为测试。

#### 类别测试
<intent-filter>元素可以包含<category>子元素，比如：
```xml
<intent-filter . . . >
<category android:name="android.Intent.Category.DEFAULT" />
<category android:name="android.Intent.Category.BROWSABLE" />
</intent-filter>
```
只有当Intent请求中所有的Category与组件中某一个IntentFilter的<category>完全匹配时，才会让该 Intent请求通过测试
，IntentFilter中多余的<category>声明并不会导致匹配失败。一个没有指定任何类别测试的 IntentFilter仅仅只会匹配没
有设置类别的Intent请求。

#### 数据测试
数据在<intent-filter>中的描述如下：
```xml
<intent-filter . . . >
<data android:type="video/mpeg" android:scheme="http" . . . />
<data android:type="audio/mpeg" android:scheme="http" . . . />
</intent-filter >
```
<data>元素指定了希望接受的Intent请求的数据URI和数据类型，URI被分成三部分来进行匹配：scheme、 authority和path
。其中，用setData()设定的Inteat请求的URI数据类型和scheme必须与IntentFilter中所指定的一致。若IntentFilter中还指定了authority或path，它们也需要相匹配才会通过测试。

<!--more-->
##### action
使用 android:name 特性来指定对响应的动作名。动作名必须是独一无二的字符串，所以，一个好的习惯是使用基于 Java 包的命名方式的命名系统。

##### category
使用 android:category 属性用来指定在什么样的环境下动作才被响应。每个 Intent Filter 标签可以包含多个 category 标签。你可以指定自定义的种类或使用 Android 提供的标准值，如下所示：

##### ALTERNATIVE
你将在这章的后面所看到的，一个 Intent Filter 的用途是使用动作来帮忙填入上下文菜单。 ALTERNATIVE 种类指定，在某种数据类型的项目上可以替代默认执行的动作。例如，一个联系人的默认动作时浏览它，替代的可能是去编辑或删除它。

##### SELECTED_ALTERNATIVE
与 ALTERNATIVE 类似，但 ALTERNATIVE 总是使用下面所述的 Intent 解析来指向单一的动作。SELECTED_ALTERNATIVE在需要一个可能性列表时使用。

##### BROWSABLE
指定在浏览器中的动作。当 Intent 在浏览器中被引发，都会被指定成 BROWSABLE 种类。

##### DEFAULT
设置这个种类来让组件成为 Intent Filter 中定义的 data 的默认动作。这对使用显式 Intent 启动的 Activity 来说也是必要的。

##### GADGET
通过设置 GADGET 种类，你可以指定这个 Activity 可以嵌入到其他的 Activity 来允许。

##### HOME
HOME Activity 是设备启动（登陆屏幕)时显示的第一个 Activity 。通过指定 Intent Filter 为 HOME 种类而不指定动作的话，你正在将其设为本地 home 画面的替代。

##### LAUNCHER
使用这个种类来让一个 Activity 作为应用程序的启动项。

##### data
data 标签允许你指定组件能作用的数据的匹配；如果你的组件能处理多个的话，你可以包含多个条件。你可以使用下面属性的任意组合来指定组件支持的数据：

##### android:host
指定一个有效的主机名（eg: www.baidu.com)

##### android:mimetype
允许你设定组件能处理的数据类型。例如，<type android:value="vnd.android.cursor.dir/*"/>能匹配任何 Android 游标。

##### android:path
有效地 URI 路径值（eg:  /transport/boats/ )

##### android:port
特定主机上的有效端口。

##### android:scheme
需要一个特殊的图示（eg:  content 或 http)