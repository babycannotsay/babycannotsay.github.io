---
title: 【ramda】函数redux假想使用场景
date: 2017-05-17 16:20:49
categories: 一劳永逸
tags:
---
因为ramda库中的函数都是curry化的，基于此，记录假想使用场景以备之后使用。

- add 计数

- addIndex(map) 等价于 Array.prototype.map 

- adjust 修改数组中某一项的值 <span style="color:red;">Array</span>

- all 等价于 Array.prototype.every <span style="color:red;">Array</span>

<!--more-->

- allPass 检测某对象是否存在某些属性 <span style="color:blue;">Object</span>

- always 等价于 const

- and 等价于 !!(a&&b)

- any 等价于 !!(a||b) <span style="color:red;">Array</span>

- anyPass 等价于*.prototype.some

- ap 将**函数列表**作用于值列表上。 <span style="color:red;">Array</span>

- aperture 将数组拆分成指定个数的子数组 <span style="color:red;">Array</span>

- append 等价于[...arr, val]

- apply 等价于func(...arr)

- applySpec 生成函数关联的属性的对象 <span style="color:blue;">Object</span>

- ascend 比较指定数值/属性，升序

- assoc 等价于Object.assign <span style="color:blue;">Object</span>

- assocPath 设置指定对象树 <span style="color:blue;">Object</span>

- binary 将任意元函数封装为二元函数

- **bind** 未理解不会绑定额外参数含义 

- both 等价于a()&&b() <span style="color:green;">Function</span>

- call 提取第一个参数作为函数，其余参数作为刚提取的函数的参数，调用该函数并将结果返回。

- chain(fn,arr) 将函数映射到列表中每个元素，并将结果连接起来。
map(fn,arr);
若chain(fn1,fn2,arr)，则fn1(fn2(arr))(arr)。 <span style="color:red;">Array</span>

- clamp 将数值限制在指定的范围内

- clone 深复制。only Function通过引用复制

- comparator 根据判断函数的比较符号排序，如<则升序。

- complement 对函数的返回值取反。 <span style="color:green;">Function</span>

- compose 从右往左执行函数组合（右侧函数的输出作为左侧函数的输入）。最右侧函数可以是任意元函数（参数个数不限），其余函数必须是一元函数。

- composeK