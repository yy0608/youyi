---
title: nodejs中管理使用express-session和redis进行会话管理
tags:
  - session
abbrlink: 31231
date: 2018-03-08 16:01:07
---
### 这个模块已经做了很多包含cookie和redis的处理，使用起来特别简单，但需要理解其中的过程。secret这个
###### 一、用到的模块express-session, connect-redis[, node_redis]，其中node_redis是可选
```
npm install express-session
npm install connect-redis
// npm install redis
```
###### 二、基本配置和使用
```
var session = require('express-session')

app.use(session({
  secret: 'session_test',
  store: new RedisStore({
    // client: redisClient,
    host: 'localhost',
    port: 6379
  }),
  cookie: {
    maxAge: 30 * 1000 // 秒
  },
  resave: true, // :(是否允许)当客户端并行发送多个请求时，其中一个请求在另一个请求结束时对session进行修改覆盖并保存。如果设置false，可以使用.touch方法，避免在活动中的session失效。
  saveUninitialized: false // 初始化session时是否保存到存储
}))

app.post('/login', function (req, res, next) { // 登录接口存session
  req.session.signed = '1' // 只有使用了req.session.signed时才会在redis保存数据，有效期和cookie里的maxAge一样
  res.json({
    success: true
  })
})

app.get('/all', function (req, res, next) { // 其他接口取session
  console.log(req.session.signed)
  res.json({
    success: true,
    logined: req.session.signed
  })
})
```
###### 三、使用node_redis的场景。当需要自行使用redis时用，配置有点不同
```
var session = require('express-session')
var redis = require('redis')

// 连接redis
global.redisClient = redis.createClient(6379, 'localhost')

app.use(session({
  secret: 'session_test',
  store: new RedisStore({
    client: redisClient
    // host: 'localhost',
    // port: 6379
  }),
  cookie: {
    maxAge: 30 * 1000 // 30秒
  },
  resave: true, // :(是否允许)当客户端并行发送多个请求时，其中一个请求在另一个请求结束时对session进行修改覆盖并保存。如果设置false，可以使用.touch方法，避免在活动中的session失效。
  saveUninitialized: false // 初始化session时是否保存到存储
}))

// 附node_redis常用方法
redisClient.set('test', 'aaa')
redisClient.get('test', function (err, v) {})
redisClient.expire('test', 30) // 30秒
```
###### 四、使用随机数生成secret，（session_id会不会重复？几率很低，小程序上自行实现session是通过系统随机数机制生成），在windows上会出错，mac可以使用，也可部署服务器
```
const exec = require('child_process').exec;

exec('head -n 80 /dev/urandom | LC_ALL=C tr -dc A-Za-z0-9 | head -c 168', function(err,stdout,stderr){ // 注意LC_ALL=C，如果不加可能会乱码
  console.log(stdout)
})
```
###### 五、在小程序和app等没有cookie机制的数据需要自行实现。
