---
title: 【Git】学习笔记
date: 2017-02-10 16:39:34
categories: "一劳永逸"
tags:
---
*该文记录在[廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "Git教程")的学习过程*
<!--more-->
## 配置用户和Email
系统上安装Git后，需要在命令行内输入：

	git config --global user.name "Your Name"
	git config --global user.email "email@example.com"

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
<!--more-->
## 创建版本库
### 初始化仓库(把这个目录变成Git可以管理的仓库)

	git init

### 把文件添加到仓库

	git add <file> (<file>)*

强制添加

	git add -f <file>

### 把文件提交到仓库
	
	git commit -m "explain"

可以先由add添加多个文件，然后由commit一次提交很多文件

	git add file1.txt
	git add file2.txt file3.txt
	git commit -m "add 3 files."

## 版本控制
### 查看仓库当前的状态

	git status

### 查看修改内容
	
	git diff

### 显示从最近到最远的提交日志

每提交一次都是一个新的版本

	git log

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数

### 回退到上一个版本
**版本回退**需要知道当前版本是哪个版本。Git中用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

	git reset --hard HEAD^

此时git log看不到未回退前的版本，通过前面git log中的命令行找到对应版本号，可以回到未回退前的版本。

	git reset --hard 版本号（不必写全，前几位就行）

### 查看命令历史

	git reflog

### 工作区和暂存区
![工作区和暂存区](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

工作区未add，status(untracked)

add后，工作区(copy)->暂存区

commit后，暂存区(cut)->master

### 查看master和工作区的区别

	git diff HEAD -- xx.txt

### 撤销工作区修改

	git checkout -- xx.txt

本质是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”，让文件回到最近一次git commit或git add时的状态

### 撤销暂存区的修改，重新放回工作区

	git reset HEAD xx.txt

### 删除文件
	git rm
	git commit -m "explain"

## 远程仓库
### 关联远程仓库

	git remote add origin git@github.com:babycannotsay/git_repo.git

远程仓库的名称是origin

### 推送本地内容至远程库

	git push -u origin master

第一次推送master分支时，加上`-u`关联本地和远程的master分支，之后使用`git push origin master`推送

### 克隆远程仓库至本地

	git clone git@github.com:babycannotsay/git_repo.git

## 分支管理
### 创建并切换到分支

	git checkout -b <name>

相当于

	git branch <name>(创建分支)
	git checkout <name>(切换分支)
### 查看当前分支

	git branch

git branch命令会列出所有分支，当前分支前面会标一个*号

### 合并分支到当前分支
	
	git checkout master
	git merge <name>

若dev分支与master分支均修改了xx.txt，合并时会有冲突，需要手动修改xx.txt再提交

### 删除分支

	git branch -d <name>

### 查看分支合并图

	git log --graph

### Fast forward模式
删除分支，会丢失分支信息

禁用Fast forward模式

	git merge --no-ff -m "merge with no-ff" dev

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

### 储存工作区

	git stash

查看工作区内容

	git stash list

恢复工作区

`git stash apply`恢复，不删除stash内容， 使用`git stash drop`删除。`git stash pop`恢复加删除 

`git stash apply <name>`恢复指定工作区

### 丢弃一个没有被合并过的分支

	git branch -D <name>

### 查看远程库信息

	git remote

查看详细信息

	git remote -v 

### 多人协作

1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果`git pull`提示“`no tracking information`”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

## 标签管理

### 创建标签
首先，切换到需要打标签的分支上。

	git branch
	git checkout master

创建标签

	git tag <name>

查看所有标签
	git tag

默认标签是打在最新提交的commit上的

创建指定commit的标签

	git tag <name> <commit-id>

查看标签信息

	git show <tagname>

创建带有说明的标签，用-a指定标签名，-m指定说明文字

	git tag -a <tagname> -m "explain" <commit-id>

**标签是指向commit的死指针，分支是指向commit的活指针**

### 删除标签
	git tag -d <tagname>
删除远程标签

先从本地删除
`git tag -d <tagname>`

再从远程删除
`git push origin :refs/tags/<tagname>`
### 推送标签
	git push origin <tagname>

推送全部标签

	git push origin =--tags

## 自定义Git
忽略某些文件时，需要编写.gitignore

.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理
### 检测gitignore规则
	git check-ignore -v App.class
### 配置别名
	git config --global alias.st status

## 参考
[廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "Git教程")

## 相关

[Git for Windows安装和基本设置](http://www.cnblogs.com/vitah/p/3612473.html)