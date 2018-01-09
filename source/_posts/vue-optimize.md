---
title: vue多页面优化之seo和路由懒加载
date: 2018-01-03 16:01:41
tags:
  - vue
  - 路由懒加载
  - vue-router lazy-load
  - code spliting
  - webpack代码分割
  - prerender-spa-plugin
---
###### 本文介绍vue使用上的一些优化，seo优化主要两个方面，一个是代码预渲染，另一个是后端渲染。然后是vue-router路由的懒加载（lazy-load），其中会配合webpack2的代码分割（code spliting），更多内容请自行查找，每一个点都可以深入。
##### 一、seo优化之代码预渲染，使用prerender-spa-plugin插件
- 1.安装prerender-spa-plugin
```
npm install --save-dev prerender-spa-plugin
```
- 2.在 build/webpack.prod.conf.js 中引用
```
var PrerenderSpaPlugin = require('prerender-spa-plugin')
```
- 3.在 build/webpack.prod.conf.js 中配置 plugins 列表
```
new PrerenderSpaPlugin(
  // 要编译的目录
  path.join(__dirname, '../dist'),
  // 你要预呈现的列表
  [ '/', '/aaa', '/bbb' ]
)
```
- 4.打包比较
```
npm run build
```
##### 二、seo优化之服务端渲染ssr，使用vue-server-renderer
##### 三、vue-router路由懒加载lazy-load，测试和预渲染冲突，懒加载是异步加载的一段js代码
- 1.基本写法
```
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)

// webpack按需加载组件(写法一)
const A = r => require.ensure([], () => r(require('@/components/A')))

// 路由对应的组件定义成异步组件(写法二)
const A = resolve => {
  // 空数组用来指定该路由组件需要加载的依赖
  require.ensure([], () => {
    resolve(require('@/components/A'))
  })
}
...
```
- 2.按组分块。把多个组件打包到同个异步chunk中，提供require.ensure第三个参数名称
```
const A = r => require.ensure([], require('@/components/A'), 'big-chunks')
```

# &nbsp;
> *参考文章，[Vue.js路由懒加载](https://www.jianshu.com/p/abb02075b56b)*，[webpack配置优化](https://sebastianblade.com/using-webpack-to-achieve-long-term-cache/)