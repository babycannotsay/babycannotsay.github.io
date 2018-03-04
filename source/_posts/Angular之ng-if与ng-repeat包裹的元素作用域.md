---
title: 【Angular】ng-if与ng-repeat包裹的元素作用域
date: 2017-03-21 16:41:43
categories: Angular
---
ng-repeat与ng-if包裹的元素如

	<div class='parent' ng-if='true'>
		<input type='text' ng-model='name'>
	</div>

<!--more-->

其中input的作用域继承自div.parent所在的作用域，所以可以显示父作用域的name，但是不能改变父作用域的name。若想与父作用域的name相互绑定，需要使用$parent.name。

以上~