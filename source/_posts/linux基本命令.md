---
title: 【linux】基本命令
date: 2017-02-10 16:53:13
categories: 一劳永逸
tags:
---
以下仅摘记所遇有关Linux命令
<!--more-->
1. mkdir xx
创建名为xx的文件夹
2. cd xx
跳转到xx目录
3. pwd
显示当前目录路径
4. ls 
	- -l 列出文件详细信息(list)
	- -a 	列出文件下所有的文件，包括以“.“开头的隐藏文件（linux下文件隐藏文件是以.开头的，如果存在..代表存在着父目录）
5. cat xx.txt
显示当前文件内容
6. rm xx.txt 
	- -r 递归删除，可删除子目录及文件
	- -f 强制删除 
删除文件
7. vim xx.txt
（创建）编辑文件
Esc : wq 保存退出
8. touch 
创建空文件
9. cp sourceFile targetFile
10. find targetFile
在文件系统中搜索某文件
11. wc targetFile
统计文本中函数、字数、字符数
12. grep str targetFile
在文本文件中查找某个字符串，显示存在该字符串的多行内容
13. rmdir targetDir
删除空目录