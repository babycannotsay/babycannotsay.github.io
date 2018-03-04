---
title: 【HTML】多个img标签的间距问题
date: 2017-02-15 16:14:30
categories: HTML
---
*问题*：两个连续的img标签以回车的方式放置时，如

	<img src='1.png'>
	<img src='2.png'>

就算img标签的padding,margin,border均为0，两张图片也存在着一定间距。

*解决*： 去掉两个标签之间的回车，即让标签处于同一行即可。

*尾话*： 若以回车的方式排列img标签，就算设置了margin,padding,border等属性，也会有上述影响。