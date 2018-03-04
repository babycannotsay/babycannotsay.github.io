---
title: 【Angular】$parse即时修改ng-model值
date: 2017-03-21 16:56:59
categories: Angular
tags:
---

以下代码自动过滤出input中的数字。

<!--more-->

	scope.$watch(attr['ngModel'], () => {
      let modelValue = $parse(attr['ngModel'])(scope);
      var data = (typeof modelValue === 'string') ? modelValue.replace(/\D+/g, '') : modelValue;
      $parse(attr['ngModel']).assign(scope, data);
	});