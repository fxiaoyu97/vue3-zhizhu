## 一、安装

官方安装地址：https://next.router.vuejs.org/zh/installation.html

```shell
npm install vue-router@4
#或者
yarn add vue-router@4
```

## 二、添加路由

在 main.ts 文件添加路由

```typescript
import { createRouter, createWebHistory } from 'vue-router'

const routerHistory = createWebHistory()
const router = createRouter({
  history: routerHistory,
  routes: [
   {
      path: '/test/:id',
      name: 'test',
      component: () => import('./views/Test.vue')
    }
  ]
})
```

## 三、获取参数和跳转路由

1、测试页面`test.vue`代码

+ 对于一个object，如果我们想再页面显示它的全部内容，除了在 js 中使用 console，也可以使用 pre 标签包裹这个变量。
+ pre 标签可定义预格式化的文本。在pre元素中的文本会保留空格和换行符。文本显现为等宽字体

```vue
<template>
  <div>
    <!-- pre标签显示object对象的内容 -->
    <pre>{{ route }}</pre>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import { useRoute } from 'vue-router'

export default defineComponent({
  setup () {
    const route = useRoute()
    return { route }
  }
})
</script>
```

请求路径：`http://localhost:8080/test/11?search=123#456`

路径对应`useRoute`参数如下:

```json
{
  "path": "/test/11",
  "name": "test",
  "params": {
    "id": "11"
  },
  "query": {
    "search": "123"
  },
  "hash": "#456",
  "fullPath": "/test/11?search=123#456",
  "matched": [
    {
      "path": "/test/:id",
      "name": "test",
      "meta": {},
      "props": {
        "default": false
      },
      "children": [],
      "instances": {},
      "leaveGuards": {
        "Set(0)": []
      },
      "updateGuards": {
        "Set(0)": []
      },
      "enterCallbacks": {},
      "components": {
        "default": {
          "__file": "src/views/Test.vue",
          "__hmrId": "5b2d5ecc"
        }
      }
    }
  ],
  "meta": {}
}
```

## 四、router-link 组件跳转的方式

我们第一种方法可以将 to 改成不是字符串类型，而是 object 类型，这个object 应该有你要前往route 的 name ，还有对应的 params。

```javascript
 :to="{ name: 'column', params: { id: column.id }}"
```

第二种格式，我们可以在里面传递一个模版字符串，这里面把 column.id 填充进去就好。

```javascript
 :to="`/column/${column.id}`"
```

使用 useRouter 钩子函数进行跳转

```javascript
const router = useRouter()
// 特别注意这个是 useRouter 而不是 useRoute，差一个字母，作用千差万别，那个是获得路由信息，这个是定义路由的一系列行为。在这里，我们可以掉用
router.push('/login') 

// router.push 方法跳转到另外一个 url，它接受的参数和 router-link 的 to 里面的参数是完全一致的，其实router link 内部和这个 router 分享的是一段代码，可谓是殊途同归了。
```

1、修改`App.vue`页面

```vue
<template>
  <div class="container">
    <global-header :user="currentUser"></global-header>
    <router-view/>
    <footer class="text-center py-4 text-secondary bg-light mt-6">
      <small>
        <ul>
          <li class="list-inline-item">2021 者也专栏</li>
          <li class="list-inline-item">课程</li>
          <li class="list-inline-item">文档</li>
          <li class="list-inline-item">联系</li>
          <li class="list-inline-item">更多</li>
        </ul>
      </small>
    </footer>
  </div>
</template>
```

2、修改`ColumnList.vue`组件

```html
<!-- 修改前 -->
<a href="#" class="btn btn-outline-primary">进入专栏</a>
<!-- 修改后 -->
<router-link to="`/column/${column.id}`" class="btn btn-outline-primary">进入专栏</router-link>
```

3、修改登录跳转

```javascript
// 引入组件
import { useRouter } from 'vue-router'

export default defineComponent({
    …
    setup() {
    	const router = userRouter()
        router.push('/')
	}
})
```

