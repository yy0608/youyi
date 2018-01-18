---
title: crypto模块介绍
date: 2018-01-18 11:01:20
tags:
  - crypto
  - hash
  - md5
---
###### crypto现在是node自带模块，不需要单独安装了。使用方式是
```
var crypto = require('crypto')
var hash = crypto.createHash('md5')

hash.update('Hello, ')
hash.update('world!')

var res = hash.digest('hex')
```
####### 介绍几个方法
- 1. createHash, 接收参数md5, sha1, sha256, sha512, ripemd160
- 2. update, 传入字符串，其实是字符串的相加，例子中结果同hash.update('Hello, world!')
- 3. digest, 默认二进制，hex为十六进制，会将hash对象清空