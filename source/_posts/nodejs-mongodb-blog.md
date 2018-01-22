---
title: nodejs和mongodb搭建博客（博客开发一）
date: 2018-01-11 17:26:23
tags:
  - nodejs搭建博客
---
##### 本文主要介绍服务端的相关开发，客户端使用vue开发web、react-native开发app、小程序等，本文不做客户端的开发介绍
- 1.目录结构
```
app.js
- models
  - User.js
- routers
  - main.js
  - api.js
  - admin.js
- schemas
  - users.js
```
- 2.app.js
```
var express = require('express')
var bodyParser = require('body-parser')
var mongoose = require('mongoose')
var cookies = require('cookies')

// 使用全局的Promise，需要高版本node支持Promise，也可使用bluebird
mongoose.Promise = global.Promise

var app = express()

// post请求数据的处理，在req.body
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: false }))

// 设置和获取cookies
app.use(function (req, res, next) {
  req.cookies = new cookies(req, res)
  next()
})

// 解决跨域
app.all('*', function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With");
  res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
  res.header("X-Powered-By", ' 3.2.1')
  // if (req.method == "OPTIONS") res.sendStatus(200); /*让options请求快速返回*/
  else next();
});

// 设置路由组
app.use('/', require('./routers/main.js'))
app.use('/api', require('./routers/api.js'))
app.use('/admin', require('./routers/admin.js'))

// 连接数据库
mongoose.connect('mongodb://localhost:27017/laogao', {
  useMongoClient: true
}, function(err) {
  if (err) {
    console.log('数据库连接失败')
  } else {
    console.log('数据库连接成功')
  }
}).catch(err => {
  console.log(err.message)
})

// 监听端口，开启服务
app.listen(3000, function(err) {
  if (err) {
    console.log(err)
  } else {
    console.log('listen on port 3000')
  }
})
```
- 3.admin.js，路由组内部写法
```
var express = require('express')

var router = express.Router()

router.get('/user', function (req, res, next) {
  res.send('admin user')
})

module.exports = router
```
- 4.schema（数据表）的定义，users.js
```
var mongoose = require('mongoose')
var Schema = mongoose.Schema

module.exports = new Schema({
  username: String,
  password: String
})
```
- 5.model的写法，User.js
```
var mongoose = require('mongoose')
var usersSchema = require('../schemas/users.js')

module.exports = mongoose.model('User', usersSchema)
```
- 6.api.js登录注册接口对数据库的相关操作，[mongoose api地址](http://mongoosejs.com/docs/4.x/docs/api.html)
```
var express = require('express')
var router = express.Router()
var User = require('../models/User.js')
var mongoose = require('mongoose')
mongoose.Promise = global.Promise

var responseData

router.use(function (req, res, next) {
  responseData = {
    code: 0,
    msg: ''
  }
  next()
})

router.post('/user/register', function (req, res, next) {
  var resBody = req.body
  var username = resBody.username
  var password = resBody.password
  if (!resBody) {
    responseData.code = 9
    responseData.msg = '请求数据处理出错'
    res.json(responseData)
    return
  }
  if (username && !username.trim()) {
    responseData.code = 1
    responseData.msg = '用户名不能为空'
    res.json(responseData)
    return
  }
  if (password && !password.trim()) {
    responseData.code = 2
    responseData.msg = '密码不能为空'
    res.json(responseData)
  }
  User.findOne({
    username: username
  }).then(data => {
    if (data) {
      responseData.code = 3
      responseData.msg = '已有相同用户名'
      return Promise.resolve(responseData)
    } // 没有存在用户名
    var user = new User({
      username: username,
      password: password
    })
    return user.save()
  }).then(data => {
    if (data.code === 3) {
      res.json(data)
    } else {
      responseData.msg = '注册成功'
      res.json(responseData)
    }
    return
  }).catch(err => {
    responseData.code = 9
    responseData.msg = '数据库查找出错'
    res.json(responseData)
    return
  })
})

router.post('/user/login', function (req, res, next) {
  var resBody = req.body
  var username = resBody.username
  var password = resBody.password
  if (!resBody || !username || !password) {
    responseData.code = 9
    responseData.msg = '请求数据出错'
    res.json(responseData)
    return
  }
  if (username && !username.trim()) {
    responseData.code = 1
    responseData.msg = '用户名不能为空'
    res.json(responseData)
    return
  }
  if (password && !password.trim()) {
    responseData.code = 2
    responseData.msg = '密码不能为空'
    res.json(responseData)
  }
  User.findOne({
    username: username
  }).then(data => {
    if (!data || data.password !== password) {
      responseData.code = 4
      responseData.msg = '用户名或密码错误'
      res.json(responseData)
      return
    } else {
      responseData.msg = '登录成功'
      responseData.user_info = {
        _id: data._id,
        username: data.username
      }
      console.log(req.cookies)
      req.cookies.set('userInfo', JSON.stringify(responseData.user_info), { httpOnly: false }) // 此处默认为true，浏览器客户端无法获取。获取的时候服务端使用req.cookies.get('userInfo')，客户端使用了vue-cookie（参考文章博客开发三）
      res.json(responseData)
      return
    }
  }).catch(err => {
    responseData.code = 9
    responseData.msg = '数据库查找出错'
    responseData.error = err
    res.json(responseData)
    return
  })
})

module.exports = router

```