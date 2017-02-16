---
title: px/dp/sp/
date: 2016-03-29 19:14:59
tags: Android
categories:
- 移动开发
---

> 20160728修改

## 基本概念
dp：Density-independent pixels，以160PPI屏幕为标准，则1dp=1px

px是pixel的缩写

dp和px的换算公式 
dp*ppi/160 = px
如1dp x 320ppi/160 = 2px。

sp：Scale-independent pixels，安卓的字体单位，以160PPI屏幕为标准，当字体大小为100%时，1sp=1px。

sp 与 px 的换算公式：sp*ppi/160 = px

in表示英寸，是屏幕的物理尺寸。每英寸=2.54厘米。
<!--more-->
ppi：
PPI = √（长度像素数² + 宽度像素数²） / 屏幕对角线英寸数

dpi
dpi是Dots Per Inch的缩写, 每英寸点数，即每英寸包含像素个数。比如320X480分辨率的手机，宽2英寸，高3英寸, 每英寸包含的像素点的数量为320/2=160dpi（横向）或480/3=160dpi（纵向），160就是这部手机的dpi，横向和纵向的这个值都是相同的，原因是大部分手机屏幕使用正方形的像素点。

density
屏幕密度，density和dpi的关系为 density = dpi/160

dp
也即dip，设备独立像素，device independent pixels的缩写，Android特有的单位，在屏幕密度dpi = 160屏幕上，1dp = 1px。

sp
和dp很类似，一般用来设置字体大小，和dp的区别是它可以根据用户的字体大小偏好来缩放。

## Android Drawable

我们新建一个Android项目后应该可以看到很多drawable文件夹，分别对应不同的dpi
drawable-nodpi 一些不能被拉伸的图片放在该文件夹，此图片将不会被放大，以原大小显示。
drawable-ldpi (dpi=120, density=0.75)
drawable-mdpi (dpi=160, density=1)
drawable-hdpi (dpi=240, density=1.5)
drawable-xhdpi (dpi=320, density=2)
drawable-xxhdpi (dpi=480, density=3)

最小占用设计资源，同时最好又只使用一套dpi的图片资源的方法。

首先必须清楚一个自动渲染的概念，Android SDK会自动屏幕尺寸选择对应的资源文件进行渲染，如SDK检测到你手机dpi是160的话会**优先**到drawable-mdpi文件夹下找对应的图片资源，假设你手机dpi是160，但是你只在xhpdi文件夹下有对应的图片资源文件，程序一样可以正常运行。所以理论上来说只需要提供一种规格的图片资源就ok了，如果只提供ldpi规格的图片，对于大分辨率的手机如果把图片放大就会不清晰，所以需要提供一套你需要支持的最大dpi的图片，这样即使用户的手机分辨率很小，这样图片缩小依然很清晰。

xhdpi成为首选
上面说了只需要提供一套大的dpi的图片就ok了，现在市面手机分辨率最大可达到1080X1920的分辨率，如Nexus5，dpi属于xxhdpi，但是毕竟还没普及，目前市面上最普遍的高端机的分辨率还多集中在720X1080范围，也就是多集中在xhdpi，所以目前来看xhpdi规格的图片成为了首选。当然随着技术规格的提高以后发展，以后可能市场上xxdpi的手机会越来越普遍，但这是后话。

## wrap_content VS dp
wrap_content和dp都是在Android开发中应该经常用到的。

假设你看了这篇文章后都是统一有xhdpi的资源，那么你用 wrap\_content 完全没有问题，Android会自动为其他规格的dpi屏幕适配,比如你在xhdpi放了一张120pxX120px大小的图片，那么在在hdpi屏幕上显示的就只有120/2*1.5=90px大小，但是如果你不小心同样把这张图片也放入了mdpi了，这个时候用wrap_content显示就会有问题，具体看下面的例子：

例如假设你只在drawable_xhdpi文件夹下放了test图片，xhdpi的设备会去xhdpi文件夹下找到test图片并直接显示，而mdpi的设备优先会去mdpi文件夹里查找test图片，但是没找到，最后在xhdpi文件夹下找到，然后会自动根据density计算并缩放显示出来，实际显示出来的大小是120/2=60px, 所以整体的显示比例才会看起来比较正常。

## 总结
px = dp*ppi/160 = sp*ppi/160
sp = dp = px / (ppi / 160)

px = sp*ppi/160
sp = px / (ppi / 160)

## drawable和mipmap
Android工程中有mipmap文件夹和drawable文件夹，上面已经说过drawable文件夹了，mipmap一般用来放不压缩的文件，比如各种ic_文件，而drawable文件夹一般放置selector和shape文件。