---
title: qiankun微前端架构注意点
tags:
  - 微前端
abbrlink: 19551
date: 2021-05-31 20:35:06
---

##### 一、基本使用

1. 主应用 qiankun 注册

2. 子应用暴露 bootstrap, mount, unmount 三个方法，并在打包结果配置为 libary: umd

##### 二、子应用切换时缓存页面

##### 三、子应用为单页面应用并使用了前端 router，此时缓存切换前端 router

##### 四、主子应用通信

1. 观察者模式，onGlobalStateChange
2. vuex(项目 Vue 前提，耦合严重的拆分应用，用 props 传递 store，Vue.observable 数据响应)
