官方文档：https://axios-http.com/zh/docs/intro

## 一、安装

```shell
yarn add axios --save
```

## 二、基本配置

+ 项目引用：`import axios from 'axios'`
+ 在`main.ts`中设置基础访问地址：`axios.defaults.baseURL='http://xxxx'`

## 三、异步请求

1、在`store `中创建请求

```javascript
mutations: {
    fetchColumns(state, resData){
        state.columns = resData.data
    }
},
actions: {
  fetchColumns(context){
    axois.get('/columns').then(res =>{
        context.commit('fetchColumns',res.data)
    })
  }   
}
```

2、在`Home.vue`中使用，在挂载的时候调用

```javascript
// 调用store的 fetchColumns 方法获取column
onMounted(() => {
    store.dispatch('fetchColumns')
})
```

3、修改store代码

```typescript
import axios from 'axios'

export interface UserProps {
  isLogin: boolean
  name?: string
  id?: number
  columnId?:number
}
interface ImageProps{
  id?: string
  url?: string
  createdAt?: string
}
export interface ColumnProps {
  id: string;
  title: string;
  avatar?: ImageProps;
  description: string;
}
export interface PostProps {
  id: number;
  title: string;
  content: string;
  image?: string;
  createdAt: string;
  columnId: number;
}
```

4、修改`ColumnList.vue`，修改图片路径

```javascript
if (!column.avatar) {
    // 使用默认图片
    column.avatar = {
        url: require('@/assets/column.jpg')
    }
} else {
    // 图片缩放
    column.avatar.url = column.avatar.url + '?x-oss-process=image/resize,m_fixed,h_100,w_100'
}
```

