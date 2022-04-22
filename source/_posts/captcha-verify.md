---
title: 使用顶象进行滑动验证
tags:
  - 验证码校验
abbrlink: 19974
date: 2018-03-09 18:06:39
---
###### 一、客户端
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="https://cdn.dingxiang-inc.com/ctu-group/captcha-ui/index.js"></script>
</head>
<body>
<div id="code"></div>
</body>

<script>
var myCaptcha = _dx.Captcha(code, {
  appId: '',
  success: function (token) {
    console.log('token:', token)
  },
  fail: function (res) {
    console.log(res)
  }
})

setTimeout(function () {
  myCaptcha.reload()
}, 10000)
</script>
</html>
```
###### 二、nodejs服务端
```
var express = require('express')
var app = express()
var bodyParser = require('body-parser')

var CaptchaSDK = require('dx-captcha-sdk')

app.use(express.static('./static'))
app.use(bodyParser.json())

var sdk = new CaptchaSDK('您的appId', '您的appSecret')

app.post('/verify_captcha', function (req, res, next) {
  var reqBody = req.body
  console.log(reqBody)
  var token = reqBody && reqBody.token
  if (!token) {
    res.json({
      success: false,
      msg: 'no token'
    })
  } else {
    sdk.verifyToken(token).then((response) => {
      console.log(response)
      res.json({
        success: true,
        msg: 'verify ok',
        res: response
      })
    }).catch(err => {
      res.json({
        success: false,
        msg: 'verify fail',
        err_msg: err
      })
    })
  }
})

app.listen(3000, function () {
  console.log('listen on port ...')
})
```
###### 三、更多实践可参考服装系统，[官方文档](https://cdn.dingxiang-inc.com/public-service/docs/captcha/dev/backend/sdk-nodejs.html)
