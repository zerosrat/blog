title: RESTful API 实战
date: 2016-06-27 22:45:05
tags: REST
categories: Back End
description: 关于 REST 的一些理解
---

## 参考

> [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
> [理解 RESTful 架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)
> [怎样用通俗的语言解释什么叫 REST，以及什么是 RESTful？](https://www.zhihu.com/question/28557115)

## 什么是 REST

- REST 并不是指 rest 这个单词，而是几个单词的缩写

- REST 的全名是 Resource Representational State Transfer，可以翻译为资源在网络中以某种表现形式进行状态转移。

- 把单词进行拆解，Resource 是网络传输的数据，如 books，friends 等等；Representational 是数据表现的形式，如 JSON 或是 XML 等等；State Transfer 是状态的变化，用 HTTP 动词实现，如 GET 或是 POST 等等。

- 这样就不难看出，REST 描述了传输的数据、传输形式和传输动作。这样我们接下来看看由 REST 设计出的 RESTful API 的实战。

## RESTful API 实战

### HTTP verbs

常用的 HTTP 动词有：

- GET：取数据

- POST：新建或更新数据

- PUT：更新数据

- DELETE：删除数据

### 域名

- 可以是 `https://api.example.com` 或是 `https://example.org/api/`，后者我使用较多

- 后者用 Spring MVC 实现
```
ApplicationBoot.java

@Configuration
@ComponentScan(basePackages = ApplicationBoot.PACKAGE_NAMESPACE)
public class ApplicationBoot extends AbstractAnnotationConfigDispatcherServletInitializer {
	public static final String PACKAGE_NAMESPACE = "com.zerosrat.demo";

	@Override
	protected String[] getServletMappings() {
		return new String[] { "/api/admin/*" };
	}
	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[] { ApplicationBoot.class };
	}
	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[] { WebConfig.class };
	}
}
```

### 路径或网址

- 每个网址代表一个资源

- 规则：**不能** 有 **动词**，**不能** 有 **大写**。而是使用名词，最好是复数

- 例子：
<table><tr><th>HTTP verbs</th><th>url</th><th>目的</th></tr><tr><td>GET</td><td>https://example.org/api/books</td><td>查询所有书籍</td></tr><tr><td>GET</td><td>https://example.org/api/books/{id}</td><td>查询 id 为 {id} 的书籍</td></tr><tr><td>POST</td><td>https://example.org/api/books</td><td>添加一本书</td></tr><tr><td>DELETE</td><td>https://example.org/api/books</td><td>删除 id 为 {id} 的一本书</td></tr></table>

## 结语

最后再次强调：不能带动词和大写，而是使用复数名词