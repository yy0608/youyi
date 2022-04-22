---
title: 生成小程序二维码并上传七牛
tags:
  - 小程序二维码
abbrlink: 42810
date: 2018-05-19 16:24:31
---
###### 一、生成access_token，保存在redis，先取redis，没有再请求微信接口
```
function getAccessToken (cb) { // 获取access_token的异步方法
  global.redisClient.get('accessTokenRedisKey', function (err, v) {
    if (err || v) {
      return cb && cb(err, v ? '从redis获取access_token成功' : '从redis获取access_token失败', v)
    }
    axios({
      url: 'https://api.weixin.qq.com/cgi-bin/token',
      method: 'get',
      params: {
        grant_type: 'client_credential',
        appid: config.mpAppConfig.appId,
        secret: config.mpAppConfig.appSecret
      }
    })
      .then(response => {
        global.redisClient.set('accessTokenRedisKey', response.data.access_token)
        global.redisClient.expire('accessTokenRedisKey', response.data.expires_in)
        cb && cb(null, '通过微信接口获取access_token成功',  response.data.access_token)
      })
      .catch(err => {
        cb && cb(err, '通过微信接口获取access_token失败', '')
      })
  })
}
```
###### 二、生成小程序二维码，注意responseType: 'stream'
```
axios({
  url: 'https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=' + accessToken,
  method: 'post',
  responseType: 'stream', // ****非常重要！！！
  data: {
    scene: '?a=hello',
    is_hyaline: !!reqBody.png,
    // page: 'pages/topics/index/index'
    page: 'pages/about/about' // 饭千金，该字段不填默认去首页
  }
})
  .then((response) => {
    utils.uploadImgStreamToQiniu({
      scope: reqBody.scope,
      key: reqBody.key,
      readableStream: response.data,
      cb: function (respErr, respBody, respInfo) {
        if (respErr) {
          return utils.fail(res, '上传生成的小程序二维码到七牛失败', err)
        }

        if (respInfo.statusCode === 200) {
          utils.success(res, '生成二维码成功', respBody.key) // resBody.hash
        } else {
          utils.fail(res, '上传生成的小程序二维码到七牛失败', undefined, { resp_info: respInfo, resp_body: respBody })
        }
      }
    })
  })
  .catch(err => {
    console.log(err)
    utils.fail(res, '获取二维码失败', err)
  })
```
###### 三、保存到七牛云，即uploadImgStreamToQiniu方法，里面generateResourceConfig方法，使用nodejs流文件保存
```
function uploadImgStreamToQiniu (params) {
  var resourceConfig = this.generateResourceConfig()

  var scope = params.scope || config.qiniuConfig.default_bucket;
  var putPolicy = new qiniu.rs.PutPolicy({ scope: scope });
  var uploadToken = putPolicy.uploadToken(this.qiniuObj.mac);

  var formUploader = new qiniu.form_up.FormUploader(resourceConfig);
  var putExtra = new qiniu.form_up.PutExtra();

  formUploader.putStream(uploadToken, params.key, params.readableStream, putExtra, function(respErr, respBody, respInfo) {
    params.cb && params.cb(respErr, respBody, respInfo)
  })
}

function generateResourceConfig () { // 七牛生成config
  this.qiniuObj.accessKey = this.qiniuObj.accessKey || config.qiniuConfig.access_key;
  this.qiniuObj.secretKey = this.qiniuObj.secretKey || config.qiniuConfig.secret_key;

  this.qiniuObj.mac = this.qiniuObj.mac || new qiniu.auth.digest.Mac(this.qiniuObj.accessKey, this.qiniuObj.secretKey);
  return new qiniu.conf.Config();
}
```
