---
title: Android openFileOutput
date: 2016-12-23 18:43:29
tags: 
- Android
- IO
categories: 移动开发
---

openFileOutput()方法的第一参数用于指定文件名称，不能包含路径分隔符“/” ，如果文件不存在，Android 会自动创建它。
创建的文件保存在/data/data/<package name>/files目录，如： /data/data/liwenquan.top/files/test.txt ，通过点击Eclipse菜单“Window”-“Show View”-“Other”，在对话窗口中展开android文件夹，选择下面的File Explorer视图，然后在File Explorer视图中展开/data/data/<package name>/files目录就可以看到该文件。
openFileOutput()方法的第二参数用于指定操作模式，
四种模式，分别为： 
Context.MODE_PRIVATE=0
Context.MODE_APPEND=32768(0x8000)
Context.MODE_WORLD_READABLE=1
Context.MODE_WORLD_WRITEABLE=2

Context.MODE_APPEND：模式会检查文件是否存在，存在就往文件追加内容，否则就创建新文件。
Context.MODE_WORLD_READABLE和Context.MODE_WORLD_WRITEABLE用来控制其他应用是否有权限读写该文件。
MODE_WORLD_READABLE：表示当前文件可以被其他应用读取；MODE_WORLD_WRITEABLE：表示当前文件可以被其他应用写入。
如果希望文件被其他应用读和写，可以传入：
openFileOutput(“FileName”, Context.MODE_WORLD_READABLE + Context.MODE_WORLD_WRITEABLE);