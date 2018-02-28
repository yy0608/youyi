---
title: nodejs和redis对小程序登录状态的维护
date: 2018-02-27 18:00:02
tags:
  - 小程序登录状态
---
###### 一、先简单记录下经验技巧，mac上使用brew安装，可直接查看安装的信息，也能直接运行命令。windows上Chocolatey有一些小区别
```
brew install redis

redis-server // 通过brew安装的可以在任意位置运行，windows找到bin文件运行

brew info redis // 可以查看安装目录及其他信息
```
###### 二、nodejs服务端使用node_redis操作redis，注意使用的npm名称为redis
```
npm install redis

var redis = require('redis')

var client = redis.createClient(6379, 'localhost')

client.set('test', 'testVal')

client.expire('test', 7200)

client.get('test', function (err, value) { // 此处为异步
})
```
###### 三、其中会有下发到服务端的session，需要使用linux系统的随机数机制
```
const exec = require('child_process').exec;

exec('head -n 80 /dev/urandom | LC_ALL=C tr -dc A-Za-z0-9 | head -c 168', function(err,stdout,stderr){ // 注意LC_ALL=C，如果不加可能会乱码
  console.log(stdout)
})
```
