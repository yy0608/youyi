---
title: script标签text/template的使用
date: 2018-06-28 15:19:08
tags:
  - template
---
## 点餐项目中前后端没分离，后端使用thinkPHP，前端实在不想使用jQuery，仍然使用了vue库，项目不太适合webpack和es6的文件模块引用，可以在页面内使用script的text/template模板，后面可以借助thinkPHP加入文件变量引入

## 组件的使用，使用代码片段
```
var app = new Vue({
  el: '#app',
  components: {
    'comp-a': {
      props: [ 'name' ],
      template: '<div>{{name}}</div>'
    }
  }
})
```
## 使用script的template
```
<script type="text/template" id="tpl">
  <div>{{name}}</div>
</script>
```
## template注册组件的时候使用一
```
var compB = Vue.extend({
  props: [ 'name' ],
  template: '#tpl'
})

var app = new Vue({
  el: '#app',
  components: {
    'comp-b': compB
  }
})
```
## template注册组件的时候使用二
```
var app = new Vue({
  el: '#app',
  components: {
    'comp-b': {
      props: [ 'name' ],
      template: tpl.innerHTML
    }
  }
})
```
## template注册组件的时候使用三，异步引入组件文件
```
var app = new Vue({
  el: '#app',
  components: {
    'comp-b': function (resolve) {
      axios({
        url: '/Public/tpls/tpl1.html'
      })
        .then(function (res) {
          resolve({
            props: [ 'name' ],
            template: res.data
          })
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
})
```
## 也可借助thinkPHP文件引用
