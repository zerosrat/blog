title: JS 学习笔记（表单脚本篇）
date: 2016-06-13 21:38:26
tags: JavaScript
categories: Front End
description: 与表单相关的操作
---

## 基础知识

- HTML 中表单是由 `<form>` 元素来表示的，是 `HTMLElement` 类型的元素

- 可以通过 `getElementById()` or `document.forms` 来取得其引用

### 提交表单

- 将 `button` or `input` 的 `type` 设置为 `"submit"`，或是将 `<input>` 的 `type` 设置为 `"image"`，点击按钮或图像按钮，就会提交表单

- 点击按钮时，先触发 onsubmit 事件，再向浏览器发送请求。可以通过阻止 submit 事件的默认行为来取消提交
```
var form = document.forms[0];
EventUtil.addHandler(form, "submit", function (event) {
	event = EventUtil.getEvent(event);
	EventUtil.preventDefault(event);
});
```

- 可以使用 form 的 `submit()` 方法提交表单，**不会** 触发 onsubmit 事件

- 解决重复提交表单问题的方法有：第一次提交后禁用提交按钮；利用 onsubmit 事件处理程序取消后续提交操作

### 重置表单

- 将 `button` or `input` 的 `type` 设置为 `"reset"`，点击按钮，就会重置表单

- 可以使用 form 的 `reset()` 方法重置表单，**会** 触发 onreset 事件

### 表单字段

- 可以使用 form 的 `elements` 属性，里面有表单所有子元素的集合

- 可以通过 `form.elements["color"]` 取得表单中 `name="color"` 的一个 NodeList 集合

- 表单子元素的常用共有属性有 `disable` 、 `name`  and `value`， 属性是可以动态修改的

- 第一次点击后禁用提交按钮的例子
```
EventUtil.addHandler(form, "submit", function (event) {
	event = EventUtil.getEvent(event);
	var btn = form.elements["submit-btn"];
	btn.disable = true; //禁用按钮
});
```

- 不同浏览器的 submit 和 click 事件的执行顺序是不同的

- HTML5 为表单字段新增了 antofocus 属性，设置后可以自动聚焦 `<input type="text" autofocus />`

***

## 文本框脚本

- 可以使用 `<input>` 实现单行文本框，使用 `<textarea>` 实现多行文本框

- 建议修改 `value` 属性来读取或设置文本框的值，而不是使用 `setAttribute()` 方法

### 选择文本

- 通过文本框的 `select()` 方法来全选文本

- 选择了文本框的文本时会触发 select 事件

- 可以通过 HTML5 提供的 `selectionStart` and `selectionEnd` 来获得选择的文本
```
function getSelectedTxt (textbox) {
	return textbox.value.substring(textbox.selectionStart, textbox.selectionEnd);
}
```

### 过滤输入

- 例子，只输入数字
```
EventHandler.addHandler(textbox, "keypress", function (event) {
	event = EventUtil.getEvent(event);
	var charCode = EventUtil.getCharCode(event);
	if(!/\d/.test(String.fromCharCode(charCode)) && charCode > 9) {
		EventUtil.preventDefault(event);
	}
})

```
### 自动切换焦点

### HTML5 约束验证 API

***

## 选择框脚本

- 选择框通过 `<select>` and `<option>` 元素创建

- 推荐使用 `text` or `value` 属性来访问或修改选项文本或值的值
```
var selectbox = document.forms[0].elements["selectbox"];
var text = selectbox.options[0].text;
var value = selectbox.options[0].value;
```

### 选择选项

- 使用 `selectedIndex` 属性获得选中项

- 使用 `selected` 属性判断是否选中

### 添加选项

- 法一：使用 DOM
```
var newOption = document.createElement("option");
newOption.appendChild(document.createTextNode("option text"));
newOption.setAttribute("value", "option value");
selectbox.appendChild(newOption);
```

- 法二：使用 Option 构造函数
```
var newOption = new Option("option text", "option value");
selectbox.appendChild(newOption);
```

- 法三：使用选择框的 `add()`（最佳）
```
var newOption = new Option("option text", "option value");
selectbox.add(newOption, undefined);
```

### 移除选项

- 法一：使用DOM
```
selectbox.removeChild(selectbox.options[0]);
```

- 法二
```
selectbox.remove(0);
```

- 法三
```
selectbox.options[0] = null;
```

***