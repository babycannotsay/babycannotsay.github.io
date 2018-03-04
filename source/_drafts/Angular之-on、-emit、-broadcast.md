---
title: 【Angular】$on、$emit、$broadcast
date: 2017-03-09 11:23:16
categories: Angular
tags:
---

- $on

	用于监听事件

- $emit

	用于向上分发（子作用域向父作用域）
<!--more-->
- $broadcast

	用于向下广播（父作用域向子作用域）

示例代码：

#### html

	<div  ng-app='myApp' ng-controller='GrandParentCtrl'>
		<div ng-controller="ParentCtrl">                <!--父级-->
			<div ng-controller="SelfCtrl">              <!--自己-->
				<button ng-click="click()">click me</button>
				<div ng-controller="ChildCtrl">
					<div ng-controller='GrandChildCtrl'></div>
				</div>   <!--子级-->
			</div>
			<div ng-controller="BroCtrl"></div>         <!--平级-->
		</div>
	</div>

#### js

	var app = angular.module('myApp', []);
	app.controller('SelfCtrl', function($scope) {
		$scope.click = function () {
			$scope.$broadcast('to-child', 'child');
			$scope.$emit('to-parent', 'parent');
		}
	});
	
	app.controller('GrandParentCtrl', function($scope) {
	
		$scope.$on('to-parent', function(event,data) {
			console.log('GrandParentCtrl', data);	   //父级能得到值
		});
		$scope.$on('to-child', function(event,data) {
			console.log('GrandParentCtrl', data);	   //子级得不到值
		});
	});
	app.controller('ParentCtrl', function($scope) {
	
		$scope.$on('to-parent', function(event,data) {
			console.log('ParentCtrl', data);	   //父级能得到值
		});
		$scope.$on('to-child', function(event,data) {
			console.log('ParentCtrl', data);	   //子级得不到值
		});
	});
	
	app.controller('ChildCtrl', function($scope){
		$scope.$on('to-child', function(event,data) {
			console.log('ChildCtrl', data);		 //子级能得到值
		});
		$scope.$on('to-parent', function(event,data) {
			console.log('ChildCtrl', data);		 //父级得不到值
		});
	});
		app.controller('GrandChildCtrl', function($scope){
		$scope.$on('to-child', function(event,data) {
			console.log('GrandChildCtrl', data);		 //子级能得到值
		});
		$scope.$on('to-parent', function(event,data) {
			console.log('GrandChildCtrl', data);		 //父级得不到值
		});
	});
	
	app.controller('BroCtrl', function($scope){  
		$scope.$on('to-parent', function(event,data) {  
			console.log('BroCtrl', data);		  //平级得不到值  
		});  
		$scope.$on('to-child', function(event,data) {  
			console.log('BroCtrl', data);		  //平级得不到值  
		});  
	});

#### 输出：
	
	ChildCtrl child  
	GrandChildCtrl child  
	ParentCtrl parent  
	GrandParentCtrl parent

#### 结论：


1. $emit用于向上分发，但不局限于父作用域，爷爷辈甚至更高的作用域都能通过$on监听到；

1. $broadcast用于向下广播，但不局限于子作用域，孙子辈甚至更低的作用域都能通过$on监听到。

#### 参考

[AngularJS的学习--$on、$emit和$broadcast的使用](http://www.tuicool.com/articles/qIBNve)