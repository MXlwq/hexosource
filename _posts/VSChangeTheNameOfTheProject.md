---
title: VS中修改工程名的方法
date: 2015-12-06 20:19
tags: VS
categories: 技巧
---

-----
VS中新建一个项目，如果开发工作都接近尾声，客户来要求更换项目的名称，差不多要变更整个解决方案中项目名称，引用等等，这个工作量还是很大的。上网搜索解决方法，还实验了专门的修改项目名称工具，但是最后都是一堆的错误。以下是不用工具的解决方案：


## 一、先修改工程名/解决方案名
举例，原先的工程名为OldProject   想要改成NewProject
1. 找到工程/解决方案所在的文件夹（已工程名/解决方案名命名，即OldProject）
2. 打开该文件夹，有一个OldProject.sln
   将其重命名为NewProject.sln
   用记事本打开该文档，点替换，将所有OldProject替换为NewProject，保存退出.
3. OldProject文件夹下还有一个OldProject文件夹，打开里面有一个OldProject.vcproj
   将其重命名为NewProject.vcproj
同上，用记事本打开该文档，点替换，将所有OldProject替换为NewProject，保存退出.
4. 将用OldProject命名的文件夹全重命名为NewProject
5. 用VS打开该工程/解决方案，点重新生成解决方案
   这样就改好了工程名/解决方案名.
## 二、接下来是该类名
举例，原来类名OldProject 想改为NewProject
1. VS中打开该工程，CTRL+F将该工程中所有OldProject字串改为NewProject
2. 手工将工程中所有.h,.cpp,.rc等文件名字含OldProject的换为NewProject
   比如我原来资源文件叫OldProject.rc2现在改为NewProject.rc2
         我原来叫OldProjectDlg.cpp的源文件改名为NewProjectDlg.cpp
         以此类推...
3. 重新编译生成.
## 三、删除多余文件
经过上面的步骤，在工程所在的文件夹内就会生成名字含NewProject的文件
但有一些名字含OldProject的文件仍然存在，手动删除即可.
以防万一，可以删一个检查一下工程是否正常，不正常就还原它.