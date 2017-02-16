---
title: Gerrit解决冲突
date: 2016-11-14 18:11:38
tags: 
- Gerrit
- git
categories: 技能树
---
# 问题
![](http://7xruee.com1.z0.glb.clouddn.com/gerrit_conflict.png)
# 原因
项目中多人提交代码修改了同一处内容

# 解决方法
## rebase
```bash
git pull --rebase
...产生冲突
...解决冲突
git add .(冲突文件)
git push origin 分支
```

## merge
需要重新commit然后push

待补充
