---
title: vue自动化上传七牛和云主机
date: 2018-05-10 11:57:28
tags:
  - vue部署
---
### 一、上传七牛，使用better-qiniu-webpack-plugin，测试过几个中最好用的
```
const QiniuPlugin = require('better-qiniu-webpack-plugin')

// 打包文件上传七牛，不重复上传，仅保留上次文件。在webpack.prod.conf.js中使用
new QiniuPlugin({
  accessKey: '', // required
  secretKey: '', // required
  bucket: env.type === '"test"' ? 'test' : 'master', // required
  bucketDomain: env.type === '"test"' ? '//*.*.com' : '//*.*.com', // required
  matchFiles: ['!*.html', '!*.map'],
  uploadPath: '/clothes_view',
  batch: 10
})
```
### 二、上传到云主机，使用scp2，在build.js中使用
```
const client = require('scp2')

if (process.env.NODE_ENV === 'production') {
  client.scp('dist/views/', { // 打包后上传服务器
    host: '111.88.88.88',
    username: 'username',
    password: 'password',
    path: process.env.type === '"test"' ? '/root/*/*' : '/root/*/*'
  }, function (err) {
    if (err) {
      return console.log(err)
    }

    console.log(chalk.green('Finished, 上传成功!'))
  })
}
```
