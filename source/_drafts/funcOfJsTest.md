---
title: funcOfJsTest
tags:
---
	function testTime(a,times) {
		var start = Date.now();
		var stop = 0;
	    for(var i = 0; i < times; i++) {
	    	var x = a;
	    }
		stop = Date.now();
		return stop - start;
	}
	function test(fn,a1,a2,time) {
		for(var i = 0; i < 10; i++) {
			console.log(fn.call(null,a1,time),fn.call(null,a2,time));
	    }
	}