---
title: 给mongodb设置密码
date: 2018-02-10 16:01:09
tags:
  - mongodb密码设置
---
###### 一、使用mongo命令，选择数据库，创建用户和密码
```
use laogao

db.createUser(
  {
    user: "user",
    pwd: "password",
    roles: [ { role: "readWrite", db: "laogao" } ]
  }
)
```
###### 二、关闭数据库，再次打开添加--auth参数，默认不开启权限
```
mongod --auth
```
###### 三、代码中连接数据库
```
// mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]

mongoose.connect('mongodb://youyi:yy0608@localhost:27017/laogao', {
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
```
