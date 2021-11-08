## 一、改造异步请求方式

```typescript
// 改造前
fetchColumns (context) {
    axios.get('/column').then(res => {
        context.commit('fetchColumns', res.data)
    })
}
getColumnById({commit},cid){
    axios.get(`/column/${cid}`).then(res => {
        commit('getColumnById',res.data)
    })
}

// 改造后
async fetchColumns (context) {
    const {data} = await axios.get('/column');
    context.commit('fetchColumns', data)
}
async getColumnById({commit},cid){
    const {data} = await axios.get(`/column/${cid}`)
    commit('getColumnById',data)
}
```

## 二、抽取公共部分

```typescript
const getAndCommit = async (url: string, mutationName: string, commit: Commit) => {
  const { data } = await axios.get(url)
  commit(mutationName, data)
}
```

改造`action`部分代码

```typescript
    fetchColumns({ commit }) {
      getAndCommit('/column', 'fetchColumns', commit)
    },
    getColumnById({ commit }, cid) {
      getAndCommit(`/column/${cid}`, 'getColumnById', commit)
    },
    fetchPosts({ commit }, cid) {
      getAndCommit(`/column/${cid}/posts`, 'fetchPosts', commit)
    }
```

