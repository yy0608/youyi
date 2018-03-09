---
title: 第三方短信服务（腾讯云）的接入
date: 2018-02-28 16:12:42
tags:
  - 短信验证码
---
#### 第三方短信发送平台，选择了腾讯云，有100条免费测试。鉴于之前对腾讯其他产品的使用，速度是真的很快。其他的有阿里云，云片等。建议是选择大厂的服务，价格是贵一点，但不至于出问题，比起阿里，腾讯这些年是真的在认真做生态，阿里靠的是智慧。
###### 一、服务端使用腾讯云的sdk
```
npm install qcloudsms_js

var QcloudSms = require("qcloudsms_js")
```
###### 二、开发准备
- 1.申请 SDK AppID 以及 App Key，[链接](https://console.cloud.tencent.com/sms/smsinfo/1400070556/0), SDK AppID 是以 14xxxxx 开头的数字
- 2.申请签名。国内文本短信->短信内容配置->短信签名，这个签名会显示在短信内容的最前面，如【饭千金】
- 3.申请模板。国内文本短信->短信内容配置->短信正文。如：{1}为您的登录验证码，请于{2}分钟内填写。如非本人操作，请忽略本短信。{3}
###### 三、逻辑部分
```
var express = require('express')
var app = express()
var QcloudSms = require("qcloudsms_js")
var bodyParser = require('body-parser')

var config = {
  smsSign: '饭千金', // 签名
  appid: 14xxxxxx,
  appkey: '32abcdefghijk',
  templateId: 90000, // 模板ID
  smsType: 0 // Enum{0: 普通短信, 1: 营销短信}
}

var ssender = undefined

app.use(bodyParser.json())

app.get('/', function (req, res, next) {
  res.send('ok')
})

app.post('/send_message', function (req, res, next) {
  var reqBody = req.body
  var number = reqBody.number
  if (!number) {
    res.json({
      success: false,
      msg: '手机号码填写错误'
    })
  }

  var qcloudsms = QcloudSms(config.appid, config.appkey)
  ssender = ssender || qcloudsms.SmsSingleSender() // 单发短信
  // ssender = ssender || qcloudsms.SmsMultiSender() // 群发短线
  ssender.send(config.smsType, 86, number, "8888为您的登录验证码，请于 2 分钟内填写。如非本人操作，请忽略本短信。--来自高叔叔", "", "", function (err, response, resData) {
    if (err) {
      res.json({
        success: false,
        err_msg: err
      })
    } else {
      if (resData.result) { // 发送成功或失败都会走这里，成功的result为0
        res.json({
          success: false,
          err_msg: resData
        })
      } else {
        res.json({
          success: true,
          msg: resData
        })
      }
    }
  });
})

app.listen(3000, function () {
  console.log('listen on port ...')
})
```
