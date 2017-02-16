---
title: Mac Hexo配置
date: 2016-09-06 13:36:12
tags: 
- mac 
- Hexo
categories: 技能树
---
# 为什么要总结

因为之前一直在Windows上配Hexo，也习惯了Windows，最近一个月开始用mac os，开始感觉很不习惯，后来觉得还是很爽的。
然后想在mac上也接着写博客，但是按照网上的一些操作一直没成功，昨天终于成功了，其实原因不是网上的教程问题，而是公司的网络把github屏蔽了，导致clone一些数据不成功，最终导致安装失败。

# 正题
其实这篇文章[在Mac下通过HEXO在Github上搭建博客](http://www.jianshu.com/p/ecd51e8ef2fa)描述的已经很详细了，但是在一些地方会产生歧义，再加上一些自己需要的部分，比如说测试github连通性[网络&权限]。

## 前期准备
### 安装Xcode
Hexo的编译依赖Xcode。这个直接从App Store上下载安装。

### 安装node.js
Hexo是基于node.js的，所以要去官网上下载安装,最新版最好。
<!--more-->
### 注册Github账户
在本地搭建好Hexo后可以将内容同步到github上，可以在网上浏览。
可以去Github官网上去注册，注册的过程我就不罗嗦了，具体的过程可以去这个页面上跳到Github的那部分去看。
其中配置SSH Keys的那部分，可以选择不配制，不配置的话以后每次提交的时候就需要手动输入账号密码，如果配置了的话就不需要了。

### 正式安装
因为安装包中有些内容被墙，可以用如下命令
```node
npm install hexo --no-optional
```
不加--no-optional可能会导致如下错误：
```
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```
如果仍然又上述问题，
可以通过重装hexo或者hexo-cli解决
```bash
npm uninstall hexo
npm install hexo --no-optional
```
```bash
npm uninstall hexo-cli -g
npm install hexo-cli -g
```
来安装
进入你要安装的目录，如
```bash
cd ~/Document/hexo
```
然后安装
```bash
hexo init
```
安装好之后不要忘记执行
```bash
npm install
```
**重点**不要忘记,这个很重要
```bash
npm install hexo-deployer-git --save
```
### 后期部署
添加文章
```bash
hexo new "postName"
```
* postName是文章名字

生成静态页面
```bash
hexo generate
```
可以缩写
```bash
hexo g
```
本地启动
执行好上面的命令之后就可以在本地启用服务来看效果了。执行下面的命令：
```bash
hexo sever
```
或缩写
```bash
hexo s
```
看到 INFO Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop. 之后，就可以在浏览器中打开页面http://localhost:4000测试

### 上传至Github
安装git部署插件
在部署之前，首先我们要确认在你的Github帐号的Repository中有 用户名.github.io 的项目。
在确认之后，就可以执行命令
```bash
npm install hexo-deployer-git --save
```
安装插件

### 配置 _config.yml 文件
在Hexo安装的目录，如 ~/Document/hexo 中找到 _config.yml 文件。打开。
翻到最后，找到 deploy 字样，修改为如下格式：
```
deploy: 
  type: git 
  repo: https://github.com/用户名/用户名.github.io.git 
  branch: master
```
需要注意的是：冒号后面有一个空格；使用github可以不用写branch那一行。
如果要使用多个 deployer，修改为如下格式：
```
deploy:
- type: git
  repo:
- type: heroku 
  repo:
```