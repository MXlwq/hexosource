---
title: AndroidStudio高效插件
date: 2016-08-05 22:48:09
tags: 
- Android
- 插件
categories: 收藏
---
> [转载地址](https://ydmmocoo.github.io/2016/06/28/Android-Studio%E6%8F%92%E4%BB%B6%E6%95%B4%E7%90%86/)

# 汇总

## GsonFormat
快速将json字符串转换成一个Java Bean，免去我们根据json字符串手写对应Java Bean的过程。


使用方法：快捷键Alt+S也可以使用Alt+Insert选择GsonFormat
## Android ButterKnife Zelezny
配合ButterKnife实现注解，从此不用写findViewById，想着就爽啊。在Activity，Fragment，Adapter中选中布局xml的资源id自动生成butterknife注解。


使用方法：Ctrl+Shift+B选择图上所示选项

## Android Code Generator
根据布局文件快速生成对应的Activity，Fragment，Adapter，Menu。

<!--more-->
## Android Parcelable code generator
JavaBean序列化，快速实现Parcelable接口。

## Android Methods Count
显示依赖库中得方法数

## Lifecycle Sorter
可以根据Activity或者fragment的生命周期对其生命周期方法位置进行先后排序，快捷键Ctrl + alt + K


## CodeGlance
在右边可以预览代码，实现快速定位


## findBugs-IDEA
查找bug的插件，Android Studio也提供了代码审查的功能（Analyze-Inspect Code…）

## ADB WIFI
使用wifi无线调试你的app，无需root权限
也可参考以下文章：
Android wifi无线调试App新玩法ADB WIFI

## AndroidPixelDimenGenerator
Android Studio自动生成dimen.xml文件插件

## JsonOnlineViewer
在Android Studio中请求、调试接口

## Android Styler
根据xml自动生成style代码的插件


Usage:
a. copy lines with future style from your layout.xml file
b. paste it to styles.xml file with Ctrl+Shift+D (or context menu)
c. enter name of new style in the modal window
d. your style is prepared!

## Android Drawable Importer
这是一个非常强大的图片导入插件。它导入Android图标与Material图标的Drawable ，批量导入Drawable ，多源导入Drawable（即导入某张图片各种dpi对应的图片）

## SelectorChapek for Android
通过资源文件命名自动生成Selector文件。

## GenerateSerialVersionUID
实现Serializable序列化bean

Adds a new action ‘SerialVersionUID’ in the generate menu (alt + ins). The action adds an serialVersionUID field in the current class or updates it if it already exists, and assigns it the same value the standard ‘serialver’ JDK tool would return. The action is only visible when IDEA is not rebuilding its indexes, the class is serializable and either no serialVersionUID field exists or its value is different from the one the ‘serialver’ tool would return.

## genymotion
速度较快的android模拟器



## LeakCanary
帮助你在开发阶段方便的检测出内存泄露的问题，使用起来更简单方便。
可以参考以下文章：
LeakCanary 中文使用说明



## Android Postfix Completion
可根据后缀快速完成代码，这个属于拓展吧，系统已经有这些功能，如sout、notnull等，这个插件在原有的基础上增添了一些新的功能，我更想做的是通过原作者的代码自己定制功能，那就更爽了

## Android Holo Colors Generator
通过自定义Holo主题颜色生成对应的Drawable和布局文件

## dagger-intellij-plugin
dagger可视化辅助工具

## GradleDependenciesHelperPlugin
maven gradle 依赖支持自动补全

## RemoveButterKnife
ButterKnife这个第三方库每次更新之后，绑定view的注解都会改变，从bind,到inject，再到bindview，搞得很多人都不敢升级，一旦升级，就会有巨量的代码需要手动修改，非常痛苦
当我们有一些非常棒的代码需要拿到其他项目使用，但是我们发现，那个项目对第三方库的使用是有限制的，我们不能使用butterknife，这时候，我们又得从注解改回findviewbyid
针对上面的两种情况，如果view比较少还好说，如果有几十个view，那么我们一个个的手动删除注解，写findviewbyid语句，简直是一场噩梦（别问我为什么知道这是噩梦）
所以，这种有规律又重复简单的工作为什么不能用一个插件来实现呢？于是RemoveButterKnife的想法就出现了。


## AndroidProguardPlugin
一键生成项目混淆代码插件，值得你安装~(不过目前可能有些第三方项目的混淆还未添加完全)

## otto-intellij-plugin
otto事件导航工具。

## eventbus-intellij-plugin
eventbus导航插件

## idea-markdown
markdown插件

## Sexy Editor
设置AS代码编辑区的背景图

首先点击界面的设置按钮 进入设置界面，选中Plugins,右边选择 Browser … ，输入Sexy … 下面自动弹出候选插件，右边点击Install 安装

安装成功 后需要重启AS

重启完成之后 进入设置界面 选择other Setting 下的Sexy Editor ， 右侧 insert 一张或多张图片即可，上面的其他设置可以设置方位 间隔时间 透明度等等，设置完成后，要关闭打开的文件，重新打开项目文件即可在代码编辑区显示插入的图片，作为代码编辑区的背景图。

## folding-plugin
布局文件分组的插件

## Android-DPI-Calculator
DPI计算插件

使用：

或者
## gradle-retrolambda
在java 6 7中使用 lambda表达式插件

修改编译的jdk为java8:

## Android Studio Prettify
可以将代码中的字符串写在string.xml文件中

选中字符串鼠标右键选择图中所示


这个插件还可以自动书写findViewById

## Material Theme UI
添加Material主题到你的AS

## .ignore
我们都知道在Git 中想要过滤掉一些不想提交的文件，可以把相应的文件添加到.gitignore 中，而.gitignore 这个Android Studio 插件根据不同的语言来选择模板，就不用自己在费事添加一些文件了，而且还有自动补全功能，过滤文件再也不要复制文件名了。我们做项目的时候，并不是所有文件都是要提交的，比如构建的build 文件夹，本地配置文件，每个Module 生成的iml 文件，但是我们每次add，commit 都会不小心把它们添加上去，而gitignore 就是解决这种痛点的，如果你不想提交的文件，就可以在创建项目的时候将这个文件中添加即可，将一些通用的东西屏蔽掉。

## CheckStyle-IDEA
CheckStyle-IDEA 是一个检查代码风格的插件，比如像命名约定，Javadoc，类设计等方面进行代码规范和风格的检查，你们可以遵从像Google Oracle 的Java 代码指南 ，当然也可以按照自己的规则来设置配置文件，从而有效约束你自己更好地遵循代码编写规范。

## Markdown Navigator
github:Markdown Navigator
Markdown插件

## ECTranslation
Android Studio Plugin,Translate English to Chinese. Android Studio 翻译插件,可以将英文翻译为中文。

## PermissionsDispatcher plugin
github:PermissionsDispatcher plugin
自动生成6.0权限的代码

## WakaTime
github:WakaTime
记录你在IDE上的工作时间
## AndroidWiFiADB
无线调试应用

## AndroidLocalizationer
可用于将项目中的 string 资源自动翻译为其他语言的 Android Studio/IntelliJ IDEA 插件

## TranslationPlugin
又一翻译插件,可中英互译。

## SingletonTest
快速生成单例模式的预设

## BorePlugin
Android Studio 自动生成布局代码插件

代码生成规则
a.自动遍历目标布局中所有带id的文件, 无id的不会识别处理
b.控件生成的变量名默认为id名称, 可以在弹出确认框右侧的名称输入栏中自行修改
c.所有的Button或者带clickable=true的控件, 都会自动在代码中生成setOnClickListener相关代码
d.所有EditText控件, 都会在代码中生成非空判断代码, 如果为空会提示EditText的hint内容, 如果hint为空则提示xxx字符串不能为空字样, 最后会把所有输入框的验证合并到一个submit方法中
e.会自动识别布局中的include标签, 并读取对应布局中的控件

## jimu Mirror
能够实时预览Android布局，它会监听布局文件的改动，如果有代码变化，就会立即刷新UI。
45.jRebel For Android
不仅能够做到UI布局的实时预览，它甚至做到了让你更改java代码后就能实时替换apk中的类文件，达到应用实时刷新，官网的介绍是：Skip build, install and run，因此它可以节约我们很多很多的时间，它的效果也十分不错。

## sdk-manager-plugin
SDK管理插件，自动检测更新并下载。(图片与插件无关哈)

## Codota
搜索最好的Android代码。(Studio里面直接可以搜到此插件)