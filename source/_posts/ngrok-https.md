---
title: sunny-ngrok内网穿透实现https
tags:
  - ngrok
abbrlink: 40472
date: 2018-07-14 13:38:01
---

### 一、ngrok介绍
- 1.提供内网穿透，本地开发调试第三方接口，必须使用域名或者https时有用，也能使外网访问内网服务，如测试环境网站，gitlab等
- 2.国外是ngrok服务，国内使用sunny-ngrok，虽然都有付费版，但国外是$5每月，国内是10元每月，付费版固定域名，注意备案域名

### 二、如何使用
- 1.购买的是广州的frp服务器，可以使用https
- 2.后台配置好自定义域名，在官网教程里有介绍，注意本机监听https的443端口
- 3.本机启的nodejs服务，需要ssl证书，可以在腾讯云申请

### 三、本地代码
```
'use strict'

const express = require('express')
const fs = require('fs')
const http = require('http')
const https = require('https')
const privateKey  = fs.readFileSync('ssl/2_api.key', 'utf8');
const certificate = fs.readFileSync('ssl/1_api.crt', 'utf8');
const app = express()

var credentials = { key: privateKey, cert: certificate }

var httpServer = http.createServer(app)
var httpsServer = https.createServer(credentials, app)

httpServer.listen(3000)
httpsServer.listen(443)

app.get('/', function (req, res) {
  res.send('ok')
})
```
