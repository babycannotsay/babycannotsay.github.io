---
title: 【JavaScript】非Array调用Array原型链上的方法
date: 2017-02-20 18:12:50
categories: JavaScript
---
要使用call或者apply调用Array原型链上的方法，通常这个对象需要满足2个条件就可以了

1. 本身可以存取属性
2. length属性不能是只读（可以没有length属性）。

注：对象/数组的length，如果为null或者undefined，甚至没有length属性，都会设置length = 0。function的length表示形参的个数，是个只读属性。

	var a = {};
	a[0] = 1;
	a[1] = 2;
	Array.prototype.push.call(a,3);
	a.length;	//1

	var a = {};
	a[0] = 1;
	a[1] = 2;
	a.length = 2;
	Array.prototype.push.call(a,3);
	a.length;	//3
	

