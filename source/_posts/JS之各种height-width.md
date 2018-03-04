---
title: 【JavaScript】各种height&&width
date: 2017-02-10 21:29:11
categories: JavaScript
---
*以下所列为我个人对Height和Width的理解。每次碰到相关的再进行更新，如若有错误，还望指出，不胜感谢！*
<!--more-->

----------

### [inner|outer][Height|Width]
- *window.innerHeight&&window.outerHeight*

![innerHeight&&outerHeight](http://i.imgur.com/9t6ecT0.png)

- *window.innerWidth&&window.outerWidth*

![innerWidth&&outerWidth](http://i.imgur.com/UJmqLMG.png)

----------
### offsetWidth && offsetHeight

某元素ele的盒模型如图，ele.offsetWidth = ele的左右内边距宽度 + ele的左右边框宽度;

图中offsetWidth = 100 + 4 + 4 + 1 + 1 = 110;

![](http://i.imgur.com/4O8xw20.png)
