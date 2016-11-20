---
title: JS 学习笔记（DOM篇 - 下）
date: 2016-06-04 10:10:10
tags: JavaScript
categories: Front End
description: 事件类型、性能方面和模拟事件的学习笔记
---

## 事件类型

DOM3 级事件将事件进行了分类：UI事件、焦点事件、鼠标事件、滚轮事件、文本事件、键盘事件、复合事件和变动事件

### UI 事件

- 现有的 UI 事件有 `DOMActivate` 、 `load` 、 `unload` 、 `abort` 、 `error` 、 `select` 、 `resize` 和 `scroll`

- `DOMActivate` 已经在 DOM3 级事件中被抛弃，不建议使用；除它之外的事件在 DOM2 级事件中都归为 HTML 事件

- `var isSupported = document.implementation.hasFeature("HTMLEvents", "2.0")`; 检测浏览器是否支持 DOM2 级的 HTML 事件

- `var isSupported = document.implementation.hasFeature("UIEvent", "3.0");` 检测浏览器是否支持 DOM3 级的 UI 事件

- `load` 是 **最常用** 的事件之一；`<img>` 指定了 `src` 就会开始下载，而 `<script>` 设置了 `src` 并且被添加到文档才会开始下载，`<link>` 同 `<script>` 类似
``` js
EventUtil.addHandler(window, "load", function () {
    var script = document.createElement("script");
    EventUtil.addHandler(script, "load", function (event) {
        alert("loaded");
    })
    script.src = "script.js";
    document.getElementByTagName("head")[0].appendChild(script);
})
```

### 焦点事件

- 有6个焦点事件：`focusout` 、 `focusin` 、 `blur` 、 `DOMFocusOut` 、 `focus` 和 `DOMFocusIn`

- 当焦点从一个页面中元素移动到另一元素，事件触发的顺序会按上面排列的顺序来触发

- 使用 `var isSupported = document.implementation.hasFeature("FocusEvent", "3.0");` 确定浏览器是否支持上面的事件

- `focus` 和 `blur` 不会冒泡，所有浏览器支持；`focusin` 和 `focusout` 冒泡；另外两个在 DOM3 级事件已被弃用

- 最常用的 `focusin` 和 `focusout`，但即使 `focus` 和 `blur` 不会冒泡，仍可以在捕获阶段监听到

### 鼠标事件

- 鼠标事件是 Web 开发 **最常用** 的一类事件，DOM3 级规定了9个鼠标事件：`click` 、 `dbclick` 、 `mousedown` 、 `mouseenter` 、 `mouseleave` 、 `mousemove` 、 `mouseout` 、 `mouseover` 和 `mouseup`

- 一个双击的事件触发顺序：`mousedown` 、 `mouseup` 、 `click` 、 `mousedown` 、 `mouseup` 、 `click` 和 `dbclick`。可以用 `var isSupported = document.implementation.hasFeature("MouseEvents", "2.0");` 检测这4个事件

- `var isSupported = document.implementation.hasFeature("MouseEvent", "3.0");` 检测9个事件

- `event` 对象的 `clientX` 、 `clientY` 、 `pageX` 和 `pageY` 属性，分别保存了鼠标在视口中的位置信息和页面中的位置信息

- `event` 对象的 `screenX` 和 `screenY` 保存了鼠标在屏幕中的位置

- `event` 对象的 `shiftKey` 、 `ctrlKey` 、 `altKey` 和 `metaKey`，可以检测点击鼠标时是否按下了相应的键

### 滚轮事件

- 就是 `mousewheel` 事件

### 文本事件

- 只有一个文本事件 `textInput`
``` js
var textbox = document.getElementById("myText");
EventUtil.addHandler(textbox, "textInput", function (event) {
    event = EventUtil.getEvent(event);
    alert(event.data); //在文本框按下一个键时就会触发
})
```

### 键盘事件

- DOM0 级事件有：`keydown` 、 `keypress` 和 `keyup`

- `event` 对象的 `keyCode` 和 `charCode` 属性，表示按下键的 ASCII 码，后者可以用 `String.fromCharCode()` 将其转换成实际字符

### 变动事件

- DOM 结构变化时触发

### HTML5 事件

- `contextmenu` 事件，显示上下文菜单

- `beforeunload` 事件，在浏览器卸载页面之前触发。在 `unload` 之前执行

- `DOMContentLoaded` 事件，形成完整 DOM 树后触发

## 内存和性能

添加到页面上事件处理程序的数量将关系到页面的整体性能

### 事件委托

- 利用事件冒泡，只指定一个事件处理程序，来管理某类型的所有事件

- 例子
``` js
var list = document.getElementById("myList");
EventUtil.addHandler(list, "click", function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    switch (target.id) {
        case "li1":
            //todo
            break;
        case "li2":
            //todo
            break;
    }
})
```

- 最适合采用事件委托的事件有 `click` 、 `mousedown`、 `mouseup` 、 `keydown` 、 `keyup` 和 `keypress`

### 移除事件处理程序

- 在不需要的时候移除事件处理程序

- 当从文档中移除带有事件处理程序的元素时，应移除其事件处理程序

- 卸载页面时，可能会有没有清理干净的处理程序滞留在了内存中，所以最好通过 `onunload` 事件来移除所有事件处理程序

## 模拟事件

### DOM 中的事件模拟

- 使用 `document.createEvent()` 创建 `event` 对象，参数是一个字符串，在 DOM2 级中可以是：`UIEvents` 、 `MouseEvents` 、 `MutationEvents` 和 `HTMLEvents`。而 DOM3 级中字符串为单数

- 创建 `event` 对象后，还要使用与事件相关的信息将其初始化，每种类型的事件都有其对应的初始化方法

- 最后使用 `dispatchEvent()` 来触发事件

### IE 中的事件模拟

- 同DOM 中的事件模拟相似，IE 中的事件模拟也可分别三步

- 使用 `document.createEventObject()` 创建 `event` 对象，但不传入参数

- 对 `event` 对象的属性添加必要的信息

- 调用 `fireEvent()` 触发事件，如 `mybtn.fireEvent("onclick",event);`
