---
title: 通过CORS解决跨域cookies
date: 2018-01-20 15:44:47
tags:
  - 跨域cookies
---
###### 开发博客项目时，服务端和web客户端使用不同服务器，跨域设置cookie出现了问题。cookie是不能直接跨域设置的，这是浏览器的同源策略导致的。这里使用CORS跨域解决
- 1.服务器设置Access-Control-Allow-Origin为指定域，不能设置为*
```
res.header("Access-Control-Allow-Origin", "http://localhost:8080")
```
- 2.服务器设置Access-Control-Allow-Credentials为true
```
res.header("Access-Control-Allow-Credentials", true)
```
- 3.客户端请求设置withCredentials为true表示请求携带凭证信息
```
axios({
  url: origin + '/api/user/login',
  method: 'post',
  data: { ...this.data },
  withCredentials: true
})
```