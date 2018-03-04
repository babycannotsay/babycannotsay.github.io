---
title: 【JavaScript】各种top&&left
date: 2017-02-10 22:03:30
categories: JavaScript
---
*以下所列为我个人对Top和Left的理解。每次碰到相关的再进行更新，如若有错误，还望指出，不胜感谢！*
<!--more-->

----------

### offsetTop && offsetLeft

要清楚offsetTop和offsetLeft的值，就必须明白所求元素对应的父元素是哪个。以下测试均在Chrome上进行。

- 没有父元素被定位。

    假设body的margin-top为x,body的第一个子元素bodyFirstChild的margin-top为y。
	计算bodyFirstChild的offsetTop时,经测试存在以下情况：

 - 若 `y > x`，则使用 `y` 作为margin-top;
 - 若 `0 < y <= x`，则使用` x` 作为margin-top;
 - 若 `y < 0`，则使用 `x+y` 作为margin-top;
 
    确定相对于html的margin-top之后,后续offsetTop按照正常思维计算即可,若定位方式是相对定位，并设置了top，还要再加上相应元素的top。。

    `bodyFirstChild.offsetTop = body的上边框宽度 + body的上内边距宽度 + margin-top;`

	`bodyFirstgrandchild = bodyFirstChild.offsetTop + bodyFirstChild的上边框宽度 + bodyFirstChild的上内边距宽度 + bodyFirstgrandchild的上外边距宽度；`后续类推；**如下图**
	![](http://i.imgur.com/MKDSNG2.png)

	`secondChild.offsetTop = firstChild.offsetTop + bodyFirstChild的高度 + bodyFirstChild的上下内边距宽度 + bodyFirstChild的上下边框宽度 + max(firstChild的下外边距宽度,secondChild的上外边距宽度);`后续类推；**如下图**

    ![](http://i.imgur.com/YOXDlqM.png)
	
    >结论：没有父元素被定位时，offsetTop的计算是相对于html来说的。


- 父元素被定位

    ![offsetTop](http://i.imgur.com/7OIBvNH.png)

	>`child.offsetTop = child.style.marginTop + child.style.top;`

	注：上式child.style的属性未设置时，读取均为""，以上公式仅为理解。


>结论：*没有父元素被定位*的情况其实就相当于 *父元素被定位*时，父元素为html的情况。

offsetLeft与offsetTop同理，不再重述。

----------
### scrollTop && scrollLeft

根据[stackoverflow](http://stackoverflow.com/questions/19618545/body-scrolltop-vs-documentelement-scrolltop-vs-window-pagyoffset-vs-window-scrol)上网友的解答,最好将scrollTop封装为:

	xx.scrollTop = (()=>window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0
	)();

此属性返回当前页面向上滚动的距离,也就是页面顶部距离浏览器视口上边缘的距离。下面附上图片解释.

如图,初始未下拉时,浏览器视口上边缘距绿字`I`的距离为160px

![](http://i.imgur.com/6YpFDz2.png)

下拉至距离为46px时,如图

![](http://i.imgur.com/4MT9xo9.png)

此时`xx.scrollTop=160px-46px=114px;`