---
title: 博客开发中的安全在本项目的实践（博客开发四）
date: 2018-01-22 14:59:06
tags:
  - cookie安全
---
###### 一、cookie安全。本文主要介绍在服务端通过cookies模块进行cookie签名，从而增加安全性。因为本项目是前后端分离，需要在浏览器客户端拿到cookie进行判断，所以在服务端设置了httpOnly为false，这样客户端就能修改cookie提交服务器，安全性很低。一些服务端中间件都有签名操作，如express的cookies方法和cookies模块，操作如下（也有session_id的中间件，比这个可能会更好一些）
```
// 实例化的时候设置签名的keys，注意后面是数组（坑了很久）
req.cookies = new cookies(req, res, {
  keys: [ 'youyi' ]
})


// 设置cookies的时候，设置signed属性为true
req.cookies.set('_id', data._id, {
  httpOnly: false,
  signed: true
})


// 获取cookies的时候，设置signed属性为true
req.cookies.get('_id', {
  signed: true
})
```
###### 二、路由安全。目前博客系统是单页面的，包括前台和后台（后面会分成多页面，至少前后台分开），通过路由进入后台系统，现在在router/index.js里的beforeEach里控制，如果是后台路由，先看全局state的userInfo里isAdmin，如果没有就调接口查询cookie里_id的用户信息
```
router.beforeEach((to, from, next) => {
  let store = router.app.$options.components.App.store
  let state = store.state
  let userInfo = state.userInfo
  if (to.path !== from.path) { // 如果路由变化，还原到默认页码数设置
    store.commit('resetPage')
  }
  if (!Object.keys(to.query).length && to.meta.hasPage) {
    return next({
      path: to.path,
      query: {
        page: state.page,
        limit: state.limit
      }
    })
  }
  if (!/admin/.test(to.path)) {
    return next()
  }
  if (userInfo) {
    if (userInfo.isAdmin) return next()
    return next('/')
  } else {
    store.dispatch('setUserInfoByCookie')
      .then(res => {
        if (res.isAdmin) return next()
        return next('/')
      })
      .catch(() => {
        return next('/')
      })
  }
})
```
###### 三、接口的安全。所有后台接口调用之前会通过cookie里_id查询用户信息，如果不存在，则不允许调用
```
router.use(function (req, res, next) { // 有权限才能拿到后台数据
  var _id = req.cookies.get('_id')
  responseData = {
    code: 0,
    msg: ''
  }
  if (!_id) {
    responseData.code = 1
    responseData.msg = '_id不存在'
    res.json(responseData)
    return
  }
  try {
    _id = JSON.parse(_id)
  } catch (e) {
    _id = _id
  }
  User.findOne({
    _id
  }).then(res => {
    if (!res) {
      responseData.code = 2
      responseData.msg = '用户不存在'
      res.json(responseData)
      return
    }
    if (!res.isAdmin) {
      responseData.code = 3
      responseData.msg = '用户无权限'
      res.json(responseData)
      return
    }
    next()
  }).catch(err => {
    responseData.code = 9
    responseData.msg = '查询数据库出错'
    responseData.message = err
    res.json(responseData)
    return
  })
})
```
