---
title: U盘病毒解决方法
date: 2015-05-19 17:22
tags: 电脑
categories: 收藏
---
## 问题描述：
由于很久前遇到过这个问题，最近在机房电脑又遇到这个问题，故将方法总结一下：
我觉得出现这种问题，一、是U盘的问题，二、是电脑有病毒（U盘病毒90%是kido病毒），杀毒软件不给力造成的

-----

## 解决方案：


 1. 可以下载USBCleaner软件，这个比较快捷，有取消隐藏并删除病毒文件的功能，免疫功能等，可以轻松解决问题；
 2. 运行CMD命令行工具（Win+R或者 开始、运行。。。），输入H：回车（H为U盘的盘符）进入U盘的目录，输入attrib -s -h -r autorun.inf回车，输入del autorun.inf回车。再用attrib -s -h命令把所有被隐藏的文件夹变回来。之后用资源管理器打开优盘，手动清除exe文件。
 3. 首先使用杀毒软件把U盘杀杀毒，除去电脑中那些.exe执行病毒。用记事本新建一个文本，然后输入
Windows Registry Editor Version
 4. [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced\Folder\Hidden\SHOWALL]"CheckedValue"=dword:00000001保存并取名为 文件名.REG（文件名自行定义），然后运行该文件，将文件信息添加到注册表中。 再新建一个文本文件，然后输入for /f "delims=" %%i in ('dir /ah /s/b') do attrib "%%i" -s -h保存重命名为 文件名.bat（文件名自行定义），把该文件拷贝到U盘根目录，运行该文件。之后U盘中隐藏的文件就都出现了，删除即可。
 5. 首先禁用U盘自动播放功能，然后点击“工具”、“文件夹选项”中的“查看”，勾选“显示所有文件和文件夹”，并将“隐藏受保护的操作系统文件”前的勾去掉，点击“应用”“确认”。然后手东删除这些文件就可以，为了防止再中，可以把autorun.inf里的内容清空，再保存为隐藏。