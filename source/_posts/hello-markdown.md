title: MarkDown 语法手册
date: 2015-11-18 16:06:21
description: 如何优雅地使用MarkDown
categories: Blog
tag: MarkDown
---
## 前言

[Markdown](http://daringfireball.net/projects/markdown/) 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客。
<!-- more -->
***

## 常用语法

### 斜体和粗体

使用 \* 和 \** 分别表示斜体和粗体。

示例：

这是 *斜体*，这是 **粗体**。

### 分级标题

使用 === 表示一级标题，使用 --- 表示二级标题。

示例：

```
这是一个一级标题
============================

这是一个二级标题
--------------------------------------------------

## 这是一个三级标题
```

你也可以选择在行首加井号表示不同级别的标题 (H1-H6)，例如：# H1, ## H2, ### H3，#### H4。

### 外链接

使用 \[描述](链接地址) 为文字增加外链接。

示例：

这是去往 [本人github](https://github.com/zerosrat) 的链接。

### 无序列表

使用 *，+，- 表示无序列表。

示例：

- 无序列表项 一
- 无序列表项 二
- 无序列表项 三

### 有序列表

使用数字和点表示有序列表。

示例：

1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三

### 文字引用

使用 > 表示文字引用。

示例：

> 野火烧不尽，春风吹又生。

### 行内代码块

使用 \`代码` 表示行内代码块。

示例：

让我们聊聊 `html`。

### 代码块

使用 四个缩进空格 表示代码块。

示例：

    这是一个代码块，此行左侧有四个不可见的空格。

### 插入图像

使用 \!\[描述](图片链接地址) 插入图像。

示例：

![我的头像](http://7xoxnz.com1.z0.glb.clouddn.com/logo1.png)

***

## 语法进阶

### 删除线

使用 ~~ 表示删除线。

~~这是一段错误的文本。~~

### 加强的代码块

支持四十一种编程语言的语法高亮的显示，行号显示。

非代码示例：

```
$ sudo apt-get install vim-gnome
```

Python 示例：

```python
@requires_authorization
def somefunc(param1='', param2=0):
    '''A docstring'''
    if param1 > param2: # interesting
        print 'Greater'
    return (param2 - param1 + 1) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
```

JavaScript 示例：

``` javascript
/**
* nth element in the fibonacci series.
* @param n >= 0
* @return the nth element, >= 0.
*/
function fib(n) {
  var a = 1, b = 1;
  var tmp;
  while (--n >= 0) {
    tmp = a;
    a += b;
    b = tmp;
  }
  return a;
}

document.write(fib(10));
```


### 使用图标

使用 [font-awesome](http://fortawesome.github.io/Font-Awesome/3.2.1/icons/) 的图标，在文档中输入

    <i class="icon-weibo"></i>

即显示微博的图标： <i class="icon-weibo icon-2x"></i>

替换 上述 `i 标签` 内的 `icon-weibo` 以显示不同的图标，例如：

    <i class="icon-github"></i>

即显示github的图标： <i class="icon-github icon-2x"></i>

更多的图标和玩法可以参看 [font-awesome](http://fortawesome.github.io/Font-Awesome/3.2.1/icons/) 官方网站。
