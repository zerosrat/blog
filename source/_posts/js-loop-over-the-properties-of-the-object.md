---
title: JavaScript 中遍历对象的属性
date: 2016-07-13 22:06:26
tags: [JavaScript, loop]
categories: Front End
---

## 参考

> [JavaScript中的属性:如何遍历属性](http://www.cnblogs.com/ziyunfei/archive/2012/11/03/2752905.html)
> 《JavaScript 高级程序设计》

***

## 概述

遍历 JavaScript 对象中的属性没有其他语言那么简单，因为两个因素会影响属性的遍历：对象属性的属性描述符 (property descriptor) 的 `[[Enumerable]]` 特性为 `true` （可枚举）才能被 `for-in` 访问；如果在对象本身没有找到属性，接下来会在原型链上查找，访问属性时会沿着整个原型链从下到上查找属性。所以说遍历属性时，要考虑这两个因素。
<!-- more -->
***

## 开始遍历

先定义两个类吧：Person 和 Student，后者继承前者。然后再声明并初始化一个 Student 的实例 p1。其中自身属性有 grade（可枚举） 和 tel（不可枚举），从原型链继承属性 type.
可枚举意思是属性的 `[[Enumerable]]` 值为 `true`，自身的属性意思是 *不是* 从 **原型链** 继承的属性

``` js
// ES3 ES5
function Person(name) {
    this.name = name;
}

Person.prototype.type = 'people';

function Student(name, grade) {
    Person.call(this, name);
    this.grade = grade;
}

Student.prototype = new Person();
Student.prototype.constructor = Student;

var p1 = new Student('Zero', 'Junior');
Object.defineProperty(p1, 'tel', {
    value: 123456,
    enumerable: false
});
```

``` js
// ES6+
class Person {
    constructor(name) {
        this.name = name;
    }
}

Person.prototype.type = 'people';

class Student extends Person {
    constructor(name, grade) {
        super(name);
        this.grade = grade;
    }
}

var p1 = new Student('zero', 'Junior');
Object.defineProperty(p1, 'tel', {
    value: 123456,
    enumerable: false
});
```

### 遍历可枚举的、自身的属性

使用 `Object.keys()` 或是 `for..in` + `hasOwnProperty()`

``` js
// Object.keys()返回可枚举、自身的属性
// 再用for..of对返回的数组进行遍历
for (let prop of Object.keys(p1)){
    console.log(prop);
}
```

``` js
// 得到可枚举、自身+继承的属性
for (let prop in p1) {
    // 过滤继承属性
    if (p1.hasOwnProperty(prop)) {
        console.log(prop);
    }
}
```

结果是：name 和 grade 属性
注： `Object.keys()` 的使用环境是 ES5+

### 遍历所有（可枚举的&不可枚举的）、自身的属性

``` js
// 使用 `Object.getOwnPropertyNames()`
for (let prop of Object.getOwnPropertyNames(p1)) {
    console.log(prop);
}
```

结果是：name 、 grade 和 tel 属性

### 遍历可枚举的、自身+继承的属性

``` js
// 使用 `for..in`
for (let prop in p1) {
    console.log(prop);
}
```

结果是：name 、 grade 和 type 属性

### 遍历所有的、自身+继承的属性

``` js
var getAllPropertyNames = (obj) => {
    var props = [];
    do {
        props = props.concat(Object.getOwnPropertyNames(obj));
    } while (obj = Object.getPrototypeOf(obj));
    return props;
};

for (let prop of getAllPropertyNames(p1)) {
    console.log(prop);
}
```

结果很多：包括自身属性 name 、 grade 等，继承属性 type 、 toString 、valueOf 等

***
EOF
