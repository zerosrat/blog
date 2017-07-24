---
title: JS 学习笔记（面向对象篇）
date: 2016-05-03 22:09:10
tags: JavaScript
categories: Front End
description: 在基础知识的基础上，进入面向对象的学习阶段。定义类和继承时，可使用 ES6 提供的 `class` 关键字，本文中有代码示例。
---

## 前言

在基础知识的基础上，进入面向对象的学习阶段。定义类和继承时，可使用 ES6 提供的 `class` 关键字，本文中有代码示例。

## 对象

- ECMAScript 中有两种属性值：数据属性和访问器属性
<!-- more -->
- 使用 `Object.defineProperty()` 和 `Object.defineProperties()` 定义属性，使用 `Object.getOwnPropertyDescriptor()` 和 `Object.getOwnPropertyDescriptors()` 读取属性

- **工厂模式** 创建对象，缺点是没有解决对象识别的问题，一般不会使用这个
``` js
function createPerson(name, age) {
  const o = {};
  o.name = name;
  o.age = age;
  o.introduce = function(){
    alert(this.name + ',' + this.age);
  };
  return o;
}

const person1 = createPerson('Zero', 12);
```

- **构造函数** 模式，解决了工厂模式的问题，缺点是函数得不到复用
``` js
// ES3 ES5
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function(){
    alert(this.name);
  };
}

const person1 = new Person('Zero', 22, 'Student');
person1.sayName();
```
***

## 原型

### 什么是原型对象

- 一个函数创建时会有一个默认的原型属性 `prototype`，这个属性指向这个函数的原型对象。这个原型对象有一个构造函数属性 `constructor`，他是指向这个函数的指针。从上面的代码看， `Person.prototype` 指向函数的原型对象； `Person.prototype.constructor` 指向 `Person`。

- 函数的实例可以访问，但不能重写原型中的值。

- `hasOwnProperty()` 检测属性是否在对象中，而非继承来的

### 应用

- **原型模式**，所有实例共享属性。故一般结合原型和构造函数模式一起使用，他们的优点相互补充
``` js
function Person() {
}

Person.prototype.name = 'Zero';
Person.prototype.age = 29;
Person.prototype.job = 'Teacher';
Person.prototype.sayName = function() {
  alert(this.name);
}

const person1 = new Person();
person1.sayName();
```

- **组合使用构造函数和原型模式**，使用最广泛、认同度最高
``` js
// ES3 ES5
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
}

Person.prototype.sayName = function() {
  alert(this.name);
}

const person1 = new Person('Zero', 12, 'student');
```

- 使用 ES6 的 `class` 关键字创建对象
``` js
// ES6
class Person {
  constructor(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
  }

  sayName() {
    console.log(this.name);
  }
}

const person1 = new Person("Zero", 12, "student");
person1.sayName();
```

## 继承

- ECMAScript 的继承通过原型链实现

- **借用构造函数**，解决了向超类的构造函数传参的问题，但函数成员得不到复用
``` js
function Super(name) {
  this.name = name;
  this.colors = ['red', 'green', 'blue'];
}

function Sub() {
  Super.call(this, 'Zero'); //extend
}

const instance = new Sub();
```

- **组合继承**，常用的继承模式
``` js
// ES3 ES5
function Super(name) {
  this.name = name;
  this.colors = ['red', 'green', 'blue'];
}
Super.prototype.sayName = function(){
  alert(this.name);
};

function Sub(name, age) {
  Super.call(this, 'Zero'); //extend the properties
  this.age = age;
}
// inherit
Sub.prototype = new Super();
Object.defineProperty(Sub.prototype, 'constructor', {
  enumerable: false,
  value: Sub
});

Sub.prototype.sayAge = function() {
  alert(this.age);
}

const instance = new Sub('Zero', 21);
```

- **寄生组合式继承**，解决组合继承调用两次超类构造函数的问题
``` js
function object(o) {
  function F() {}
  F.prototype = o;
  return new F();
}

// super class
function Super() {
  this.property = true;
}
Super.prototype.getSuperValue = function() {
  return this.property;
}
// sub class
function Sub() {
  this.property = false;
}
// inherit
if (Object.create) {
  Sub.prototype = Object.create(Super.prototype);
} else {
  Sub.prototype = object(Super.prototype)
}
Object.defineProperty(Sub.prototype, 'constructor', {
  enumerable: false,
  value: Sub
});

Sub.prototype.getSubValue = function() {
  return this.property;
}

const instance = new Sub();
instance.getSuperValue(); // false
```

- 使用 ES6 `class` 和 `extend` 关键字
``` js
// ES6
class Super {
  constructor(name) {
    this.name = name;
  }
  sayName() {
    console.log(this.name);
  }
}

class Sub extends Super {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
  sayAge() {
    console.log(this.age);
  }
}

const instance = new Sub('Nick', 12);
instance.sayName();
instance.sayAge();
```
