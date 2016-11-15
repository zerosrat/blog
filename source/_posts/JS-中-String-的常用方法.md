---
title: JS 中 String 的常用方法
date: 2016-11-15 16:42:29
tags: JavaScript
categories: Front End
---

## 参考

《JavaScript 高级程序设计》 第三版
《JavaScript 权威指南》 第六版

## Overview

![](http://7xoxnz.com1.z0.glb.clouddn.com/js-string-overview00.png)

> 注：本博客讲解的是 String 的常用方法，未提及的还有 `localeCompare()` `valueOf()`

<!-- more -->

## 现在开始吧

### charAt() / charCodeAt()
- 描述：前者返回字符串中的第 n 个字符， 后者返回字符串中第 n 个字符的编码
- 参数：n
- 例子
  ``` js
  var str = 'hello';
  console.log(str.charAt(1));  // 'e'
  console.log(str.charCodeAt(1));  // 101
  ```

### concat()
- 描述：将一个或多个字符串拼接并返回
- 参数：str, ...
- 例子
  ```js
  var str = 'hello';
  console.log(str.concat(' world'));  // 'hello world'
  console.log(str);  // 'hello'
  ```

###  indexOf() / lastIndexOf()
- 描述：前者从开头搜索一个字符串并返回位置，后者反向；为找到的话返回 -1
- 参数：substring, start?
- 例子
  ``` js
  var str = 'hello world';
  console.log(str.indexOf('o'));  // 4
  console.log(str.lastIndexOf('o'));  // 7
  ```

### replace()
- 描述：找到并替换字符串中的指定substring，第一个参数是要查找的值，可以是字符串或正则表达式，第二个参数是要替换成的字符串
- 参数：substring/regexp, replacement
- 例子
  ``` js
  var str = 'cat, bat, fat';
  str.replace('at', 'xxx');
  console.log(str);  // 'cxxx,bat,fat'
  str.replace(/at/g, 'yyy');
  console.log(str);  // 'cxxx,byyy,fyyy'
  ```

### rearch()
- 描述：根据一个正则 **正向** 找到并返回其 **第一个** 位置，参数不是正则的话会被转换成正则
- 参数：regexp
- 例子
  ``` js
  var str = 'cat, bat, fat';
  console.log(str.search(/at/g));
  ```

### match()
- 描述：根据正则找到一个或多个匹配结果
- 参数：regexp
- 例子
  ``` js
  var str = '1 plus 2 equals 3';
  console.log(str.match(/\d+/g));  // ['1','2','3']
  ```

> 话外音：[Fastest way to check a string contain another substring in Javascript?](http://stackoverflow.com/questions/5296268/fastest-way-to-check-a-string-contain-another-substring-in-javascript/)

### slice()
- 描述：提取并返回字符串，根据指定区间 [start,end)，若未指定end，区间为 [start,str.length-1]
- 参数：start, end?
- 例子
  ``` js
  var str = 'hello world';
  console.log(str.slice(3));  // 'lo world'
  console.log(str.slice(3,7));  // 'lo w'
  console.log(str);  // 'hello world'
  ```

### substring()
- 描述：提取并返回字符串，根据指定区间 [start,end)，若未指定end，区间为 [start,str.length-1]
- 参数：start,end?
- 例子
  ``` js
  var str = 'hello world';
  console.log(str.substring(3));  // 'lo world'
  console.log(str.substring(3,7));  // 'lo w'
  console.log(str);  // 'hello world'
  ```

> 话外音：[What is the difference between String.slice and String.substring?](http://stackoverflow.com/questions/2243824/what-is-the-difference-between-string-slice-and-string-substring)

### substr()
- 描述：提取并返回字符串，第一个参数为起始点，第二个参数为返回子串的长度
- 参数：start,length
- 例子
  ``` js
  var str = 'hello world';
  console.log(str.substr(3));  // 'lo world'
  console.log(str.substr(3,7));  // 'lo worl'
  console.log(str);  // 'hello world'
  ```

### split()
- 描述：根据指定字符串或正则，将字符串分割成数组
- 参数：delimiter,limit
- 例子
  ``` js
  var str = 'a b c';
  console.log(str.split(' '));  // ['a','b','c']
  console.log(str.split(''));  // ['a',' ','b',' ','c']
  console.log(str.split());  // ['a b c']
  ```

### toUpperCase() / toLowerCase()
- 描述：将字符串转为大写/小写
- 参数：无
- 例子
  ``` js
  var str = 'Hello world!';
  console.log(str.toUpperCase());  // 'HELLO WORLD!'
  console.log(str);  // 'Hello world'
  console.log(str.toLowerCase());  // 'hello world'
  console.log(str);  // 'Hello world'
  ```

### trim()
- 描述：删除字符串开头和结尾的空格并返回
- 参数：无
- 例子
  ``` js
  var str = '  Hello world!  ';
  console.log(str.trim());  // 'Hello world!'
  console.log(str);  // '  Hello world!  '
  ```

---
EOF
