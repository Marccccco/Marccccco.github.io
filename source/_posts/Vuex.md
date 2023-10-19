---
title: Vuex
---



[toc]

## Vuex

### store模式-[状态管理](https://cn.vuejs.org/guide/scaling-up/state-management.html)

如果是一个不怎么复杂的页面可以使用这个模式.

在选项式API中,响应式数据是用`data()`选项声明的.在内部,`data()`的返回值对象会通过`reactive()`这个公开的API函数转为响应式.

```js
// store.js
import { reactive } from 'vue'

export const store = reactive({
  count: 0,
  increment() {
    this.count++
  }
})
```

```vue
<!-- ComponentA.vue -->
<script>
import { store } from './store.js'

export default {
  data() {
    return {
      store
    }
  }
}
</script>

<template>From A: {{ store.count }}</template>
```

```vue
<!-- ComponentB.vue -->
<script>
import { store } from './store.js'

export default {
  data() {
    return {
      store
    }
  }
}
</script>

<template>
  <button @click="store.count++">
    From B: {{ store.count }}
  </button>
</template>
```

现在每当`store`对象被更改时,`<ComponentA>`跟`<ComponentB>`都会自动更新,这也意味着任意一个导入了`store`的组件都可以随意更改它的状态.

### [Vuex](https://vuex.vuejs.org/zh/)

#### State

