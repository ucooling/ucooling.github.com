---
layout: post
title: "ES6中的Promise"
date: 2017-02-15 16:24:42 +0800
comments: true
categories: 
---
去年ES6正式发布，这次发布将Promise也正式列入了规范，虽然很多的库或者框架已经对Promise做了很好的封装，但是还是很有必要深入的理解它的概念和使用方法。

## 什么是Promise
很多人在开始学习一个新知识之前都会问自己这个问题，对于很多的新手也许就会掉进这个魔咒，因为网上有很多文章在讲Promise是什么，但是讲的都似是而非，读起来似乎明白了又好像没太理解。其实很简单Promise也是一个JavaScript对象，你只需要console出来不就知道是什么了，就是这么粗暴。

![console.log(Promise)](/images/Promise.png)

这样一看就很简单，Promise就是一个JavaScript对象，自己有all、resolve、reject等这几个方法，原型链上有then、catch等方法。

## 小试Promise
```
var promise = new Promise(resolve, reject) {
	//这里可以实现一个异步操作
	//resolve是异步操作成功时的回调
	//reject是异步操作失败时的回调
	setTimeout(function(){
		console.log("执行完成");
		resolve('异步执行成功的回调');
	}, 1000);
}
```

上边这段代码实现了一个Promise，对于Promise对象接受一个参数，是一个函数，并且这个函数需要传入两个参数resolve(成功时的回调)、reject(失败时的回调)。这段代码执行一个异步操作。1秒后输出“执行完成”，并且调用resolve方法。

看到这里可能有的人问了这样跟普通的回调有什么区别，不要着急，接下来就讲。
对于传统的回调函数，如果只是简单的一次回调，ok那没有任何问题，但是对于一个很大的系统，代码库非常的复杂，回调会很深，如果你遇到这样的问题肯定深有体会。看这样的代码真是想死的心都有。但是如果通过Promise来实现的话，就可以通过调用then方法链式调用。下边是一个例子：

```
	function tryAsync() {
		var promise = new Promise(resolve, reject) {
			//这里可以实现一个异步操作
			//resolve是异步操作成功时的回调
			//reject是异步操作失败时的回调
			setTimeout(function(){
				console.log("执行完成");
				resolve('异步执行成功的回调');
			}, 1000);
		}
		return promise;
	}
	
	tryAsync().then(function(data){console.log(data)}).then().then()...

```

## resolve、reject用法
到这里应该对Promise有了一个基本的认识，那么现在我们来看看resolve和reject的用法，其实他们就是成功时的回调，和失败时的回调。

```
function getNumber() {
	var promise = new Promise(resolve, reject) {
		setTimeout(function(){
			var num = Math.ceil(Math.random()*10);
			if(num > 5){
				resolve();
			}else {
				reject()
			}
		}, 1000)
	};
	
	return promise;
}
getNumber().then(
	function(data){
		console.log('resolved');
		console.log(data);
	},
	function(reason, data){
		console.log('reject');
		console.log(reason);
	}
)
```

getNumber函数用来异步获取一个数字，1秒后执行完成，如果数字小于5，就代表我们执行了resolve函数，修改Promise的状态。否则就是失败了，调用reject方法。

##总结
其实Promise还提供了类似all、race、catch等方法，用法基本类似，网上也有很多的参考资料，不再赘述。





