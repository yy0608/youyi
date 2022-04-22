---
title: node和vue博客开发中的技巧经验小结（博客开发五）
tags:
  - node博客开发技巧
abbrlink: 50372
date: 2018-02-03 14:38:46
---
###### 一、在router/index.js里获取全局的store和state，在页码组件相关路由跳转修改路由的query，便于刷新和复制链接
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
###### 二、vue的watch方法的两种写法，watch对象和this.$watch，前者不能或者很难获取到this组件实例，所以需要this组件实例的时候使用后者。在添加分类和分类列表两个路由页面有监听了$route
```
created () {
  this.checkPathHasCategory(this.$route.path)
  this.$watch('$route', function (newVal, oldVal) {
    this.checkPathHasCategory(newVal.path)
  })
},
methods: {
  checkPathHasCategory (path) {
    if (/category/.test(path)) {
      this.routerData[1].active = true
    } else {
      this.routerData[1].active = false
    }
  }
}
```
###### 三、element-ui里pagination组件的封装，全局config.js有limit和page的配置，然后将这两个值放到全局state里，组件外传入改变页面之后的方法，包括修改state里的总条数。组件内会修改路由里query的limit和page
```
// Pagination.vue页面
<template>
<el-pagination
  layout="total, sizes, prev, pager, next, jumper"
  @current-change="pageChange"
  @size-change="sizeChange"
  :page-sizes="[10, 15, 20, 30]"
  :current-page="page"
  :page-size="limit"
  :total="count">
</el-pagination>
</template>

<script>
import { mapState } from 'vuex'

export default {
  computed: mapState([
    'page',
    'limit'
  ]),
  created () {
    let query = this.$route.query
    if (Object.keys(query).sort().toString() === ['page', 'limit'].sort().toString()) {
      let page = parseInt(query.page)
      let limit = parseInt(query.limit)
      if (!isNaN(page) && !isNaN(limit)) {
        this.$store.commit('changePage', page)
        this.$store.commit('changeLimit', limit)
      }
    }
    this.change()
  },
  props: [ 'count', 'change' ],
  methods: {
    pageChange (val) {
      this.$store.commit('changePage', val)
      this.$router.push({
        path: this.$route.path,
        query: { page: this.page, limit: this.limit }
      })
      this.change()
    },
    sizeChange (val) {
      this.$store.commit('changeLimit', val)
      this.$router.push({
        path: this.$route.path,
        query: { page: this.page, limit: this.limit }
      })
      this.change()
    }
  }
}
</script>


// 需要使用Pagination组件的Category分类列表页面
<tempalte>
<pagination :change="pageChange" :count="categoryCount"></pagination>
</tempalte>

<script>
import { mapState } from 'vuex'
import Pagination from '../Pagination.vue'

export default {
  computed: mapState([
    'page',
    'limit',
    'categoryList',
    'categoryCount'
  ]),
  components: {
    Pagination
  },
  methods: {
    getCategoryList () {
      this.$store.dispatch('getCategoryList')
        .then(() => {
        })
        .catch(err => {
          console.log(err)
          this.$message({
            message: '获取分类列表出错',
            type: 'error'
          })
        })
    },
    pageChange (page) {
      this.getCategoryList()
    }
  }
}
</script>
```
###### 四、列表删除的操作，因为是在同组件（同页面）操作，所以没有去调接口获取列表，列表是在组件creatd钩子里获取的。删除传入index和_id，成功之后通过index修改全局state里的列表
###### 五、列表数据的添加和编辑和删除不同，使用了路由跳转新页面，返回会在created钩子里重新获取列表数据。添加和编辑是通过路由的query里是否有_id判断，编辑的话先获取详情，此处就需要获取详情的接口。之前有想过通过index去全局state里列表拿到某条列表数据。这种方法比较麻烦，需要很多flag，而且刷新页面全局state数据会变
###### 六、修改textarea中的回车换行，在vue的v-html中正常显示
```
changeEnter (str) {
  var resStr = str.replace(/<\s*/g, '&lt;')
  resStr = resStr.replace(/&lt;\s*/g, '&lt; ')
  resStr = resStr.replace(/\n/g, '<br>')
  return resStr
}
```
###### 七、跨域使用cookie的时候，如果仅仅是端口不同，前后端都能拿到cookie，但如果子域名不同，后端设置了cookie，前端不能通过document.cookie拿到
