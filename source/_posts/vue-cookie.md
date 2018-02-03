---
title: 通过vue-cookie在浏览器端使用cookie（博客开发三）
date: 2018-01-22 10:19:41
tags:
  - 客户端cookie
---
###### 博客系统开发如果使用服务端渲染，如jade、ejs、swig等，可以在服务端通过cookies模块操作（参考博客开发一），如果在客户端笔者使用了vue框架，此处使用了vue-cookie模块，在app.vue里获取cookie里的用户信息，然后将用户信息放到全局state里，此处用到了vuex。代码参考：
```
// main.js
import vueCookie from 'vue-cookie'

Vue.use(vueCookie)


// App.vue
created () {
  var userInfoString = this.$cookie.get('userInfo')
  var userInfoJSON
  if (userInfoString) {
    try {
      userInfoJSON = JSON.parse(userInfoString)
      this.$store.dispatch('setUserInfo', userInfoJSON)
    } catch (e) {
      console.log('从cookie解析userInfo出错，错误信息是：' + e)
    }
  }
}
```