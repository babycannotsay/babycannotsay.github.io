---
title: 【Angular】指令scope中的属性=?
date: 2017-06-02 18:15:09
categories: Angular
---
在**angular1.2.3**中，
使用指令中的scope "="双向绑定时，在指令的link中指定了某个属性的值。而如果父作用域的该属性不存在，将抛出NON_ASSIGNABLE_MODEL_EXPRESSION异常。为了避免上述异常，且显示link中指定的属性的值，可以通过使用=?或者=?attr。

例子如下：
<!--more-->

	<body>
	
	<div ng-app="myApp" ng-controller='ctrl'>
		<h2>{{test}}</h2>
		<directive/>
	
	</div>
	
	<script>
	var app = angular.module('myApp', []);
	app.controller('ctrl',function($scope){
		$scope.test = 1;
	});
	app.directive('directive', function(){
		return {
			restrict: 'EA',
			replace: true,
			scope: {
				local: '='
			},
			template: "<h1 >{{local}}</h1>",
			link: function(scope){
				scope.local = 'showMe';
			}
		}
	});
	</script>
	</body>

结果显示如下：
![](http://i.imgur.com/LBiuAIq.png)

	scope: {
		local: '=?'
	},

改为如上所示时，没有异常，且能正常显示。
![](http://i.imgur.com/A6VgBJ1.png)

在angular1.4.6版本时，`?`存在与否都不会报错。

结论： `scope: {prop: '=?'}` 在ng1高版本中应该已经弃用，可以向下兼容低版本。