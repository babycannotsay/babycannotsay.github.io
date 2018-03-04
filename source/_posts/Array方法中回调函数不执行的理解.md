---
title: 【JavaScript】Array方法中回调函数不执行的理解
date: 2017-07-04 09:46:28
categories: JavaScript
---


	var arr = new Array(2); 
	/*	
		console.log(arr)
		
		Chrome下 => [undefined * 2]
		Firefox下 => Array [ <2 个空的存储位置> ]
	*/
	arr.map((value,index)=>console.log(index));

在控制台不会输出index的值，即不会执行map中的函数。同理，涉及到Array中有需要回调函数的其他方法，如forEach，也不会执行其回调函数。
<!--more-->
相同的情况还有，

	var arr = [1,2];
	arr[3] = undefined;

遍历arr时，设其回调函数为

	(value,index) => console.log(value);

也只会输出`1, 2, undefined`。也就是说虽然`arr.length = 4`;
但是实际上回调函数只执行了3次。

***`delete`***

	var willDeleteArr = [1,2,3];
	delete willDeleteArr[1];
	willDeleteArr.forEach((v) => console.log(v)); //1, 3

### 原因（个人理解）如下： ###
因为数组本质上是一个对象，所以可以使用对象的方法hasOwnProperty。而

	arr.hasOwnProperty('3'); //true
	arr.hasOwnProperty('2'); //false
	arr.hasOwnProperty('1'); //true
	arr.hasOwnProperty('0'); //true

再结合
	
	var obj = {
		x: undefined,
		y: 1
	}
	obj.hasOwnProperty('y'); //true
	obj.hasOwnProperty('x'); //true
	obj.hasOwnProperty('z'); //false

而for in循环obj，显然也只会执行2次。上述数组的情况，现在也说得通了。

***ps***: 另外，在JSX中，Array.proptotype.map中只接受前面2个参数`(value,index)`，而不包括第三个参数原数组。
	