---
title: JS 学习笔记（DOM篇 - 上）
date: 2016-05-26 19:09:42
tags: JavaScript
categories: Front End
description: DOM 1级 的相关知识
---

## Before starting

在上一节的 BOM 学习之后，我们再来学习一下 DOM 吧。

###  什么是 DOM

- DOM 是用于 XML 或 HTML 的一个 API，它将整个页面映射成一个多层节点 (Node) 结构。
![HTML DOM 树](http://7xoxnz.com1.z0.glb.clouddn.com/dom.gif)

- DOM 并不只是针对 JavaScript 的，很多别的语言也实现了 DOM

### DOM 级别

- DOM1级，由 DOM Core 和 Dom HTML 组成，前者规定如何映射文档结构，后者则是在前者基础上添加了对 HTML 的对象和方法

- DOM2级在上一级的基础上进行了扩充，引入了 DOM 事件、DOM 样式等新模块

- DOM3级在前一级的基础上进行了扩充，引入了加载和保存文档的方法 (DOM Load and Save 模块)；验证文档的方法 (DOM Validation 模块)

***

## Node 类型

- 在开头的图 (HTML DOM 树) 中可以看到每个节点都是 Node 类型实现的，有12种节点类型，如文档节点是 Document 类型的。

<table><tr><th>Node 类型</th><th width="32%">nodeType</th><th>nodeName</th><th>nodeValue</th><th>parentNode</th></tr><tr><td>Document 类型</td><td>Node.DOCUMENT_NODE(9)</td><td>#document</td><td>null</td><td>null</td></tr><tr><td>Element 类型</td><td>Node.ELEMENT_NODE(1)</td><td>元素的标签名</td><td>null</td><td>Document or Element</td></tr><tr><td>Text 类型</td><td>Node.TEXT_NODE(3)</td><td>#text</td><td>节点的文本值</td><td>Element</td></tr><tr><td>Comment 类型</td><td>Node.COMMENT_NODE(8)</td><td>#comment</td><td>注释内容</td><td>Document or Element</td></tr><tr><td>CDATASection 类型</td><td>Node.CDATA_SECTION_NODE(4)</td><td>#cdata-section</td><td>CDATA 区域中内容</td><td>Document or Element</td></tr><tr><td>DocumentType 类型</td><td>Node.DOCUMENT_TYPE_NODE(10)</td><td>doctype 的名字</td><td>null</td><td>Document</td></tr><tr><td>DocumentFragment 类型</td><td>Node.DOCUMENT_FRAGMENT_NODE(11)</td><td>#document-fragment</td><td>null</td><td>null</td></tr><tr><td>Attr 类型</td><td>Node.ATTRIBUTE_NODE(9)</td><td>特性的名字</td><td>特征的值</td><td>null</td></tr></table>

- 每个节点都有 `childNodes` 属性，保存了一个 `NodeList` 对象（非 Array），它是基于 DOM 结构 **动态** 查询的结果，DOM 结构的变化能自动反映在 `NodeList` 对象中。可以使用 `item()` 方法来访问 `NodeList` 对象

- 其他一些属性 `parentNode` 、 `previousSibling` 、 `nextSibling` 、 `firstChild` 、 `lastChild` 和 `ownerDocument`

下面来依次看看各个类型的 Node

### Document 类型（常用）

- `document` 对象是 `HTMLDocument` （继承自 `Document` 类型）的一个实例

- HTML 页面只有一个文档元素，文档中其他所有元素都包含在文档元素中

- 可通过 `documentElement` or `childNodes[0]` 来访问子节点 `<html>` 元素，所有浏览器都支持 `documentElement` and `body` 属性

- 通过 `getElementById()` 、 `getElementsByTagName()` and `getElementsByName` 查找元素，后两者都会返回 `HTMLCollection` 的对象
```
var images = document.getElementsByTagName("img");
var myImg = images.namedItem("myImgae");
```

- 通过 `write()` 将输出流写入到网页里

### Element 类型（常用）

- `nodeName` and `tagName` 值相等，它们都表示元素的标签名。在 HTML 中返回的标签全部 **大写**

- 常用特性 `id` and `className`

- 特性操作 `getAttribute()` 、 `setAttribute()` 和 `removeAttribute()`。有关自定义属性的操作使用 `getAttribute()` 和 `setAttribute()`，而不是直接在 `id` 上进行操作

- 属性遍历时可以使用 `attributes`

- `document.createElement()` 创建新元素
```
var div = document.createElement("div");
div.id = "div_id";
div.className = "div_class";
document.body.appendChild(div);
```
- 元素也支持 `getElementByTagName()`，常用于 `<ol>` 、 `<ul>` 等标签

### Text 类型

- `createTextNode()` 创建文本节点
```
var element = document.createElement("div");
var textNode = document.createTextNode("hello");
element.appendChild(textNode);
document.body.appendChild(element);
```

- 通过 `normalize()` 合并相邻文本节点，通过 `splitText()` 分割一个文本节点，后者是提取数据的常用方式

### Comment 类型

- 同 `Text` 类型有相同的基类，也可通过 `nodeValue` or `data` 取得内容

### CDATASection 类型

- CDATA 区域只出现在 XML 文档中

### DocumentType 类型

- 并不常用，只有 Firefox、Safari、Opera 和 Chrome 4.0+ 支持

### DocumentFragment 类型

- 所有节点类型中，只有它在文档中没有对应标记，主要作用是被作为一个临时“仓库”，通过 `createDocumentFragment()` 创建

- 一个例子
```
var fragment = document.createDocumentFragment();
var ul = document.getElementById("list");
for(var i = 0; i < 3; i++){
    var li = document.createElement("li");
    var text = document.createTextNode("li-"+i);
    li.appendChild(text);
    fragment.appendChild(li);
}
ul.appendChild(fragment);
```
上面代码进行了一次性添加，避免了多次渲染

### Attr 类型

- 不推荐直接访问特性节点。使用 `getAttribute()` 、 `setAttribute()` 和 `removeAttribute()` 也比操作特性节点更方便

***

## DOM 操作技术

### 动态脚本

- 插入外部脚本
```
var script = document.createElement("script");
script.type = "text/javascript";
script.src = "js/lib.js";
document.body.appendChild(script);
```

- 插入 JavaScript 代码
```
var script = document.createElement("script");
script.type = "text/javascript";
var code = "function sayHi(){alert('hi');}";
try {
    script.appendChild(document.createTextNode(code));
} catch(ex) {
    script.text = code;
}
document.body.appendChild(script);
```

### 动态样式

- 能把 CSS 样式包含到 HTML 页面的元素有两个。其中，`<link>` 元素用于包含来自外部的文件，`<style>` 元素用于指定嵌入的样式

- 动态指定外部文件
``` js
var link = document.createElement("link");
link.rel = "stylesheet";
link.type = "text/css";
link.href = "css/style.css";
var head = document.getElementsByTagName("head")[0];
head.appendChild(link);
```

- 动态指定嵌入的样式
``` js
var style = document.createElement("style");
style.type = "text/css";
var code = "body{background-color:red;}";
try {
    style.appendChild(document.createTextNode(code));
} catch(ex) {
    style.styleSheet.cssText = code;
}
var head = document.getElementsByTagName("head")[0];
head.appendChild(style);
```

- 操作 NodeList 时

- 一个例子
``` js
var divs = document.getElementsByTag("div");
for(var i = 0, len = divs.length; i < len; i++){
    var div = document.createElement("div");
    document.body.appendChild(div);
}
```
以上预先存储了 divs 的长度，避免了无限循环的问题，因为 `NodeList` 是动态的

- 尽量减少访问 `NodeList` 的次数，每次访问它都会运行一次基于文档的查询。操作对性能的影响较大

***

## DOM 扩展

### Selectors API

- 这个 API 致力于让浏览器原生支持 CSS 查询，其中的两个核心方法是： `querySelector()` 和 `querySeletorAll()`

- `querySelector()` 接收一个 CSS 选择符参数，返回匹配的第一个元素

- `querySelectorAll()` 参数同上方法，返回的是 `NodeList` 的实例

### 元素遍历

- 不同浏览器使用 `childNodes` and `firstChild` 等属性时行为不一致，如对于元素间的空格，IE9 以及之前版本不会返回文本节点。而 Element Traversal API 正是为了解决这个差异性问题而制定出来的规范

- Element Traversal API 为 DOM 元素添加了5个属性：`childElementCount` 、 `firstElementChild` 、 `lastElementChild` 、 `previousElementSibling` and `nextElementSibling`. 使用它们就不用担心空白文本节点

### HTML5

- `getElementsByClassName()` 是 H5 添加的最受欢迎的方法，可通过 `document` 对象和所有 HTML 元素

- 通过 `classList` 属性的 `add()` 、 `contains()` 、 `remove()` and `toggle()` 方法来更方便对元素的 `class` 进行操作

- 通过 `document.activeElement` 引用当前获得焦点的元素，通过 `document.hasFocus()` 确定文档是否获得了焦点

- 通过 `document.readyState` 的值（`loading` or `complete`），来确定文档是否已经加载完毕

- 通过 `document.compatMode` 的值（`CSS1Compat` or `BackCompat`），来确定渲染模式是标准的还是混杂的

- `document.charset` 表示文档中实际使用的字符集

- HTML5 规定给元素加自定义属性时，名字要加前缀 `data-`，添加后可以通过元素的 `dataset` 属性来访问
    ``` js
    <div id="myDiv" data-name="haha"></div>

    var div = document.getElementById("myDiv");
    var name = div.dataset.name;
    ```

- `innerHTML` 操作元素的子节点。读模式下返回元素的所有子节点的 HTML 标记，写模式下会根据新制定的值创建 DOM 树来替代原来的所有子节点

- `outHTML` 操作元素及其子节点。读模式下返回元素和它的子节点的 HTML 标记，写模式下会用新的 DOM 树来替代原来的元素和它的子节点

- 通过调用元素的 `scrollIntoView()` 方法来滚动到元素位置

### 专有扩展

- 原本是一些浏览器独有的扩展，但被收纳为标准了

- 元素的 `children` 属性不会包含文本节点，除此之外，`children` and `childNodes` 没有什么区别

- 通过 `contains()` 检验某个节点是不是另一个节点的后代

- 通过 `innerText` 读取一个元素下的文本内容
