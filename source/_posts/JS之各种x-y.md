---
title: 【JavaScript】各种x&&y
date: 2017-02-10 21:29:11
categories: JavaScript
---
*以下所列为我个人对X和Y的理解。每次碰到相关的再进行更新，如若有错误，还望指出，不胜感谢！*
<!--more-->

----------

### screenX && screenY

screenX 事件属性可返回事件发生时鼠标指针相对于**屏幕**的水平坐标。

screenY 事件属性可返回事件发生时鼠标指针相对于屏幕的垂直坐标。

![](http://i.imgur.com/CZypSXN.png)

图上的小点即鼠标所在的位置，不固定。

----------

### clientX && clientY

clientX 事件属性返回当事件被触发时鼠标指针向对于浏览器页面（或客户区）的水平坐标。

clientY 事件属性返回当事件被触发时鼠标指针向对于浏览器页面（客户区）的垂直坐标。

客户区指的是当前窗口。

![](http://i.imgur.com/ldQfyS4.png)

图上的小点即鼠标所在的位置，不固定。

### pageX && pageY

pageX 属性返回鼠标指针的位置，相对于文档的左边缘。

pageX 属性返回鼠标指针的位置，相对于文档的上边缘。

	pageX = clientX + scrollLeft;
	pageY = clientY + scrollTop;

scrollTop与scrollLeft的解释见 <a href="/2017/02/10/JS之各种top-left/" target="_blank">JS之各种top&&left(持续更新)</a>