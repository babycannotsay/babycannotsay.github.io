---
title: 【Angular】初探bindToController
date: 2017-02-27 10:13:56
categories: Angular
tags:
---
- #### bindToController属性介绍

	默认false。这个属性用来绑定scope的属性直接赋给controller。可以为true或者和scope相同格式的对象。此外使用此属性，要设置controller的别名，通常通过"controllerAs"来设置。如果一个directive里同时使用了bindToController和scope，并且是object。那么bindToController会覆盖scope。

<!--more-->

- #### 代码

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="https://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body ng-app='testDirectiveBindToController' ng-controller="testDirectiveBindCtr">  
			<span>标题:</span> <input ng-model="title"><br>  
			<span>内容:</span> <input ng-model="content"><br>  
			<!--受testDirectBinCtr的影响，包括内容4改变testDirectBinCtr造成的影响-->
			<h3>内容1:没有用bindToController的情况</h3>  
			<test-directive-none-bind title={{title}} content="{{content}}"></test-directive-none-bind>  
		
			<!--只受自身controller的影响-->
			<h3>内容2:bindToController＝true,且不设置标签属性的情况</h3>  
			<test-directive-simple-bind></test-directive-simple-bind>  
			
			<!--受testDirectBinCtr的影响也受自身controller的影响，但不能影响testDirectBinCtr-->
			<h3>内容3:bindToController＝true,且设置标签属性的情况</h3>  
			<test-directive-simple-bind title="{{title}}"content="{{content}}"></test-directive-simple-bind>  
			
			<!--内部controller的属性与testDirectBinCtr的属性互相影响-->
			<h3>内容4:bindToController＝obj,设置标签属性,设置scope对象，但没有bindToController的权限高</h3>  
			<test-directive-obj-bind title="title" content="content"></test-directive-obj-bind>  
	
			<!-------------------------------------------script---------------------------------------->
	
			<script>
				var app = angular.module("testDirectiveBindToController", []);  
			
				app.controller("testDirectiveBindCtr", function ($scope) {  
					$scope.title = "默认标题";  
					$scope.content = "默认内容";  
				}).directive("testDirectiveNoneBind", function () {  
					var dir = {};  
					dir.restrict = "E";  
					dir.replace = true;  
					dir.scope = {  
					    title: "@",  
					    content: "@"  
					};  
					dir.template = "<div>我的标题是:{{title}}" +  
					    "<div>我的内容是:{{content}}</div></div>";  
					return dir;  
				}).directive("testDirectiveSimpleBind", function () {  
					var dir = {};  
					dir.bindToController = true;  
					dir.restrict = "E";  
					dir.replace = true;  
					dir.controllerAs = "ctr";  
					dir.scope = {  
					    title: "@",  
					    content: "@"  
					};  
					dir.controller = function () {  
					    this.title = "controller中设置标题1";  
					    this.content = "controller中设置内容1";  
					    this.onChangeValue = function () {  
					        this.title = "绑定为true改变的标题";  
					        this.content = "绑定为true改变的内容";  
					    }  
					};  
					dir.template = "<div>我的标题是:{{ctr.title}}" +  
					    "<div>我的内容是:{{ctr.content}}</div>"+  
					    "<button ng-click='ctr.onChangeValue()'>改变标题和内容</button></div>";  
					return dir;  
				}).directive("testDirectiveObjBind", function () {  
					var dir = {};  
					dir.bindToController =true;  
					dir.restrict = "E";  
					dir.replace = true;  
					dir.controllerAs = "ctr";  
					dir.scope = {  
					    title: "@",  
					    content: "@"  
					};  
					dir.bindToController ={  
				        title: "=",  
				        content: "="  
				    };  
					dir.controller = function () {  
					    this.title = "controller中设置标题";  
					    this.content = "controller中设置内容";  
					    this.onChangeValue = function () {  
					        this.title = "绑定为obj改变的标题";  
					        this.content = "绑定为obj改变的内容";  
					    }  
					};  
					dir.template = "<div>我的标题是:{{ctr.title}}" +  
					    "<div>我的内容是:{{ctr.content}}</div>"+  
					    "<button ng-click='ctr.onChangeValue()'>改变标题和内容</button></div>";  
					return dir;  
				});  
			</script>
		</body>  
		
		</html>

#### 结论
1. controllerAs中设置的是'testDirectBinCtr'的别名。
2. $scope里的属性比指令中controller的属性优先级更高。
3. bindToController用来绑定scope的属性直接赋给controller,不过要考虑单向绑定，双向绑定与标签中是否有相应属性的情况。
4. 如果一个directive里同时使用了bindToController和scope，并且是object。那么bindToController会覆盖scope。

#### 参考
<http://www.cnblogs.com/mafeifan/p/5834413.html>
<http://blog.csdn.net/liyanq528/article/details/53815929>
