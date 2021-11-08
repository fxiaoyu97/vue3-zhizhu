vuex getters 文档 ：https://vuex.vuejs.org/zh/guide/getters.html

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

```typescript
getters: {
  biggerColumnsLen(state) {
    return state.columns.filter(c => c.id > 2).length
  }
}
// 定义完毕，就可以在应用中使用这个 getter 了
// Getter 会暴露为 store.getters 对象，你可以以属性的形式访问这些值：
const biggerColumnsLen =computed(()=>store.getters.biggerColumnsLen)
```

```typescript
getColumnById: (state) => (id: number) => {
  return state.columns.find(c => c.id === id)
},
getPostsByCid: (state) => (id: number) => {
  return state.posts.filter(post => post.columnId === id)
}
// 定义完毕以后就可以在应用中使用 getter 快速的拿到这两个值了
const column = computed(() => store.getters.getColumnById(currentId))
const list = computed(() => store.getters.getPostsByCid(currentId))
```

