---
title: 【Angular】基于ng-repeat，使用自定义指令给子元素添加属性的问题
date: 2017-03-14 10:08:44
categories: Angular
tags:
---
本笔记对在使用自定义指令添加属性时，出现的问题做个记录。至于为何要通过指令添加属性，主要是因为添加的属性也是可以作为指令的，以此将指令应用到更多的标签中。

<!--more-->

#### 指令代码

	app.directive('addFilterCharacterToInput', function () {
      return {
          link(scope, ele, attr) {
              ele.find('input').attr('filter-char', '');
          }
      }
	});

#### 可能结果图示

属性添加成功时，如下图
![](http://i.imgur.com/ipdsC7G.png)

属性添加不成功时，如下图
![](http://i.imgur.com/XjDtKTz.png)

主要区别就是，input标签中有没有多出filter-char属性

#### HTML代码

1. 不使用任何ng指令

		<div add-filter-character-to-input>
			<div>
				<input type='text'>
			</div>
		</div>

	结果：属性添加成功

2. 使用一个ng-repeat指令

		
	    <div ng-repeat="data in dataset"  add-filter-character-to-input>
	      <span><input type="text" ng-model="data"></span>
	    </div>
	 
	结果：属性添加成功

3. 使用2个ng-repeat指令

	- add-filter-character-to-input添加到最上层的ng-repeat
		
			<div ng-repeat="data in dataset" add-filter-character-to-input>
			    <div ng-repeat="d in data">
			      <span ><input type="text" ng-model="d"></span>
			    </div>
		  	</div>
		结果：属性添加失败

	- add-filter-character-to-input添加到次层的ng-repeat

			<div ng-repeat="data in dataset">
			    <div ng-repeat="d in data" add-filter-character-to-input>
			      <span ><input type="text" ng-model="d"></span>
			    </div>
		  	</div>
		结果：属性添加成功

4. 使用一个ng-repeat以及一个ng-if指令

		<div ng-repeat="data in dataset" add-filter-character-to-input>
		    <span ng-if="true"><input type="text" ng-model="data[0]"></span>
		</div>
	结果：属性添加失败

#### 思考及解决

思考：姑且把最外层所在的div称作大爷，其ng-repeat生成的子元素称作儿子，儿子用ng-repeat生成或者ng-if包裹的子元素称作孙子。

大爷有一股快要消失的力量想要传给孙子，但是当时孙子还没出生，而孙子出生的时候力量已经消失了，所以孙子没能得到这股力量。

解决1. 给两个调用同一个ngModel值的其中一个添加指令属性，该指令属性也会影响另一个，从而达到相同的效果。很牵强。