title: JavaScript 数组操作合集
date: 2016-09-21 23:48:07
tags: [JavaScript, Array]
categories: Front End
#description: JavaScript 中操作数组的各种方法，方法不包含 ES6+. 本博客可作为字典用
---

## 参考

> 《JavaScript 权威指南》

> 《JavaScript 高级程序设计》

---

## Overview
![](http://7xoxnz.com1.z0.glb.clouddn.com/js-array-overview.png)
<!-- more -->
---

## JavaScript 数组的特性

- 无类型

- 动态

- 可能稀疏

---

## 创建数组

两种方式

- 使用数组字面量（数组直接量）：`var array = [];`

- 使用构造函数 Array: `var array = new Array[];`

---

## 数组检测

- ES5：使用 `Array.isArray()` 方法
``` js
Array.isArray([])  //true
```


- ES3：
``` js
var isArray = Function.isArray || function(o) {
  return typeof === 'object' &&
  Object.prototype.toStrong.call(o) === '[Object Array]';
}
```

---

## 数组上的操作

### 先来个总结

| 方法       | 参数       |   作用    |  是否修改原数组  |
| :--------:  | :-----:    | :----: | :---:|
| join()      | 字符串，作为分隔符（可选） |   将数组中所有元素转化为字符串并连接在一起   | 否|
| reverse()      | 无      |   将数组中的元素颠倒顺序    | 是|
| sort()        | 比较函数（可选）      |   进行排序。默认按字母表顺序    | 是|
| concat() | 用于拼接 | 创建一个新的拼接好的数组 | 否|
| slice() | 两个参数分别指定了片段的开始和结束位置 [a, b) | 返回指定数组的一个片段 | 否 |
| splice() | 第一个参数制定删除的起始位置，第二个指定被删除的个数，之后参数是插入的元素 | 插入和删除 | 是 |
| push() | 待添加元素 | 数组尾部添加值并返回数组新长度 | 是|
| pop() | 无 | 数组尾部删除并返回删除的值 | 是 |
| unsift() | 待添加元素 | 数组头部添加值并返回数组新长度 | 是|
| shift() | 无 | 数组头部删除并返回删除的值 | 是 |

### join()
``` js
var arr = [1, 2, 3];
arr.join();  // return "1,2,3"
arr.join('');  // return "123"
arr.join(' ');  // return "1 2 3"
var b = new Array(10);
b.join('-');  // return "----------"
```

### reverse()
``` js
var arr = [1, 2, 3];
arr.reverse();  // arr is [3,2,1]
```

### sort()
``` js
var arr = [33, 4, 1111, 222];
arr.sort();  // arr is [1111, 222, 33, 4]
arr.sort((a, b) => a-b);  // arr is [4, 33, 222, 1111]
```

### concat()
``` js
var arr = [1, 2];
arr.concat(3, 4);  // return [1, 2, 3, 4]
arr.concat([3, 4]);  // return [1, 2, 3, 4]
```

### slice()
args: [start, end)
``` js
var arr = [1, 2, 3, 4, 5];
arr.slice(0, 3);  // return [1, 2, 3]
arr.slice(3);  // return [4, 5]
```

### splice()
args: (index, count, toBeAdded...)
index 从零开始计数，删除 count 个 index 对应元素之后的元素（包括自己），被添加的元素添加到 index 对应元素之前
``` js
var arr = [1, 2, 3, 4, 5, 6, 7];
arr.splice(4);  // return [5, 6, 7]; arr is [1, 2, 3, 4]
arr.splice(1, 2);  // return [2, 3]; arr is [1, 4]
arr.splice(1, 0, 'a', 'b');  // return []; arr is [1, 'a', 'b' ,4]
arr.splice(1, 1, 'c');  // return ['a']; arr is [1, 'c', 'b', 4]
```

---

## 数组遍历的方法（均为ES5+）

- forEach(val, index, arr)
  要提前终止的话，要使用 `try-catch`
  ``` js
  [].forEach((el, i, arr) => {
    // do something
  })
  ```

- map()
  遍历数组每一个元素并对其进行操作之后，返回新的数组
  ``` js
  var a = [1, 2, 3];
  var b = a.map(i => i * i); //[1, 4, 9]
  ```

- filter()
  遍历数组每一个元素并进行判断之后，返回判断为真的元素组成的新数组
  ``` js
  var a = [1, 2, 3];
  var b = a.filter(val => val > 1); //[2, 3]
  ```

- every() & some()
  遍历数组每个元素，根据表达式进行判断，返回一个布尔值，对于 `every()` 是所有，对于 `some()`是存在
  ``` js
  var arr = [1, 2, 3];
  arr.every(i => i<4);  //true
  arr.some(i => i%2===0);  //true
  ```

- reduce() & reduceRight()
  [参考这个 -- Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
  可用于数组求和以及数组扁平化
  ``` js
  var sum = [0, 1, 2, 3].reduce(function(a, b) {
    return a + b;
  }, 0);
  // sum is 6

  var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
    return a.concat(b);
  }, []);
  // flattened is [0, 1, 2, 3, 4, 5]
  ```

- indexOf() & lastIndexOf()
  在数组中寻找值，前者为顺序查找，后者为倒序。若找到，则返回找到的值的 index，反之返回 -1

---

## ES6 的数组方法

可参考下面博客进行学习
[ECMAScript 6’s new array methods](http://www.2ality.com/2014/05/es6-array-methods.html)
[JavaScript学习笔记：ES6数组方法](http://www.w3cplus.com/javascript/es6-array-methods.html)

---

## 一些总结

有副作用的操作：`reverse()` `sort()` `splice()` `push()` `pop()` `shift()` `unshift()`

---
EOF
