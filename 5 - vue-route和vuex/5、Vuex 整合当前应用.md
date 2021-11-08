一、创建 `store.ts` 文件

```typescript
import { createStore } from 'vuex'
import { testData, testPosts, ColumnProps, PostProps } from './testData'
interface UserProps {
  isLogin: boolean;
  name?: string;
  id?: number;
}
export interface GlobalDataProps {
  columns: ColumnProps[];
  posts: PostProps[];
  user: UserProps;
}
const store = createStore<GlobalDataProps>({
  state: {
    columns: testData,
    posts: testPosts,
    user: { isLogin: false }
  },
  mutations: {
    login(state) {
      state.user = { ...state.user, isLogin: true, name: 'viking' }
    }
  }
})

export default store
```

**在`main.ts`文件中使用**

```typescript
import { useStore } from 'vuex'
import { GlobalDataProps } from '../store'

...
const store = useStore<GlobalDataProps>()
const list = computed(() => store.state.columns)
```

