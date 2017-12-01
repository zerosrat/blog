---
title: 谈谈 Function.prototype.bind() 的 polyfill（上）
date: 2017-12-01 11:01:38
tags: JavaScript
---

## 为啥写这篇博客
以前一直是忽略了 `bind` 函数是怎么实现的，以致于看见下面这段代码时不知所措。

``` js
function largest(arr) {
  return arr.map(Function.apply.bind(Math.max, null))
}

largest([[1,34],[456,2,3,44]) //[34, 456]
```

于是我便开始看 `bind` 的实现，其中也涉及了很多概念如下，是非常值得重温和学习的
- 作用域链
- 闭包
- 柯里化
- `new` 操作做了什么
- ...

在真正理解了其实现过程后，会明白 `bind` 函数在上面代码里起到了重要作用

<!-- more -->

## 基础

### 一段简单的代码

``` js
var obj = {
  a: 1,
  print(x, y) {
    console.log(this.a, x + y)
  }
}

var test = {
  a: 2
}

obj.print(1, 2) // 1 3

obj.print.apply(test, [1, 2]) // 2 3
obj.print.call(test, 1, 2) // 2 3
obj.print.bind(test)(1, 2) // 2 3
obj.print.bind(test, 1)(2) // 2 3
obj.print.bind(test, 1, 2)() // 2 3
```

### Function.prototype.call(context, ...arg)
> [Function.prototype.call() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

一句话：在调用一个存在的函数时，你可以为其指定一个上下文对象 `context`，并且传入多个参数。

### Function.prototype.apply(context, arr)
> [Function.prototype.apply() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

一句话：在调用一个存在的函数时，你可以为其指定一个上下文对象 `context`，并且传入多个参数。

### Function.prototype.bind(context, ...arg)
> [Function.prototype.bind() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

一句话：`bind()` 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体

### 函数柯里化
> [JavaScript专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)

理解：用闭包把参数保存起来，当参数的数量足够执行函数了，就开始执行函数

---

## 目标
1. 返回一个由指定的 `this` 值（第一个参数）和初始化参数的新函数（是原函数的拷贝）
2. 返回的新函数可以继续指定参数
3. `bind` 函数不可通过 `new` 调用
3. 新的绑定函数也能使用 `new` 操作符创建对象，这样做时指定的 `this` 无效

## 从零开始实现

下面代码实现了最简单的 `bind` 函数，返回新函数绑定了传入的 this 上下文以及给新函数传入了默认参数 arg

``` js
Function.prototype.bind = Function.prototype.bind || function(context) {
  var target = this
  var args = Array.prototype.slice.call(arguments, 1)
  return () => target.apply(context, args)
}
```

然后我们要求新函数也可以传入参数，这时候就不能用箭头函数了
``` js
Function.prototype.bind = Function.prototype.bind || function(context) {
  var target = this
  var args = Array.prototype.slice.call(arguments, 1)

  return function() {
    var newArgs = aArray.prototype.slice.call(arguments)
    target.apply(context, newArgs.concat(args))
  }
}
```

`new fn.bind()` 是非法的，非法调用的话要抛出异常
``` js
Function.prototype.bind = Function.prototype.bind || function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable')
  }

  var target = this
  var args = Array.prototype.slice.call(arguments, 1)

  return function() {
    var newArgs = aArray.prototype.slice.call(arguments)
    target.apply(context, newArgs.concat(args))
  }
}
```

再来实现返回函数支持 `new` 操作
我们知道，当代码 `new Foo(...)` 执行时：
- 一个新对象被创建。它继承自Foo.prototype。
- 使用指定的参数调用构造函数Foo，并将 this绑定到新创建的对象。

``` js
Function.prototype.bind = Function.prototype.bind || function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable')
  }

  var target = this
  var args = Array.prototype.slice.call(arguments, 1)

  var fn = function() {}
  fn.prototype = target.prototype
  fnBound.prototype = new fn()
  // or use `Object.create()`
  // fnBound.prototype = Object.create(target.prototype)

  return function fnBound() {
    var newArgs = aArray.prototype.slice.call(arguments)
    var finalArgs = newArgs.concat(args)
    if (this instanceof fn) {
      target.apply(this, finalArgs)
    } else {
      target.apply(context, finalArgs)
    }
  }
}
```

到了这里，代码其实和 MDN 里的 polyfill 差不多了，代码见[链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Compatibility)

## 已经完美？
当我们认为这份代码已经完美时，这时应该看看 [es5-shim 对 bind 的实现](https://github.com/es-shims/es5-shim/blob/master/es5-shim.js#L201)。下部分再来讲遗漏的点吧！

---

## 参考
- [深入学习JavaScript之一：bind函数的实现](https://github.com/shhdgit/blogs/issues/1)
- [从一道面试题的进阶，到“我可能看了假源码](https://exp-team.github.io/blog/2017/02/20/js/bind/)
