---
title: vuex在vue中的使用
date: 2018-01-17 16:46:52
tags:
  - vuex
---

###### 记性越来越差，前端的东西也越来越多，但用的也并不是很频繁。比如 vuex 在项目中，一般构建一次就不会在添加了，所以容易忘记。本文介绍在 vue 中使用 vuex 的写法，记一个笔记。

- 1.创建目录

```
vuex
  - index.js
  - state.js
  - mutations.js
  - actions.js
```

- 2.index.js。此处 Vue.use(Vuex)，不是在 main.js 里的 new Vue 传参（和 router 不同）。最后的方法是 new Vuex.Store()，不是 new Vuex()

```
import Vue from 'vue'
import Vuex from 'vuex'

import state from './state.js'
import mutations from './mutations.js'
import actions from './actions.js'

Vue.use(Vuex)

export default new Vuex.Store({
  state,
  mutations,
  actions
})
```

- 3.state.js

```
const state = {
  number: 1
}

export default state
```

- 4.mutations.js。此处的参数是 state 和传入的 value，如果多个值请使用对象，此处只能接收两个参数

```
const mutations = {
  test (state, number) {
    state.number = state.number + 1
  }
}

export default mutations
```

- 5.actions.js。此处的参数是包含 commit 和 state 的对象，后面可以传第二个参数

```
const actions = {
  test ({ commit, state }, value) {
    commit('test', 1)
  }
}

export default actions
```

- 6.项目入口配置 store。

- 7.以上基本就完成了代码的使用，介绍 mapState 和 mapActions 的使用

```
// mapState使用一
computed: mapState([
  'number'
])

// mapState使用二
computed: mapState({
  'number': state => state.number
})


// mapActions使用一
methods: mapActions([
  'test'
])

// mapActions使用二，使用dispatch，commit类似
this.$store.dispatch('test', value)
this.$store.commit('test', value)

// 如果computed里除了mapState之外还有其他计算值，使用结构，记住computed里需要的是对象
computed: {
  ...mapState([
    'number'
  ]),
  a () {
    return 1
  }
}
```
