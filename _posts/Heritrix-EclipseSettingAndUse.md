---
title: Heritrix1.14.4在Eclipse的配置和使用
date: 2015-12-05 17:50
tags: 网络
categories: 技能树
---

**1**、首先在 Eclipse 中新建 Java 工程 ，工程名自取，以MyHeritrix为例。利用下载的源代码包根据以下步骤来配置这个工程。

**2**、导入类库
Heritrix 所用到的工具类库都在 heritrix-1.14.4-src\lib 目录下，需要将其导入 MyHeritrix 工程。
1）将 heritrix-1.14.4-src 下的 lib 文件夹拷贝到 MyHeritrix 项目根目录；
2）在 MyHeritrix 工程上右键单击选择“Build PathConfigure Build Path …”，然后选择 Library 选项卡，单击“Add JARs …”，如图 1 所示。
![图 1. 导入类库 - 导入前](http://img.blog.csdn.net/20151205173820969)
图 1. 导入类库 - 导入前
 
3）在弹出的“JAR Selection”对话框中选择 MyHeritrix 工程 lib 文件夹下所有的 jar 文件，然后点击 OK 按钮。如图 2 所示。

图 2. 选择类库
![图 2. 选择类库](http://img.blog.csdn.net/20151205173845329)  
<!--more-->
设置完成后如图 3 所示：

图 3. 导入类库 - 导入后
 ![图 3. 导入类库 - 导入后](http://img.blog.csdn.net/20151205173858632)
**3**、拷贝源代码
1）将 heritrix-1.14.4-src\src\java 下的 com、org 和 st 三个文件夹拷贝进 MyHeritrix 工程的 src 下。这三个文件夹包含了运行 Heritrix 所必须的核心源代码；
2）将 heritrix-1.14.4-src\src\resources\org\archive\util 下的文件 tlds-alpha-by-domain.txt 拷贝到 MyHeritrix\src\org\archive\util 中。该文件是一个顶级域名列表，在 Heritrix 启动时会被读取；
3）将 heritrix-1.14.4-src\src 下 conf 文件夹拷贝至 Heritrix 工程根目录。它包含了 Heritrix 运行所需的配置文件；
4）将 heritrix-1.14.4-src\src 中的 webapps 文件夹拷贝至 Heritrix 工程根目录。该文件夹是用来提供 servlet 引擎的，包含了 Heritrix 的 web UI 文件。需要注意的是它不包含帮助文档，如果想使用帮助，可以将 heritrix-1.14.4.zip\docs 中的 articles 文件夹拷贝到 MyHeritrix\webapps\admin\docs（需新建 docs 文件夹）下。或直接用 heritrix-1.14.4.zip 的 webapps 文件夹替换 heritrix-1.14.4-src\src 中的 webapps 文件夹，缺点是这个是打包好的 .war 文件，无法修改源代码。
拷贝完毕后的 MyHeritrix 工程目录层次如图 4 所示。这里运行 Heritrix 所需的源代码等已经准备完备，下面需要修改配置文件并添加运行参数。
<!--more-->
图 4. MyHeritrix 工程的目录层次
 ![图 4. MyHeritrix 工程的目录层次](http://img.blog.csdn.net/20151205173914305)
**4**、修改配置文件
conf 文件夹是用来提供配置文件的，里面包含了一个很重要的文件：heritrix.properties。heritrix.properties 中配置了大量与 Heritrix 运行息息相关的参数，这些参数的配置决定了 Heritrix 运行时的一些默认工具类、Web UI 的启动参数，以及 Heritrix 的日志格式等。当第一次运行 Heritrix 时，只需要修改该文件，为其加入 Web UI 的用户名和密码。如图 5 所示，设置 heritrix.cmdline.admin = admin:admin，“admin:admin”分别为用户名和密码。然后设置版本参数为 1.14.4。

图 5. 设置登陆用户名和密码
 ![图 5. 设置登陆用户名和密码](http://img.blog.csdn.net/20151205173926792)
**5**、配置运行文件
在 MyHeritrix 工程上右键单击选择“Run As Run Configurations”，确保 Main 选项卡中的 Project 和 Main class 选项内容正确，如图 6 所示。其中的 Name 参数可以设置为任何方便识别的名字。
图 6. 配置运行文件—设置工程和类
 ![图 6. 配置运行文件—设置工程和类](http://img.blog.csdn.net/20151205173940145)
然后在 Classpath 页选择 UserEntries 选项，此时右边的 Advanced 按钮处于激活状态，点击它，在弹出的对话框中选择“Add Folders”，然后选择 MyHeritrix 工程下的 conf 文件夹。如图 7 所示。

图 7. 添加配置文件
 ![图 7. 添加配置文件](http://img.blog.csdn.net/20151205173951621)
至此我们的 MyHeritrix 工程已经可以运行起来了。
**6**、找到 org.archive.crawler 包中的 Heritrix.java 文件，它是 Heritrix 爬虫启动的入口，右键单击选择“Run AsJava Application”，如果配置正确，会在控制台输出如图 8 所示的启动信息。

图 8. 运行成功时控制台输出
 ![图 8. 运行成功时控制台输出](http://img.blog.csdn.net/20151205174001863)
在浏览器中输入 http://localhost:8080，会打开如图 9 所示的 Web UI 登录界面。
 ![登录界面](http://img.blog.csdn.net/20151205174021046)
输入之前设置的用户名 / 密码：admin/admin，进入到 Heritrix 的管理界面，如图 10 所示。因为我们还没有创建抓取任务，所以 Jobs 显示为 0。

图 10. Heritrix 控制台
 ![Heritrix 控制台](http://img.blog.csdn.net/20151205174032327)
Heritrix 使用 Web 用户界面来启动、设置爬行参数并监控爬行，简单直观，易于管理。下面我们以北京林业大学首页 (http://www.bjfu.edu.cn/) 为种子站点来创建一个抓取实例。
在 Jobs 页面创建一个新的抓取任务，如图 11 所示，可以创建四种任务类型。

图 11. 创建抓取任务
![创建抓取任务](http://img.blog.csdn.net/20151205174041331) 
Based on existing job：以一个已经有的抓取任务为模板生成新的抓取任务。
Based on a recovery：在以前的某个任务中，可能设置过一些状态点，新的任务将从这个设置的状态点开始。
Based on a profile：专门为不同的任务设置了一些模板，新建的任务将按照模板来生成。
With defaults：这个最简单，表示按默认的配置来生成一个任务。

这里我们选择“With defaults”，然后输入任务相关信息，如图 12 所示。

图 12. 创建抓取任务“BJFU”
 ![图 12. 创建抓取任务“BJFU”](http://img.blog.csdn.net/20151205174053536)
注意图 11 中下方的按钮，通过这些按钮可以对抓取工作进行详细的设置，这里我们只做一些必须的设置。
首先点击“Modules”按钮，在相应的页面为此次任务设置各个处理模块，一共有七项可配置的内容，这里我们只设置 Crawl Scope 和 Writers 两项，下面简要介绍各项的意义。

1）Select Crawl Scope：Crawl Scope 用于配置当前应该在什么范围内抓取网页链接。例如选择 BroadScope 则表示当前的抓取范围不受限制，选择 HostScope 则表示抓取的范围在当前的 Host 范围内。在这里我们选择 org.archive.crawler.scope.BroadScope，并单击右边的 Change 按钮保存设置状态。
2）Select URI Frontier：Frontier 是一个 URL 的处理器，它决定下一个被处理的 URL 是什么。同时，它还会将经由处理器链解析出来的 URL 加入到等待处理的队列中去。这里我们使用默认值。
3）Select Pre Processors：这个队列的处理器是用来对抓取时的一些先决条件进行判断。比如判断 robot.txt 信息等，它是整个处理器链的入口。这里我们使用默认值。
4）Select Fetchers：这个参数用于解析网络传输协议，比如解析 DNS、HTTP 或 FTP 等。这里我们使用默认值。
5）Select Extractors：主要是用于解析当前服务器返回的内容，取出页面中的 URL，等待下次继续抓取。这里我们使用默认值。
6）Select Writers：它主要用于设定将所抓取到的信息以何种形式写入磁盘。一种是采用压缩的方式（Arc），还有一种是镜像方式（Mirror）。这里我们选择简单直观的镜像方式：org.archive.crawler.writer.MirrorWriterProcessor。
7）Select Post Processors：这个参数主要用于抓取解析过程结束后的扫尾工作，比如将 Extrator 解析出来的 URL 有条件地加入到待处理的队列中去。这里我们使用默认值。
设置完毕后的效果如图 13：

图 13. 设置 Modules
 ![图 13. 设置 Modules
](http://img.blog.csdn.net/20151205174107477)
设置完“Modules”后，点击“Settings”按钮，这里只需要设置 user-agent 和 from，其中：
“@VERSION@”字符串需要被替换成 Heritrix 的版本信息。
“PROJECT_URL_HERE”可以被替换成任何一个完整的 URL 地址。
“from”属性中不需要设置真实的 E-mail 地址，只要是格式正确的邮件地址就可以了。
对于各项参数的解释，可以点击参数前的问号查看。本次任务设置如图 14 所示。

图 14. 设置 Settings
 ![图 14. 设置 Settings](http://img.blog.csdn.net/20151205174120090)
完成上述设置后点击“Submit job”链接，然后回到 console 控制台，可以看到我们刚刚创建的任务处于 pending 状态，如图 15 所示。

图 15. 启动任务
![图 15. 启动任务](http://img.blog.csdn.net/20151205174131723) 
点击“Start”启动任务，刷新一下即可看到抓取进度以及相关参数。同时可以暂停或终止抓取过程，如图 16 所示。需要注意的是，进度条的百分比数量并不是准确的，这个百分比是实际上已经处理的链接数和总共分析出的链接数的比值。随着抓取工作不断进行，这个百分比的数字也在不断变化。

图 16. 开始抓取
 ![图 16. 开始抓取](http://img.blog.csdn.net/20151205174141758)
同时，在 MyHeritrix 工程目录下自动生成“jobs”文件夹，包含本次抓取任务。抓取下来网页以镜像方式存放，也就是将 URL 地址按“/”进行切分，进而按切分出来的层次存储。

-------------------
**一些说明**
尽管 Heritrix 也提供了一些抓取范围控制的类，但是根据实际测试经验，如果想要完全实现自己的抓取逻辑，仅仅靠 Heritrix 提供的抓取控制是不够的，只能修改扩展源代码。

1、创建项目后，Heritrix中报错：sun.net.www.protocol.file.FileURLConnection，原因为sun包是受保护的包，默认只有sun公司的软件才能使用。Eclipse会报错，把对保护使用waring就可以了。
步骤如下：Windows -> Preferences -> Java -> Compiler -> Errors/Warnings-> Deprecated and trstricted API -> Forbidden reference (access rules): -> change to warning
 ![这里写图片描述](http://img.blog.csdn.net/20151205174230760)

2、在进入module配置页，若发现所有的配置可以删除，移动，但是不可以添加和修改，没有可选的下拉框。原因为配置文件找不到，应该在classpath标签页添加配置文件的路径。前面已经提到
即第一部分的第9步。
3、问题： thread-10 org.archive.util.ArchiveUtils.<clinit>() TLD list unavailable
java.lang.NullPointerException
at java.io.Reader.<init>(Unknown Source)
at java.io.InputStreamReader.<init>(Unknown Source)
at org.archive.util.ArchiveUtils.<clinit>(ArchiveUtils.java:759)
解决：将heritrix-1.14.4-src.zip解压中src/resources/org/archive/util中tlds-alpha-by-domain.txt文件复制到工程中org.archive.util包下。
问题：heritrix1.14.4配置-没有add和change按钮的问题
在eclipse中可以启动heritrix，但在jobs->modules.jsp页面中没有添加（“Add”）按扭，且出现以下异常。
              致使错误：“无法编译样式表”              严重 thread-12 org.archive'crawler.framework.WriterPodProcessor.io.arc.......            
 解决办法：将heritrix 项目中的modulse的上一级目录文件添加到eclipse的classpath中。若问题未解决，则把heritrix-1.14.4\src\conf下的modules文件夹移到src文件夹中

-------------------
**Modules中的一些配置项**
在Modules界面中，共有8个选项需要配置，包括以下
1、Crawl Scope
用于配置抓取范围。选项请见下图。
根据名称可以直观的知道抓取范围，默认是BroadScope，即不限制范围。
从下拉框中选择某一项后，点击chang按键，则下拉框上面的解释会相应的发生变化，描述当前选项的特征。
![这里写图片描述](http://img.blog.csdn.net/20151205174250243)
 
2、URI Frontier
用于确定待抓取的url的顺序，亦即相应的算法。默认项为BdbFrontier。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174300859)
3、Pre Processors：Processors that should run before any fetching
在抓取前，处理器对一些先决条件做的判断。比如判断robot.txt等信息，它是整个处理器链的入口。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174313470)
4、Fetchers：Processors that fetch documents using various protocols
指定解释、提取哪些类型的文件
 ![这里写图片描述](http://img.blog.csdn.net/20151205174328072)
5、Extractors：Processors that extracts links from URIs
用于提取当前获取到文件的信息。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174342690)
6、Writers：Processors that write documents to archive files
选择保存的方式，常用的有2种：
org.archive.crawler.writer.MirrorWriterProcessor：保存镜像，即将文件直接下载下来。
org.archive.crawler.writer.ARCWriterProcessor：以归档的格式下载下来，此时文件不可直接查看。此项为default模块的默认选项。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174355521)
7、Post Processors Processors that do cleanup and feed the Frontier with new URIs
抓取后的收尾工作。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174405405)
8、Statistics Tracking
用于一些统计信息。
 ![这里写图片描述](http://img.blog.csdn.net/20151205174416432)

-------------------
**Setting中的一些配置项**
 
修改下载文件的路径
![这里写图片描述](http://img.blog.csdn.net/20151205174423666)