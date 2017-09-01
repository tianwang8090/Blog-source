---
title: 3种Web会话管理方式
date: 2017-09-01 14:45:04
categories: 
- web
- http
tags:
- web
- session
- cookie
icon: fa-comments
---

# 3种Web会话管理方式
**无状态的HTTP**
> 一次请求结束、链接断开。服务器再收到下一个请求、它就不知道是哪个用户发过来了。但是服务器会知道是从哪个客户端发起的、不过我们管理的是用户、不是客户端。所以就有了会话管理​​

<!-- more -->

## server端session
### 过程
1. 用户第一次请求 --> 服务器创建session对象(每个session会分配有唯一id，以及设置有失效时间) --> 服务器通过cookie返回sessionid到浏览器
2. 用户请求 --> 服务器获取cookie里的sessionid --> 验证session有效 --> 服务器延长失效时间
3. session超过失效时间 --> 服务器销毁该session --> 用户请求 --> 服务器创建新session --> 重新登录
4. 用户登录成功 --> 服务器往session里设置登录凭证
5. 用户请求 --> sessionid通过cookie携带发送到服务器 --> 服务器验证对应session的登录凭证 --> 响应请求
6. 用户登出 --> 服务器清除登录凭证

### 优点
1. Java、.net、PHP原生支持，开发简单
2. 安全性好。只要sessionid足够随机

### 缺点
1. 多用户同时在线会占据较大内存
2. 应用采用集群部署时需要处理多台服务器之间session共享的问题
3. 多台服务器之间共享session时，还可能需要处理跨域问题
4. 不适合用在native app：不好管理cookie
5. 不适合用来做纯API服务的登录认证

### 解决方法
- 对缺点1和2：可以采用中间服务器管理session
- 对缺点3：前后端解决sessionid的cookie跨域即可
- 实际使用时可以采用单点登录框架

## cookie-base
### 过程
1. 用户登录 --> 服务器创建登录凭证、数字签名、对称加密 --> key-value形式写入cookie
2. 用户请求 --> 服务器获取cookie、解密、数字签名认证 --> 判断登录凭证有效期 --> 重新登录/响应请求

### 优点
1. 减轻服务端内存压力，只需要创建和验证cookie
2. 没有多服务器共享session的问题，只需要登录逻辑、数字签名和密钥文件在服务器之间共享
3. 很多web开发平台(比如PHP的yii)或框架默认使用该方式

### 缺点
1. cookie的大小限制
2. 依然用了cookie，所以也有跨域问题
3. 不适合用在native app：不好管理cookie
4. 不适合用来做纯API服务的登录认证 

## token-base
### 过程
1. (类似cookie-based)用户登录 --> 服务端返回token到客户端
2. 用户请求(通过url参数或者http header主动带上token) --> 服务端验证token

### 优点
1. 适用于native app、SPA

### 缺点
1. 仅限于纯API的web应用，不适用于点击链接直接请求的情况
2. 可能会有跨域问题，不过很容易解决
3. token刷新的问题需要注意

### 相关
- [JWT标准(json-web-token)](https://jwt.io)