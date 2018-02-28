---
title: 七牛FormData上传和小程序上传
date: 2018-02-27 17:42:37
tags:
  - 七牛上传
---
###### 一、nodejs服务端生成token凭证
- 1.引用qiniu.js
```
npm install qiniu.js

var qiniu = require('qiniu')
```
- 2.生成token
```
var accessKey = '[...]';
var secretKey = '[...]';

var mac = new qiniu.auth.digest.Mac(accessKey, secretKey);
var putPolicy = new qiniu.rs.PutPolicy({ scope: 'wusuowei' });
var uploadToken = putPolicy.uploadToken(mac);
```
- 3.token的有效期默认为1小时（3600），本地缓存起来，不然还要请求服务器，也可以修改为2小时
```
var putPolicy = new qiniu.rs.PutPolicy({ scope: 'wusuowei', expires: 7200 });
```
###### 二、浏览器以FormData形式上传，此处使用axios和vue
```
var file = e.currentTarget.files[0]
var formData = new FormData()
formData.append('file', file)
formData.append('key', 'test/1.jpg') // 此处需要自己改名
formData.append('token', 'QimXTd2UT59EgNfZuEJ2_27gEwHRCSmw5sW_sO9u:VmCYu_KNngPLEGbSe3LPhKrju6g=:eyJzY29wZSI6Ind1c3Vvd2VpIiwiZGVhZGxpbmUiOjE1MTk3MjU3OTR9') // 通过服务端请求或者客户端缓存获得
axios({
  method: 'post',
  url: 'https://upload-z2.qiniup.com', // 华南地址
  headers: {
    'Content-Type': 'multipart/form-data'
  },
  data: formData
})
  .then(res => {
    console.log(res.data)
  })
  .catch(err => {
    console.log(err)
  })
```
###### 三、小程序上传七牛，之前有测试开发，没有记录，回去补上。可参考[qiniu-wxapp-sdk](https://github.com/gpake/qiniu-wxapp-sdk)
