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
- 使用 `Object.defineProperty()` 和 `Object.defineProperties()` 定义属性，使用 `Object.getOwnPropertyDescriptor()` 读取属性

- 工厂模式创建对象，缺点是没有解决对象识别的问题，一般不会使用这个
``` js
function createPerson(name, age) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.introduce = function(){
        alert(this.name + "," + this.age);
    };
    return o;
}

var person1 = createPerson("Neo", 12);
```

- 构造函数模式，解决了工厂模式的问题，缺点是他的成员得不到复用，包括函数
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

    var person1 = new Person("Neo", 12, "Teacher");
    person1.sayName();
    ```

***

## 原型

### 什么是原型对象

- 一个函数创建时会有一个默认的原型属性 `prototype`，这个属性指向这个函数的原型对象。这个原型对象有一个构造函数属性 `constructor`，他是指向这个函数的指针。那上面的代码看， `Person.prototype.constructor` 指向 `Person`。

- 函数的实例可以访问，但不能重写原型中的值。

- `hasOwnProperty()` 和 `hasPrototypeProperty()` 检测属性是在对象中还是原型中

### 应用

- 原型模式，所有实例共享属性。故一般结合原型和构造函数模式一起使用，他们的优点相互补充
    ``` js
    function Person() {
    }

    Person.prototype.name = "Neo";
    Person.prototype.age = 29;
    Person.prototype.job = "Teacher";
    Person.prototype.sayName = function() {
        alert(this.name);
    }

    var person1 = new Person();
    person1.sayName();
    ```

- 或是直接重写函数的 `prototype` 属性，并将 `constructor` 指向函数
    ``` js
    function Person() {
    }

    Person.prototype = {
        constructor: Person,
        name: "Neo",
        age: "12",
        job: "Teacher",
        sayName: function() {
            alert(this.name);
        }
    };
    ```

- **组合使用构造函数和原型模式**，使用最广泛、认同度最高
    ``` js
    // ES3 ES5
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
    }

    Person.prototype = {
        constructor : Person,
        sayName : function() {
            alert(this.name);
        }
    }

    var person1 = new Person("Neo", 12, "student");
    ```

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

    var person1 = new Person("Neo", 12, "student");
    person1.sayName();
    ```

## 继承

- ECMAScript 的继承通过原型链实现

- 通过原型链继承，但属性会被所有实例共享，且创建子类时不能向超类的构造函数传参
``` js
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
Sub.prototype = new Super(); // extend
Sub.prototype.getSubValue = function() {
    return this.property;
}

var instance = new Sub();
instance.getSuperValue(); // false
```

- 借用构造函数，解决了向超类的构造函数传参的问题，但函数成员得不到复用
``` js
function Super(name) {
    this.name = name;
    this.colors = ["red","green","blue"];
}

function Sub() {
    Super.call(this, "Neo"); //extend
}

var instance = new Sub();
```

- **组合继承**，结合了前两者的优点，是最常用的继承模式
    ``` js
    // ES3 ES5
    function Super(name) {
        this.name = name;
        this.colors = ["red","green","blue"];
    }
    Super.prototype.sayName = function(){
        alert(this.name);
    };

    function Sub(name, age) {
        Super.call(this, "Neo"); //extend the properties
        this.age = age;
    }
    Sub.prototype = new Super(); //extend the methods
    Sub.prototype.constructor = Sub;
    Sub.prototype.sayAge = function() {
        alert(this.age);
    }

    var instance = new Sub("Neo", 21);
    ```

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

    var instance = new Sub("Nick", 12);
    instance.sayName();
    instance.sayAge();
    ```
