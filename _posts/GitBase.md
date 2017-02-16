---
title: Git基础
date: 2016-07-24 12:45:00
tags: 
- git
categories: 技能树
thumbnail: http://img3.douban.com/lpic/s9122139.jpg
---
> 0706更新[推荐崔庆才的一篇博文](http://cuiqingcai.com/297.html)
0708添加reset和revert的区别(见文章底部)
0724添加merge、rebase和stash命令
0801[推荐backlogtool上的git-guide](http://backlogtool.com/git-guide/cn/)

[在线《Pro Git》书的链接](http://iissnan.com/progit/)
<!--more-->
# 前言
Git是目前世界上最先进的分布式版本控制系统。
## SVN与Git的最主要的区别
SVN是集中式版本控制系统，版本库是集中放在中央服务器的，开始工作时首先从中央服务器那里得到最新的版本，工作结束后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工。
Git是分布式版本控制系统，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。
## 安装Git
从https://git-scm.com/download/下载，然后进行默认安装即可。安装完成后，在开始菜单里面找到 “Git –> Git Bash”
配置用户信息
第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：
git config --global user.name "lwq"
git config --global user.email "name@example.com"
如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。
<!--more-->
> [详细内容参见该链接](http://iissnan.com/progit/html/zh/ch1_4.html)

# 深入   
## 理解工作区（Working Dictionary）与暂存区（Staging Area）的区别

git repo 的三大 components，分别是 working directory(代码仓库) staged snapshot(快照:add的缓存库) commit history(commit历史) 
使用Git提交文件到版本库有两步：
1. 使用git add 把文件添加进去，实际上就是把文件添加到暂存区。
2. 使用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支上。
创建与合并分支命令如下：
查看分支：git branch
创建分支：git branch name
切换分支：git checkout name
创建+切换分支：git checkout –b name
合并某分支到当前分支：git merge name
删除分支：git branch –d name

## git revert&git reset

传递给 git reset 和 git checkout的参数会决定命令的作用范围。当命令并不包括含一个文件路径时，命令作用于整个 commit
### commit 级别的操作
* Reset

在 commit 级别上，git reset 命令移动 HEAD 到当前分支的一个 commit， 这可以用来撤销当前分支的一些 commit 。

例如，下面的命令会让'dev'分支回退两个commits
```bash
git checkout dev
git reset HEAD~2
```
先前在 HEAD 之前的两次 commit 现在**处在 HEAD 之后，这意味着他们在下一次 git 提交时被作为垃圾删掉，换句话说这两次提交会被抛弃。**
git reset用于撤销未被提交到远端的改动。除了可以移动当前分支的HEAD，你可以通过不同的标记选择修改 staged snapshot 或者 working directory

--soft： staged snapshot 和 working directory 都未被改变 (建议在命令行执行后，再输入 git status 查看状态)

--mixed： staged snapshot 被更新， working directory 未被更改。**这是默认选项**(建议同上)

--hard： staged snapshot 和 working directory 都将回退。

--hard 很危险，它会直接回退你之前所有的修改，使用前，可以事先保存 commit id.

* Revert

git revert 命令通过创建一次新的 commit 来撤销一次 commit 所做出的修改。这种撤销的方式是安全的，因为它并不修改 commitm history, 比如下边的命令将会查出倒数第二次（即当前commit的往前一次）提交的修改，并创建一个新的提交，用于撤销当前提交的上一次 commit。

git checkout dev
git revert HEAD~2

### File 级别的操作
Reset:

当调用一个文件路径时，git reset 命令会更新 staged snapshot 去匹配某次 commit。 下面的命令将会使文件回退一个 commit。

git reset HEAD~1 ［文件］(不建议使用)

### 总结
1. git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。 
![](http://backlogtool.com/git-guide/cn/img/post/stepup/capture_stepup7_2_2.png)
2. 在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git revert是用一次逆向的commit“抵消”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，但是git reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入。 
3. git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。
![](http://backlogtool.com/git-guide/cn/img/post/stepup/capture_stepup7_3_2.png)

## The relationship between HEAD / Working Tree / Index in Git
![](http://i.stack.imgur.com/caci5.png)
HEAD指向的是现在使用中的分支的最后一次更新。通常默认指向master分支的最后一次更新。通过移动HEAD，就可以变更使用的分支。它也是我们最初从仓库的那个分支开始的指针，就是我们checkout的地方，也是下一个commit的父指针。HEAD是我们理解分支的一个重要概念。对应于git中HEAD就是我们上次commit的地方，当我们再次commit的时候，git会移动HEAD到最新的地方。对于两个不同的分支也是一样的情况，始终指向当前分支的开始工作的地方。

## 修改最近提交

指定--amend选项执行提交的话，可以修改同一个分支最近的提交内容和注解。
修改最近的提交主要使用的场合：

* 添加最近提交时漏掉的档案
* 修改最近提交的注解

## 取消过去的提交

在revert可以取消指定的提交内容。使用rebase -i(可以改写、替换、删除或合并提交)或reset也可以删除提交。但是，不能随便删除已经发布的提交，这时需要通过revert创建要否定的提交。
主要使用的场合：

* 安全地取消过去发布的提交

## 合并修改记录
在执行pull之后，进行下一次push之前，如果其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝。

这种情况下，在读取别人push的变更并进行合并操作之前，你的push都将被拒绝。因为，如果不进行合并就试图覆盖已有的变更记录的话，其他人push的变更就会丢失。

### git merge
![](http://my.csdn.net/uploads/201206/14/1339682845_9921.jpg)
合并当前分支到master分支时，如果master分支的状态没有被更改过，那么这个合并是非常简单的。当前分支的历史记录包含master分支所有的历史记录，所以只要把当前分支移动到master分支就可以导入当前分支的内容了。这样的合并被称为**fast-forward（快进）合并**。

但是，master分支的历史记录有可能在当前分支分叉出去后有新的更新。这种情况下，要把master分支的修改内容和当前分支的修改内容汇合起来。
因此，合并两个修改会生成一个提交。这时，master分支的HEAD会移动到该提交上。
**执行合并时，如果设定了non fast-forward选项，即使在能够fast-forward合并的情况下也会生成新的提交并合并。**
### git rebase
![](http://my.csdn.net/uploads/201206/14/1339682915_7495.jpg)
历史记录简单，是在原有提交的基础上将差异内容反映进去。
因此，可能导致原本的提交内容无法正常运行。
![](http://backlogtool.com/git-guide/cn/img/post/stepup/capture_stepup1_4_8.png)
![](http://backlogtool.com/git-guide/cn/img/post/stepup/capture_stepup1_4_9.png)
### 解决冲突

如果远程数据库和本地数据库的同一个地方都发生了修改的情况下，因为无法自动判断要选用哪一个修改，所以就会发生冲突。
Git会在发生冲突的地方修改文件的内容，如下图。所以我们需要手动修正冲突。
![](http://7xruee.com1.z0.glb.clouddn.com/conflict.png)
**==分割线上方是本地仓库的内容, 下方是远程仓库的编辑内容。**

在rebase的过程中，也许会出现冲突(conflict). 在这种情况，Git会停止rebase并会让你去解决 冲突；在解决完冲突后，用"git-add"命令去更新这些内容的索引(index), 然后，你无需执行 git commit,只要执行:
$ git rebase --continue
这样git会继续应用(apply)余下的补丁。
在任何时候，你可以用--abort参数来终止rebase的行动，并且"mywork" 分支会回到rebase开始前的状态。
$ git rebase --abort

## git stash
git stash命令可用来暂存当前正在进行的工作，比如想pull 最新代码，又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。
git stash  对当前的暂存区和工作区状态进行保存。 
git stash list  列出所有保存的进度列表。 
git stash pop [--index] [<stash>] 恢复工作进度
