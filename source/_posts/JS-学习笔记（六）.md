title: JS 学习笔记（DOM篇 - 中）
date: 2016-06-01 21:21:02
tags: JavaScript
categories: Front End
description: DOM2 和 DOM3 的学习笔记以及事件处理程序和事件对象的学习笔记
---

## DOM2 和 DOM3

### 样式

### 遍历

### 范围

***

## 事件

- JavaScript 和 HTML 之间的交互是通过 **事件** 实现的

- 事件很早就出现了，被正式规范是在 DOM2 级的事件模块。现主流浏览器已实现“DOM2 级事件”模块的核心部分

### 事件流

- DOM2 级事件规定的事件流包括：事件捕获阶段、处于目标阶段和事件冒泡阶段
![DOM事件流](http://7xoxnz.com1.z0.glb.clouddn.com/dom_eventflow.jpg)

- 如上图，事件捕获是从 `document` 节点开始一直到预定目标之前（使用较少，因为老版本浏览器不支持）；事件冒泡是从接受节点开始逐级向上传播，主流浏览器支持冒泡到 `window` 对象

- IE8以及更早版本不支持 DOM 事件流，只支持事件冒泡

### 事件处理程序

响应某个事件的函数就叫做事件处理程序

#### HTML 事件处理程序

- 例子1
```
<input type="button" value="click" onclick="alert('hi');" />
```

- 例子2
```
<script type="text/javascript">
	function sayHi () {
		alert("hi");
	}
</script>
<input type="button" value="click" onclick="sayHi()" />
```

- 缺点是：如果用户在页面解析事件处理函数之前就触发了事件，会引发错误；HTML 与 JS 代码紧密耦合在了一起

#### DOM0 级事件处理程序

- 将函数赋值给事件处理程序属性

- 例子
```
var btn = document.getElementById("mybtn");
//add event
btn.onclick = function () {
	alert(this.id); //"mybtn"
};
//remove event
btn.onclick = null;
```

#### DOM2 级事件处理程序

- 使用方法 `addEventListener()` 和 `removeEventListener()`

- 例子1
```
var btn = document.getElementById("mybtn");
//第一个参数是要处理的事件名；第二个是事件处理函数；第三个若为 true 表示在捕获阶段调用函数，否则在冒泡阶段调用
btn.addEventListener("click", function () {
	alert(this.id);
}, false);
```

- 例子1的事件处理函数是匿名的，这样就无法移除了。除非使用函数表达式，如下
```
var btn = document.getElementById("mybtn");
var handler = function () {
	alert(this.id);
} 
//add event
btn.addEventListener("click", handler, false);
//remove event
btn.removeEventListener("click", handler, false);
```

- 使用 DOM2 级事件处理程序的主要好处是可以在一个节点上添加多个事件处理程序，触发时 **按添加顺序依次调用**

- 一般是在冒泡阶段调用事件处理函数，这样可以最大限度地兼容各种浏览器

#### IE 事件处理程序

- 使用 `attachEvent()` 和 `detachEvent()`

- 例子1
```
var btn = document.getElementById("mybtn");
btn.attachEvent("onclick", function () {
	alert(this === window); //true
})
```

- 事件处理程序会在全局运行，而不是其所属元素的作用域中，所以上面代码 `this === window` 为 `true`

- 可以在一个节点上添加多个事件处理程序，触发时按添加顺序的 **反序** 调用

- 例子2
```
var btn = document.getElementById("mybtn");
var handler = function () {
	alert("hi");
} 
//add event
btn.attachEvent("onclick", handler);
//remove event
btn.detachEvent("onclick", handler);
```

#### 跨浏览器的事件处理程序

- 使用能力检测，例子如下
```
var EventUtil = {

	addHandler : function (element, type, handler) {
		if (element.addEventListener) {
			element.addEventListener(type, handler, false);
		} else if (element.attachEvent) {
			element.attachEvent("on" + type, handler)
		} else {
			element["on" + type] = handler;
		}
	},

	removeHandler : function (element, type, handler) {
		if (element.removeEventListener) {
			element.removeEventListener(type, handler, false);
		} else if (element.detachEvent) {
			element.detachEvent("on" + type, hander);
		} else {
			element["on" + type] = null;
		}
	}
};

var btn = docment.getElementById("mybtn");
var handler = function () {
	//to-do
};
EventUtil.addHander(btn, "click", handler);
EventUtil.removeHander(btn, "click", handler);
```

### 事件对象

在触发 DOM 上的某个事件时，会产生一个事件对象 `event`，这个对象中包含着所有与事件有关的信息

#### DOM 中的事件对象

- 兼容 DOM 的浏览器会将一个 `event` 对象传入到事件处理程序中
```
var btn = document.getElementById("mybtn");
btn.onclick = function (event) {
	alert(event.type); //click
};
btn.addEventListener("click", function (event) {
	alert(event.type); //click
}, false);
```

- 触发的事件类型不同，可用的 `event` 对象的属性和方法也不同

- 常用的事件属性有 `type` 、`eventPhase` 、 `currentTarget` 和 `target`，常用的事件方法有 `preventDefault()` 和 `stopPropagation()`.完整的成员可以参考 [这里的标准 Event 属性和方法](http://www.w3school.com.cn/jsref/dom_obj_event.asp)

- `type` 是指事件类型，可以参考 [这里的事件句柄](http://www.w3school.com.cn/jsref/dom_obj_event.asp)

- `eventPhase` 的值可以是 [1, 2, 3]，分别对应捕获、处理和冒泡阶段

- `currentTarget` 和 `target` 不一定相等，前者是等于 `this`，而后者是真正触发事件的目标

- `preventDefault()` 用于阻止特定事件的默认行为，如点击链接会导航到其 `href` 指定的 URL

- `stopPropagation()` 用于取消事件的捕获或冒泡

#### IE 中的事件对象

- 使用 DOM0 级添加事件处理程序时，`event` 对象作为 `window` 对象的一个属性存在
```
var btn = document.getElementById("mybtn");
btn.onclick = function () {
	var event = window.event;
	alert(event.type); //click
};
```

- 如果是用 `attachEvent()`，就会有一个 `event` 对象传入到事件处理程序中
```
var btn = document.getElementById("mybtn");
btn.attachEvent("onclick", function (event) {
	alert(event.type); //click
});
```

- IE 的 `event` 对象的 `srcElememt` 属性同 `target`，`returnValue` 属性相当于 `preventDefault()` (设置为 `false` 来阻止默认行为), `cancelBubble` 属性相当于 `stopPropagation()` 方法(设置为 `true` 来取消冒泡)。完整的属性可参考 [这个的 IE 属性](http://www.w3school.com.cn/jsref/dom_obj_event.asp)

#### 跨浏览器的事件对象

- 增强自定义的 `EventUtil` 对象
```
var EventUtil = {

	addHandler : function (element, type, handler) {
		if (element.addEventListener) {
			element.addEventListener(type, handler, false);
		} else if (element.attachEvent) {
			element.attachEvent("on" + type, handler)
		} else {
			element["on" + type] = handler;
		}
	},

	removeHandler : function (element, type, handler) {
		if (element.removeEventListener) {
			element.removeEventListener(type, handler, false);
		} else if (element.detachEvent) {
			element.detachEvent("on" + type, hander);
		} else {
			element["on" + type] = null;
		}
	},

	getEvent : function (event) {
		return event ? event : window.event;
	},

	getTarget : function (event) {
		return event.target || event.srcElement;
	},

	preventDefault : function (event) {
		if (event.preventDefault) {
			event.preventDefault();
		} else {
			event.returnValue = false;
		}
	},

	stopPropagation : function (event) {
		if (event.stopPropagation) {
			event.stopPropagation();
		} else {
			event.cancelBubble = true;
		}
	}
};

var btn = docment.getElementById("mybtn");
var handler = function (event) {
	event = EventUtil.getEvent(event);
	var target = EventUtil.getTarget(event);
};
EventUtil.addHander(btn, "click", handler);
```

