---
layout: post
title: constructor、call、this
date: 2018-01-19 09:38:41
tags:
---

之前研究过__proto__ 属性，今天结合__proto__ 属性再研究下constructor,call直接上代码

定义两个构造函数

```
	function Watch () {
	  this.watch = []
	  this.reduce = 'jud'
	}

	function Dep () {
	  this.subs = []
	}

```
当我们在定义构造函数的时候，就自带一个prototype属性，prototype 属性又有一个属性constructor,一般情况下是指向构造函数本身。再说下call
，call用法归纳为亮点：（1）实现继承 （2）改变执行函数作用域this的指向


```
	Dep.prototype.add = function() {
		  console.log('this1', this)
		  Watch.call(this)
		  console.log('this2', this)
	}

```
此时我们调用add方法
```
var dep = new Dep()
    dep.add()
```
this1-->指向当前函数执行的作用域，就是Dep函数本身
当执行Watch.call(this) 此时this的作用域已经改变
this2--》指向Watch 函数本身，而此时Dep函数也继承了Watch构造函数上面的方法和属性
但原型链上面的方法和属性无法继承。

一般情况下时，我们可以在构造函数外面直接定义一个属性， 比如

```
Dep.target =  6

```
为何说一般情况呢？当prototype 指向一个对象，比如 如下

```
	Dep.prototype = {
	    add : function() {
	        console.log('this1', this)
	        Watch.call(this)
	        console.log('this2', this)
	    }
	}

```
此时，如果你再在构造函数外面定义一个属性，就找不到这个属性了
此时，Dep.prototype.constructor 就指向了Obejct了，除非你手动转换下
Dep.prototype.constructor = Dep
