---
title: 【Hexo】学习笔记
date: 2017-02-09 19:43:48
updated: 2018-03-03 13:36
categories: "一劳永逸"
tags:
---
*该文记录学习Hexo的过程，并附带一些问题及解决方案。*
<!--more-->

hexo中文文档：<https://hexo.io/zh-cn/docs/>

Next主题文档：<http://theme-next.iissnan.com/getting-started.html>

Archer主题文档: <https://github.com/fi3ework/hexo-theme-archer>


----------

### 使用hexo碰到的一些问题汇总

- 为什么运行hexo deploy没有反应?

    解决方法：

    
 1. `npm install hexo-deployer-git --save`
 2.  将`_config.yml`中的`deploy`里的`type`设置成`type: git`

----------

- 使用hexo，如果换了电脑怎么更新博客？

    使用coneycode的开源插件:<https://github.com/coneycode/hexo-git-backup>。由于其中解释的并不十分详细，个人尝试了一下后。分享自己的理解。
	
 1. 在_config.yml里配置好backup并保存，我theme没有配置，因为目前对.git的作用并不熟悉，作者也给了个提示，所以没有去尝试了。
 2. backup中的branchName设置成另一个分支的名字，不是master就行。
 3. 然后使用命令`hexo b`，如果repository里，没有该分支，就会自动生成。如果已经存在该分支，新内容会替换里面的内容。新内容是写博客的一些必要文件，如_config.yml,source等。第一次使用该命令会初始化branchName分支，之后使用会更新内容。
 4. 然后要将branchName分支设置成默认分支，以便后续git clone会复制该分支。 
 5. 初始化一个新的hexo文件夹，然后用clone下来的文件去替换掉这个新的hexo里的文件。如此一来就能换电脑更新博客了。注意，替换只是替换其中名字相同的。所以初始化的hello-world.md还是存在的。
 
    注：每次hexo deploy之后，记得用hexo b更新branchName分支。

----------


- hexo server后 打开localhost:4000，显示`cannot get /`

    解决方法：需要先hexo init，生成必要的模块文件