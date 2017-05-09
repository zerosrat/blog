---
title: JavaScript 数组遍历
date: 2016-07-24 10:02:07
tags: [JavaScript, loop]
categories: Front End
---

## 参考

> [For-each over an array in JavaScript?](http://stackoverflow.com/questions/9329446/for-each-over-an-array-in-javascript)
> [Why is 'for(var item in list)' with arrays considered bad practice in JavaScript?](http://stackoverflow.com/questions/2265167/why-is-forvar-item-in-list-with-arrays-considered-bad-practice-in-javascript)
> [forEach and runtime cost](http://blog.niftysnippets.org/2012/02/foreach-and-runtime-cost.html)

---

## 开始遍历

多种选择：

- `for`

- `for-in`

- `forEach` 以及相关的（ES5+）

- `for-of`（ES6+）

- 使用迭代器（ES6+）
<!-- more -->

先声明并初始化一个数组吧：`let a = ["a", "b", "c"];`

### 使用 `for` 循环

``` js
for (let i = a.length; i--; ) {
    console.log(a[i]);
}
```

### 使用 `for-in` （不推荐）

``` js
for (let i in a) {
    console.log(a[i]);
}
```

### `forEach` 以及相关的

``` js
a.forEach((e, i, a) => console.log(`element:${e}, index:${i}, array:${a}`));
```

``` js
a.map(n => console.log(n));
```

### 使用 `for-of`

``` js
for (let val of a) {
    console.log(val);
}
```

### 使用迭代器

``` js
for (let entry, itr = a[Symbol.iterator](); !(entry = itr.next()).done; ) {
    console.log(entry.value);
}
```

---

## 比较上述遍历方式

- `for` 这是最常见的遍历方式，浏览器都支持

- `for-in` 不推荐，两个原因：不能保证遍历的顺序是预期的；遍历可能会带出原型链上的属性

- `forEach()` 非常好用的遍历方式，ES5+，如果担心运行时资源消耗的问题，可以看看 [forEach and runtime cost](http://blog.niftysnippets.org/2012/02/foreach-and-runtime-cost.html)。缺陷是不能使用 `break`，但可以用 `try-catch` 来 hack

- `map()` ES5+，适用于“链式”场景，如 `a.map(i => i + i).sort();`

- `for-of` ES6+，适用于全部元素的遍历，缺陷是不知道 index

- 迭代器，ES6 新特性，大家可以慢慢玩

---
EOF
