---
title: webhook-deploy
tags:
  - webhook
  - deploy
abbrlink: 62021
date: 2018-05-18 11:05:06
---
#### 上篇文章讲到的主要是前端的自动化部署，上传服务器和七牛cdn，本文将会介绍后端代码的部署，使用webhook和sh脚本操作
###### 一、在github或者coding.net的项目设置webhook接口，可以使用ip，最好添设置上token，避免其他请求导致问题
###### 二、webhook接口为自己开发，此处有个简单的demo，后面会用数据库做部署记录和回滚
```
router.post('/pushed', function (req, res, next) { // utils是自己封装的函数
  var reqBody = req.body;
  if (reqBody.token !== 'from-coding-push') {
    return utils.fail(res, '非coding操作')
  }

  var std = 'Now deploying!'
  child_process.execFile('/root/command-sh/test.clothesapi.sh', [], {
    env: {}
  }, function (err, stderr, stdout) {
    console.log(err, stderr, stdout)
  })
  utils.success(res, 'deployed', req.body)
})
```
###### 三、sh脚本要执行的操作，注意权限chmod 777 *.sh，此处可能会出现git command not found，使用which git找到bin，使用bin下的命令
```
cd /root/deploy-test
forever stop clothes.js
/usr/local/git/bin/git pull origin master
npm install
forever start clothes.js
```
