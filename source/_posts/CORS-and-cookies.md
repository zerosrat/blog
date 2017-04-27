---
title: 跨源资源共享 ( CORS )机制与 Cookies
date: 2017-03-15 20:41:14
tags: [CORS, Cookies]
---

## 参考

> [HTTP访问控制(CORS)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

> [HTTP Cookies](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)

> [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

## 谈谈 CORS

CORS，全称 Cross-origin sharing.

当一个资源请求一个其它域名或者另外一个端口的资源时会产生一个跨域HTTP请求(cross-origin HTTP request)。比如说，http://domaina.example 的某HTML页面通过 `<img>` src 请求 http://domainb.foo/image.jpg。

出于安全考虑，浏览器会限制脚本中发起的跨域请求。比如，使用 XMLHttpRequest 发起 HTTP 请求就必须遵守同源策略。

跨源资源共享 ( CORS )机制让 Web 应用服务器能支持跨站访问控制，从而使得安全地进行跨站数据传输成为可能。
<!-- more -->

## 谈谈 Cookies

HTTP Cookie（也叫Web cookie或者浏览器Cookie）是服务器发送到用户浏览器并保存在浏览器上的一块数据，它会在浏览器下一次发起请求时被携带并发送到服务器上。

创建Cookie：当服务器收到HTTP请求时，可以在响应头里面增加一个Set-Cookie头部。浏览器收到响应之后会去除Cookie信息并保存，之后对该服务器每一次请求中都通过Cookie请求头部将Cookie信息发送给服务器。

## 场景

最近在做一个小项目，项目中是用 cookies(Client) + session(Server) 的机制来保存登录状态，如下图。

![](http://7xoxnz.com1.z0.glb.clouddn.com/cookies-session.png)

问题描述：登录之后，发现虽然登录的 http 请求返回了 Set-Cookie 头并且包含键值对，但是浏览器中却没 cookie 值，这让我很困惑。排除了其他原因后，注意到在开发环境中，因为前后台项目被部署在不同的端口下，所以后台配置了 CORS 实现跨域，cookie 没有成功设置可能是跨域带来的。之后，将前台项目和后台项目部署在同一域下，发现 cookie 成功设置了，这样一来，更加明确了问题的所在。

## 问题解决

CORS 请求默认是不会发送 Cookie 和认证信息的。要发送和保存 Cookie，要在分别在前端和后端进行设置。

前端的话，需要将 `withCredentitals` 属性设置为 true.
``` js
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

后端也要进行配置，`Access-Control-Allow-Credentials: true`，并且如果浏览器要发送 cookie 的话，`Access-Control-Allow-Origin` 就不能设为星号，其值必须指定明确的、与请求网页一致的域名。
