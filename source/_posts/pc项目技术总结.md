---
title: 【总结】pc项目技术总结
date: 2018-2-19 11:04:48
categories: "一劳永逸"
tags:
---
## React Router相关 ##
1 路由前判断，从而决定跳转链接

	const requirePermission = (nextState, replace) => {
  		if (!sessionStorage.getItem("username")) {
   		 replace({ pathname: "/login" });
  		}
	};
	
	<IndexRoute
        component={user}
        onEnter={requirePermission}
  	/>
	// 若用户未登录则直接路由到/login，从而避免页面闪屏。

<!--more-->
## dva相关 ##
1 消除url地址中hash后续的随机值

	import { useRouterHistory } from "dva/router";
	import { createHashHistory } from "history";
	const app = dva({
  		history: useRouterHistory(createHashHistory)({ queryKey: false }),
	});

2 loading
	
	import createLoading from "dva-loading";
	app.use(createLoading({ namespace: "loading", effects: true }));
	
	loading: state.loading.effects[`${model}/${effect}`]

3 model->subscriptions->history.listen({pathname}=>do something）

以上pathname在刷新页面如(/login)时会得到两个path,分别为(/login,login)。而在用户点击跳转到login时，只存在一个path(/login)

4 model->effects某个effct中，
	
	yield firstFunc();
	yield secondFunc();
	//如果firstFunc中存在着异步操作，那么可能该异步操作会在secondFunc执行完后才执行结束。

5 可以在Componnet的dispatch中，传递一个回调函数。然后在model的effect相应的方法中，在执行完相关的异步操作后，（通过参数引用，类似payload）引用回调函数。

6 routerRedux跳转链接

	import { routerRedux } from "dva/router";
	...
	yield put(routerRedux.push("/"));

7 按需加载

	import React from 'react';
	import { Router } from 'dva/router';
	
	const cached = {};
	function registerModel(app, model) {
	  if (!cached[model.namespace]) {
	    app.model(model);
	    cached[model.namespace] = 1;
	  }
	}
	
	function RouterConfig({ history, app }) {
	  const routes = [
	    {
	      path: '/',
	      name: 'IndexPage',
	      getComponent(nextState, cb) {
	        require.ensure([], (require) => {
	          cb(null, require('./routes/IndexPage'));
	        });
	      },
	    },
	    {
	      path: '/users',
	      name: 'UsersPage',
	      getComponent(nextState, cb) {
	        require.ensure([], (require) => {
	          registerModel(app, require('./models/users'));
	          cb(null, require('./routes/Users'));
	        });
	      },
	    },
	  ];
	
	  return <Router history={history} routes={routes} />;
	}
	
	export default RouterConfig;
## antd相关 ##
1 `Form`

	const { validateFields, getFieldsValue } = form;
	validateFields((err, values) => {
	    if (!err) {
	      //此处values与通过getFiedsValue获取到的对象的相同
	    }
  	});


2 `YearPicker`

日期选择框年份hack

	const handleOpenChange = (status) => {
	  if (status) {
	    setTimeout(() => {
	      const yearEle = document.querySelector(
	        ".ant-calendar-month-panel-year-select"
	      );
	      yearEle.click();
	      const hackYearCalendar = () => {
	        const yearCells = document.querySelectorAll(
	          ".ant-calendar-year-panel-cell"
	        );
	        const table = document.querySelector('.ant-calendar-year-panel-table');
	
	        const currentYear = new Date().getFullYear();
	        for (let i = 0; i < yearCells.length; i++) {
	          if (yearCells[i].title > currentYear) {
	            yearCells[i].firstChild.style.cssText = "color: #bcbcbc; background: #f5f5f5;";
	            // 官方样式用addEventListener添加的事件，获取不到那个回调函数，也就清空不了事件。所以想到用蒙层覆盖，不让用户点击
	            const mask = document.createElement("div");
	
	            const yearCellWidth = yearCells[i].offsetWidth;
	            const yearCellHeight = yearCells[i].offsetHeight;
	            mask.style.cssText = `position: absolute; cursor: not-allowed; width: ${yearCellWidth}px; height: ${yearCellHeight}px; left: ${i % 3 * yearCellWidth}px; top: ${Math.floor(i / 3) * yearCellHeight}px`;
	
	            table.style.position = "relative";
	            table.appendChild(mask);
	          } else if (i === 0) {
	            // 点击年份九宫格第一格清除初始蒙层，重新调用生成蒙层函数
	            yearCells[i].addEventListener("click", () => {
	              clearMask(table)
	              setTimeout(() => {
	                hackYearCalendar();
	              });
	            });
	          } else if (i === 11) {
	            // 点击年份九宫格最后一格重新调用生成蒙层函数。若最后一格可以点击，必然不存在蒙层。
	            yearCells[i].addEventListener("click", () => {
	              setTimeout(() => {
	                hackYearCalendar();
	              });
	            });
	          } else {
	            // 隐藏月份九宫格，自动点击月份九宫格第一格
	            yearCells[i].addEventListener("click", () => {
	              document.querySelector(".ant-calendar-month").style.opacity = 0;
	              setTimeout(() => {
	                document
	                  .querySelectorAll(".ant-calendar-month-panel-month")[0]
	                  .click();
	              });
	            });
	          }
	        }
	      };
	      document.querySelector('.ant-calendar-year-panel-prev-decade-btn').addEventListener('click', () => {
	        const table = document.querySelector('.ant-calendar-year-panel-table');
	        clearMask(table)
	        setTimeout(() => {
	          hackYearCalendar();
	        })
	      })
	      document.querySelector('.ant-calendar-year-panel-next-decade-btn').addEventListener('click', () => {
	        const table = document.querySelector('.ant-calendar-year-panel-table');
	        clearMask(table)
	        setTimeout(() => {
	          hackYearCalendar();
	        })
	      })
	      hackYearCalendar();
	    });
	  }
    }
	//单线程，事件队列

3 `Modal`


Modal需要给key一个动态的值(`<Modal key={randomKey} />`)，否则打开的模态框在将某个`<Input>`中的值1修改为2，取消操作后，再次打开，该输入框的值仍为2。

4 `Form`

初始值必须为相应元素defaultValue所要求的类型，否则会报错。
如DatePicker的初始值必须为moment类型，多选Select初始值须为Array类型等。


5 `LocaleProvider`

加入`LocaleProvider`作为`router.js`返回的virtual dom的根元素，能避免DatePicker等组件中出现英文的情况。

6 `Button`

 	<Button htmlType="submit" > //default button

>  The button submits the form data to the server. This is the default if the attribute is not specified, or if the attribute is dynamically changed to an empty or invalid value. 
>  
>  [The Button element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#attr-type)

7 `Form`

[自定义的组件如何接收getFieldDecorator的options.initialValue？](https://github.com/ant-design/ant-design/issues/5098)
## 外部库相关 ##


1. [react-vcode](https://github.com/javaLuo/react-vcode)
	一个简单的React验证码组件
1. [moment](http://momentjs.cn/)
	JavaScript 日期处理类库
1. [echarts-for-react](https://github.com/hustcc/echarts-for-react)
	A very simple echarts(v3.0) wrapper for react.
## JavaScript原生相关 ##

1.

> - 保存数据到sessionStorage
>  
> `sessionStorage.setItem('key', 'value');`
> 
> - 从sessionStorage获取数据
> 
> `var data = sessionStorage.getItem('key');`
> 
> - 从sessionStorage删除保存的数据
>  
> `sessionStorage.removeIt1em('key');`
> 
> - 从sessionStorage删除所有保存的数据
> 
> `sessionStorage.clear();`
> 
> ***来源:*** [Window.sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)


2.

> localStorage 与 sessionStorage 相似。不同之处在于，存储在 localStorage 里面的数据没有过期时间（expiration time），而存储在 sessionStorage 里面的数据会在浏览器会话（browsing session）结束时被清除，即浏览器关闭时。
> 
> **ps**: 无论是 localStorage 还是 sessionStorage 中保存的数据都仅限于该页面的协议。
> 
> ***来源：***[Window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)

3.

`...someArray.filter(()=>true)[0].list`中的优先级顺序为`...((someArray.filter(()=>true)[0]).list)`

4.

\`0${month}\`.slice(-2)可以用来处理日期选择2位数的情况. 

## CSS相关 ##
1. rem：指一个元素相对html根元素的font-size的相对大小。

	如html根元素的:`font-size: 16px`， 则`1rem`表示`16px`，`1.2rem`表示`16*1.2=19.2px`，以此类推

1. em：指一个元素相对自身元素的font-size的相对大小。不要因为font-size是可继承的而认为是相对于父元素的font-size的相对大小。
2. calc可以计算%,px,em等值从而得出理想的值。如`calc(100% - 2px + 3em)`;

	**ps**: 操作符两边需要空格隔开。在less中使用时，上述须写为`calc(~'100% - 2px + 3em')`
1. 若padding-top padding-bottom margin-top margin-bottom的值为百分比，则其是相对父元素的宽度而不是高度。

	原因：
> 正常流中的大多数元素都会足够高以包含其后代元素（包括其外边距）。如果一个元素的上下外边距是父元素的height的一个百分数，就可能导致一个无限循环，父元素的height会增加，以适应后代元素上下外边距的增加，而相应地，上下外边距又必须增加，以适应新的父元素的height。
> 
> ***来源：***CSS权威指南 P221


	//用处1：使某个背景图片始终以等比例缩放
	<div class='parent'>
		<div class='image'></div>
	</div>
	.image {
		width: 50%;
		height: 0;
		padding-bottom: 40%;
		background-image: url('xx.jpg');
	}
	//此处xx.jpg的比例始终为宽高5:4，因为width和padding-bottom始终是相对于父元素的宽度计算值的。
	
