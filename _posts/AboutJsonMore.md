---
title: Json相关总结
date: 2016-08-08 20:31:08
tags: 
- Android
- Json
categories: 技能树
---

# Json相关的
这里不是Json处理和生成的总结，而是和Json相关的其他的一些小的技巧
## 插件类
### **Chrome**
JSONView

postman
![](http://7xruee.com1.z0.glb.clouddn.com/postman.png)
DHC
![](http://7xruee.com1.z0.glb.clouddn.com/DHC.png)

### **AndroidStudio** 
GsonFormat
![](http://7xruee.com1.z0.glb.clouddn.com/GsonFormat.png)
![](http://7xruee.com1.z0.glb.clouddn.com/GsonFormat1.png)
### **SublimeText** 
Pretty JSON
<!--more-->
步骤
1. Package Control安装
```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
Ctrl+\`
输入上述代码，重启SublimeText；
2. 通过快捷键 Ctrl+Shift+P 打开Package Control来安装插件了。在打开的输入框中输入 install ，会根据你的输入自动提示，选择 Install Package
输入Pretty JSON即可安装。