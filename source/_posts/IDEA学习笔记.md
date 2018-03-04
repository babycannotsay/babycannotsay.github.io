---
title: 【IDEA】学习笔记
date: 2017-02-15 15:59:16
categories: 一劳永逸
---

### 问题及解决
----------
- 问题：编辑好html文件后，使用idea自带的显示在浏览器功能时，提示*Windows找不到文件'Chrome'。请确定文件名是否正确后，再试一次*

	解决：打开idea的设置，在Tools->Web Browers上修改相应浏览器的路径即可。

<!--more-->
----------
- 问题：使用Intellij提交repository

	解决：

	1. 首先安装好Git，并配置好SSH。
	2. 在Settings->Version Control->Git中找到git.exe，然后test一下
	3. 成功后点击VCS->Import into Version Control->Share Project on Github即可。
	4. 

----------
	
- 问题：使用intellij打开公司项目时(包括前后端），跳转到一个界面，界面上显示<span style='color:red;'>JAVA_HOME not defined yet</span>。

	解决：需要下载java sdk。

	方法：登录[oracle官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)。点击**JDK DOWNLOAD**跳转到新的页面。在该页面需要先点击*Accept License Agreement*，然后下载相应版本的jdk即可。下载安装完成后，再回到打开intellij的最初页面。选择`Configure->Project Structure`，选择`Project`。点击`new`，选择刚刚安装好的sdk安装目录。然后再次打开项目，上面的问题应该已经解决了。参考：[Windows 7下java SDK下载、安装及环境变量设置](http://jingyan.baidu.com/article/e5c39bf5a418e439d76033ee.html)

	这时，设置gradle location的时候可能会报错：gradle location is incorrect。

	解决方法：点击右边的`...`找到intellij所在目录，然后点击`plugins->gradle`即可。

----------

- 设置单行注释风格

	Setting->Editor->Code Style->Javascript->Comment Code

----------

- 取消输入自动提示时，输入标点符号，自动补全

	Setting->Editor->General->Code Completion->Insert selected variant by typing dot, space, etc.

### 相关教材
[Intellij IDEA 简体中文专题教材](https://github.com/judasn/IntelliJ-IDEA-Tutorial)