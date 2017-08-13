---
title: 我理解的 this
date: 2017-08-13 13:52:39
tags:
---
- 在普通函数中`this` 的指向是在运行的时候决定的，而不是定义的时候。
- 在箭头函数中， this 的指向在定义的时候就决定了。

<!--more-->

`this` 指向的是调用他的对象

## 普通函数调用

```
    var b = '1';
    function thisHandler() {
	    console.log('b:',this.b,'this:',this);
    }
    
    thisHandler() // b: 1 this: window
```
可以看出，这时 `this` 是指向 `window` 的

## 方法调用

```
var thisHandler = {
    name: "ym",
    test: function(){
        console.log(this.name); 
    }
}
thisHandler.test() // ym
```
这时 `this` 指向的是 `thisHandler` 对象，因为是它调用的 test。


```
    var thisHandler = {
        name: "ym",
	fn: {
		name: 'mm',
		test: function(){
        	console.log(this.name); 
    	    }
	}
}
thisHandler.fn.test() // mm
```

这时 `this` 指向的是对象 `fn` ，说明 `this` 指向的是它的上一级对象。

```
var thisHandler = {
        age: "12",
	fn: {
		age: '13',
		test: function(){
        	console.log(this.age); 
    	    }
	}
    
}
var testHandler = thisHandler.fn.test
testHandler() // undefined
```

这时 `this` 指向的就是 `window`，因为 `this` 指向的是最后调用它的对象，第一步是赋值给了 `testHandler`，可是并没有执行。所以最后输出的是 `undefined`。如果我们想要让 `this` 指向 `fn` 可以这样写。

## apply / call 调用

```
var thisHandler = {
        age: "12",
	fn: {
		age: '13',
		test: function(){
        	console.log(this.age); 
    	    }
	}
    
}
var testHandler = thisHandler.fn.test;
testHandler.apply(thisHandler.fn) //  13
```
通过 `apply` 方法，就改变了 `this` 的指向，指向了 `thisHandler.fn`。 



## 构造函数的调用

```
var name = 'ym'
function bar() {
	this.name = 'mm'
}
var handlerA = new bar();
console.log(handlerA.name);  // mm
console.log(name) // ym
```
这时， this 指向的是 handlerA，这时就要了解一下 new 的原理了。

- 创建一个新的对象，继承自bar.prototype
- 构造函数 bar 被执行，this 指向这个新的实例

上源码

```
function New (f) {
    var n = { '__proto__': f.prototype }; /*第一步*/
    return function () {
        f.apply(n, arguments);            /*第二步*/
        return n;                         /*第三步*/
    };
}
```



