title: JS 学习笔记（基础篇）
date: 2016-04-25 23:57:23
tags: JavaScript
categories: Front End
description: 关于 JavaScript 基础知识重要知识点的一些总结。
---

## 前言

记录自己学习 JavaScript 过程中，发现的一些重要的知识点。主要知识来源是《JavaScript高级程序设计》。

本文主要是 JavaScript 的基础知识记录。

## JavaScript 简介

- 一个完整的 JavaScript 实现应该由三个部分组成：核心（ECMAScript）、文档对象模型（DOM）和浏览器对象模型（BOM）

- ECMAScript，由 ECMA-262 （第五版）定义，提供核心语言功能

- DOM，提供访问和操作网页内容的方法和接口

- BOM，提供与浏览器交互的方法和接口
<!-- more -->
## 在 HTML 中使用 JavaScript

- `<script>` 标签的属性 `type` 的默认值是 `text/javascript`.

- 现代 Web 应用程序一般都把全部 JavaScript 引用放在 `<body>` 元素中页面内容的后面，即主要内容后面，`</body>` 标签前面

- `<script>` 的 `src` 属性还可以包含来自外域的 JavaScript 文件，类似的还有 `<img>` 标签

## 基本概念

- ECMASript 的变量是松散类型的，可以在修改变量值的同时修改值的类型。

- ECMASript 5 引入了严格模式的概念，可以在脚本顶部添加 `use strict;` 使其生效

- ECMASript 的5种简单数据类型（基本数据类型）： `Undefined`、`Null`、`Boolean`、`Number` 和 `String`

- 使用 `typeof` 操作符可能返回的的值有：`"undefined"`、`"boolean"`、`"string"`、`"number"`、`"object"` 和 `"function"`

- `Undefined` 和 `Null` 类型只有一个值，分别为 `undefined` 和 `null`

- 未经初始化或是未声明的变量，对其使用 `typeof` 操作符返回的值为 "undefined"

- `undefined` 值是派生自 `null` 值的，`null == undefined` 的返回值为 `true`

- `NUmber` 类型可以表示整数和浮点数，或是 `NaN` (Not a Number)

- 字符串一旦创建，他们的值就不能改变；数值、布尔值、对象和字符串值都有 `toString()` 方法

- ECMAScript 函数没有重载，但可以通过函数参数 `arguments[i]` 在一定程度上实现重载

- 无须指定函数的返回值，未指定返回值的函数返回的值是 `undefined`

## 变量、作用域和内存问题

- JS 不允许直接访问内存中的位置，操作对象时操作的是对象的引用而不是实际对象

- 参数都是按值传递的

- 内部环境可以通过作用域链（scope chain）访问所有的外部环境，外部环境不能访问内部环境中的任何变量和函数

- JS 具有自动垃圾收集机制，程序员不必关心内存分配和回收的问题

## 引用类型

- 引用类型和类不是同一概念，对象引用类型的一个实例

- ECMAScript 数组的每一项可以保存任务类型的数据

- `Array` 类型的 `length` 属性不是只读的

- 所有对象都具有 `toLocaleString()`、`toString()` 和 `valueOf()` 方法

- 数组的栈方法为 `push()` 和 `pop()`，数组的队列方法为 `shift()` 和 `push()`

- 函数是 `Funcation` 类型的实例，所以函数也是对象

- 函数内部有两个特殊对象：`this` 和 `arguments`，可以在函数内部使用 `agruments.callee` 消除紧密耦合

- 函数包含两个属性：`length` 和 `prototype`，前者是指函数的参数个数

- 函数包含两个方法：`apply()` 和 `call()`，作用是传递参数和扩充函数运行作用域，前者传数组，后者传所有参数

- 读取基本类型值的时候，会创建对应基本包装类型的一个对象，读取结束时候销毁这个对象

- `eval()` 是个危险的方法，使用时要尤为谨慎，特别是在用它执行用户输入数据的情况下

- 作用域的两个单体内置对象：`Global` 和 `Math`，前者虽然不能直接访问，但浏览器将其作为 `window` 对象的一部分实现

## 正则表达式

- 常用的元字符

<table><tr><th>元字符</th><th>描述</th></tr><tr><td>.</td><td>匹配除换行符以外的任意字符</td></tr><tr><td>\w</td><td>匹配字母或数字或下划线或汉字</td></tr><tr><td>\s</td><td>匹配任意的空白符</td></tr><tr><td>\d</td><td>匹配数字</td></tr><tr><td>\d</td><td>匹配单词的开始或结束</td></tr><tr><td>^</td><td>匹配字符串的开始</td></tr><tr><td>$</td><td>匹配字符串的结束</td></tr></table>

- 常用的限定符

<table><tr><th>元字符</th><th>描述</th></tr><tr><td>*</td><td>重复\>=0次</td></tr><tr><td>+</td><td>重复\>=1次</td></tr><tr><td>?</td><td>重复0或1次</td></tr><tr><td>{n}</td><td>重复n次</td></tr><tr><td>{n,}</td><td>重复\>=n次</td></tr><tr><td>{n,m}</td><td>重复[n,m]次</td></tr></table>

### RegExp 类型

- ECMAScript 通过 RegExp 类型来支持正则表达式。`var expression = / pattern / flags`，
其中 pattern 是正则表达式，flags 可以是3个标志或它们的结合 (`g` 表示 global，发现第一个匹配项时不会停止；`i` 表示不区分大小写；`m` 表示到达文本末尾时还会到下一行继续查找)
eg1：`var reg1 = /[bc]a/g; //匹配字符串中所有的 ba 或 ca`
eg2：`var reg2 = new RegExp(".at", "i"); //匹配结尾是 at 的字符串，不分大小写`

- RegExp 类型的属性，`global` 、 `ignoreCase` 、 `lastIndex` 、 `multiline` and `source`

- RegExp 实例方法，`exec()` 和 `test()`，前者返回匹配的字符串，后者返回布尔值

