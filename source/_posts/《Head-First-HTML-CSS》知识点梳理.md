title: 《Head First HTML & CSS》知识点梳理
date: 2016-08-16 21:19:59
tags: [HTML, CSS]
categories: Front End
description: 《Head First HTML & CSS》的知识点梳理，主观性较强。主要记录的是自己比较生疏的知识点和非常重要的知识点。
---

## 开始了解 HTML

- `HTML` 即 HyperText Markup Language
- 在 `<style type="text/css"></style>` 中插入样式
- `CSS` 即 Cascading Style Sheet
- `HTML` 中的 ML 是指描述网页结构的标记语言；HT 是指把一个网页链接到其他网页，得益于 `<a>` 元素

## 深入理解超文本

- `<a>` 元素的 `href` 属性告诉浏览器链接的目的地，是 hypertext reference 的简写
- 把 `<img>` 放进 `<a>` 或是设置 `<a>` 的 `background-image` 属性，实现可点击的图片
- 相对路径与绝对路径，HTML 路径必须使用 “/” 分隔符

## 网页创建

- 使用 `<q>` 让被引用的文字的两侧显示双引号，更好的结构化代码
- 少量引用使用 `<q>` 一段引用使用 `<blockquote>`
- `block` 与 `inline` 元素
- 空元素，如 `<br>` 和 `<img>`
- 自定义列表的元素：`<dl>` & `<dt>` & `<dd>` 
```
<dl>
   <dt>计算机</dt>
   <dd>用来计算的仪器 ... ...</dd>
   <dt>显示器</dt>
   <dd>以视觉方式显示信息的装置 ... ...</dd>
</dl>
```
- `<img>` 是 inline 元素，但却有 block 的行为；`<br>` 是 inline；`<hr>` 是 block
- 输入文本时，用 `&lt` 代表 `<` ；用 `&rt` 代表 `>`；用 `&amp` 代表 `&`
- `<pre>` 元素可定义预格式化的文本。被包围在 pre 元素中的文本通常会保留空格和换行符。而文本也会呈现为等宽字体。

## 开始链接

- 当点击一个相对链接时，浏览器会在后头根据相对路径和你所点击的网页的路径生成一个绝对路径
- 当在浏览器输入 `http://www.google.com` 时，web 服务器会尝试在目录中定位默认文件，一般是 `index.html`
- `<a>` 中使用 `title` 属性，当鼠标移到元素上时会显示属性中的内容以解释链接；
- 在 `<a>` 中使用 `id` 属性来更加精准的定位
```
<a href="#target">to target</a>
...
<a id="target">I am the target</a>
```
- 在 `<a>` 中设置 `target="_blank"` 来打开一个新的窗口

## 认识媒体

- 图片格式对比：jpeg 是有损的，不支持透明，适合于图片；gif 是无损的，支持透明，适合 logo、线条画；png 是无损的，支持透明，但尺寸较大，适合小图
- `<img>` 使用 `alt` 属性，当图片加载失败时显示属性中的内容
- 可以给图片单独指定他的 `width` 或是 `height`
- `<img>` 是内联元素

## 严格的 HTML

- `<meta http-equiv="Content-Type" content="text/html; charset=utf-8">`
- 坚持标签小写
- 严格 HTML 4.01：网页始终以一个 `DOCTYPE` 开头；所有内联元素和文本元素必须在块元素中；内联元素不可嵌套块元素；`<p>` 不可嵌套块元素；`<blockquote>` 只可嵌套块元素；`<a>` 不可自我嵌套；空元素不可再嵌套

## 转到 XHTML

- 需要在 `<html>` 中添加 `xmlns` 和 `lang` 和 `xml:lang`
- 空元素以 `/>` 结尾

## 开始学习 CSS

- `<link type="text/css" rel="stylesheet" href="xxx.css" >`
- sans-serif 字体没有衬线
- 子元素会继承父元素的一些样式；子元素自己的样式优先于继承而来的样式
- 使用 `/** comment **/` 注释

## 字体和颜色样式

- `font-family` 有几大类：`serif` `sans-serif` `cursive` `fantasy` `monospace` 
- `font-size` 的常用单位 `px` 和 `em`，是指一个字顶部到底部的高度
- `color` 可使用内置单词表示或是 `rbg( , , )` 或是十六进制，其中十六进制最常用
- `font-weight` 可使用 `lighter` `normal` `bold` `bolder`
- `text-decoration` 可使用 `none` `underline` `overline` `line-through` `blink`
- `font-size` 可以设置百分比，表示的是相对于父元素的大小，em 表示时亦然
- `font-style` 可选的值有 `normal` `italic` `oblique` `inherit`
- 256色的屏幕会有web安全色的问题，但这已成为历史

## 盒模式

- 元素由内到外依次为内容区、内边距、边框和外边距，他们之间没有互相依赖
- `background-clip`(CSS3) 属性规定背景的绘制区域，默认是 `border-box`，即背景被裁剪到边框
- `background-repeat` 的默认值是 `repeat`，其他可能的值有：`repeat-x` `repeat-y` `no-repeat` `inherit`
- `background-position` 默认是表示左上角
- `border-style` `border-width` `border-color` 分为代表边框的风格、宽度和颜色

## div和span

- 使用 `<div>` 和 `<span>` 的目的是更好的展示结构，但是不要为了结构而结构地滥用
- width 属性值用来定义内容区的宽度
- 块元素默认宽度和高度是 `auto`
- `text-align` 只能用于块元素，设置其中的文本对齐方式
- 给 `line-height` 设置值而不带单位，表示相对于自己 `font-size` 的值
- border 和 background 的缩写顺序是随意的
- [font 的缩写](http://www.w3school.com.cn/cssref/pr_font_font.asp)
- 伪类的合理顺序是：link，visited，focus，hover，active

## 布局和排版 

- CSS 布局模型：流动模型、浮动模型和层模型
- 垂直相邻的两个块级元素会发生重叠（流动模型中）
- 浮动模型中设置 `float` 进行漂移
- 层模型包含绝对布局、相对布局和固定布局
- 绝对布局和固定布局会脱离正常的流，相对布局会留下一块区域
- 绝对布局的偏移是相对于页面，固定布局的偏移是相对于浏览器

## 表格和更多列表

- `<table>` 中的 `<caption>` 设置表格标题
- `caption-side` 表示标题的位置，可选的值有 `top` 和 `bottom`，默认是前者
- 表格中的元素没有外边距，取而代之的是 `border-spacing`
- 通过设置 `border-collapse: collapse` 消除边框间距
- 设置 'colspan' 和 `rowspan` 扩展元素的大小
- 使用 `list-style-type` 改变列表的标志
- 使用 `list-style-image` 自定义列表的标志
- 使用 `text-align` 和 `vertical-align` 改变表格数据的对齐方式

## 表单

-  相关的单选框或复选框的 `name` 值必须相同
-  `<select>` 元素的 `name` 指定了他的 `<option>`的 `name`
-  使用 `fieldset` + `<legend>` 绘制一个“标签框”
-  `<select>` 加上 `multiple="multiple"`，把单选变成多选
-  给提交按钮设置 `value` 来改变其上的文字
-  `<input>` 的 `type` 可以是 `text` `submit` `radio` `checkbox` `password` 等

---
EOF