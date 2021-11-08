## 一、loading组件开启关闭方法

1、在`store.ts`中的`GlobalDataProps`中添加`loading`字段

```typescript
export interface GlobalDataProps {
  loading: boolean
  columns: ColumnProps[]
  posts: PostProps[]
  user: UserProps
}
```

2、添加初始值

```typescript
const store = createStore<GlobalDataProps>({
  state: {
    loading: false,
    columns: [],
    posts: [],
    user: { isLogin: false }
  }
  ……
})
```

3、添加`mutation`方法，修改loading值

```typescript
setLoading(state,status){
    state.loading = status
}
```

4、添加加载效果

```typescript
const getAndCommit = async (url: string, mutationName: string, commit: Commit) => {
  // 发送请求前启动加载效果
  commit('setLoading',true)
  const { data } = await axios.get(url)
  commit(mutationName, data)
  // 发送请求后关闭加载效果
  commit('setLoading',true)
}
```

5、此方法只能用于get请求，当需要应用于全局请求时，在拦截器中处理

```javascript
axios.interceptors.request.use(config =>{
  store.commit('setLoading',true)
  return config
})
axios.interceptors.response.use(config=>{
  setTimeout(() => {
    store.commit('setLoading', false)
  }, 1000)
  return config
})
```

## 二、loading组件效果

+ 长宽 100% 的半透明遮罩层 
+ 可以自定义颜色
+ 一个动态的旋转的圆形图标，可以添加帮助文字

**组件基础代码**

```vue
<template>
  <div class="d-flex justify-content-center align-items-center h-100 w-100 loading-container" :style="{ backgroundColor: background || '' }">
    <div class="loading-content">
      <div class="spinner-border text-primary" role="status">
        <span class="sr-only">{{ text || 'loading' }}</span>
      </div>
      <p v-if="text" class="text-primary small">{{ text }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    text: {
      type: String
    },
    background: {
      type: String
    }
  },
  setup() {}
})
</script>
<style>
.loading-container {
  background: rgba(255, 255, 255, 0.5);
  z-index: 100;
  position: fixed;
  top: 0;
  left: 0;
}
.loading-container {
  text-align: center;
}
</style>
```



2、在`App.vue`中引入组件使用

```vue
 <loader text="拼命加载中" background="rgba(0,0,0,0.8)"/>
```

3、在指定区域渲染，使用`teleport`包裹组件，指向 id 为 back 的位置

```html
<teleport to="#back">
    ……
</teleport>
```

4、创建 id 为 back 的组件，在`Loader.vue`组件的 setup 方法中实现

```javascript
setup() {
    const node = document.createElement('div')
    node.id = 'back'
    document.body.appendChild(node)
}
```

5、组件卸载时，清除节点

```javascript
onUnmounted(() => {
    document.body.removeChild(node)
})
```

