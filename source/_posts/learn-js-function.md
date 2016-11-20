---
title: JS学习笔记（函数表达式篇）
date: 2016-05-21 16:33:13
tags: JavaScript
categories: Front End
description: 与函数表达式相关的重要知识点的梳理，如闭包。
---

## Before starting

先复习一下创建函数的两种方式：函数声明和函数表达式。

- 函数声明
``` js
function funcName(arg0, arg1){
    // To do
}
```

- 函数表达式
``` js
var funcName = function(arg0, arg1){
    // To do
}
```

***

## 递归函数

- 最佳实践
    ``` js
    function factorial(num){
        if(num <= 1){
            return 1;
        } else{
            return num * arguments.callee(num - 1);
        }
    }
    ```

- 标准错误答案
    ``` js
    function factorial(num){
        if(num <= 1){
            return 1;
        } else{
            return num * factorial(num - 1);
        }
    }

    var anotherFactorial = factorial;
    factorial = null
    alert(anotherFactorial(3)); //fatal error!
    ```

***

## 闭包

### 什么是闭包

- 在函数中创建函数，就创建了闭包

- 一个实例
    ``` js
    function createCompareFunc(property) {
        return function(obj1, obj2) {
            var val1 = obj1[property];
            var val2 = obj2[property];
            if(val1 < val2) {
                return -1;
            } else if(val1 > val2) {
                return 1;
            } else {
                return 0;
            }
        }
    }

    var compare = createCompareFunc("name");
    var result = compare({"name": "Tywin"}, {"name": "Jamie"});
    alert(result);
    compare = null; //将匿名函数解除引用
    ```

    notes: 最后解除对匿名函数的引用，销毁匿名函数的作用域，从而释放上层函数 `createCompareFunc()` 的活动对象

### 闭包与 this 对象

- 之前知道了，每个函数有两个对象-- `this` 和 `arguments`。由于作用域链的作用，当闭包函数使用 `this` 对象时，在闭包函数中是不可访问到外部函数的 `this` 的。但可以通过将外部作用域中 `this` 保存在闭包可以访问到的变量里的方式，来解决上述问题。
``` js
var name = "window";
var obj = {
    name: "my obj",
    getName: function() {
        var that = this; //store the var
        return function() {
            return that.name;
        }
    },
    setName: function(name) {
        this.name = name;
    }
};
alert(obj.getName()); //"my obj"
```

***

## 模仿块级作用域（或是称为私有作用域）

- JavaScript 本是没有块级作用域的概念的，但可以通过匿名函数来实现

- 最佳实践
    ``` js
    (function() {
        //block scope
    })();
    // or
    (function() {

    })
    ```

- 主要用于全局作用域中，避免全局的命名冲突

***

## 私有变量

- 函数中的这些变量可称为私有变量，函数参数、局部变量和函数内定义的函数，他们在函数外是不可被访问的

- 在构造函数中创建特权方法
``` js
function MyObj() {
    var privateVar = 1;
    function privateFunc() {
        return false;
    }

    //特权方法
    this.publicFunc() {
        privateVar++;
        return privateFunc();
    }
}
```

- 静态私有变量，解决构造函数模式不能复用的缺点
``` js
(function() {
    var privateVar = 1;
    function privateFunc() {
        return false;
    }

    //全局构造函数，使其可在块作用域之外被访问
    MyObj = function() {
    };
    //特权方法，原型模式的影子
    MyObj.prototype.publicFunc = function() {
        privateVar++;
        return privateFunc();
    };
})();
```

***

## 单例模式

- 通过块级作用域和特权方法来实现
``` js
var singleton = function() {
    var components = new Array();
    components.push(new BaseComponent());

    return {
        getComponentCount: function() {
            return components.length;
        },
        registerComponent: function(component) {
            if(typeof component == "object") {
                components.push(component);
            }
        }
    };
}();
```
