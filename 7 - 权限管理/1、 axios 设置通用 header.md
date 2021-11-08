1、修改`store.js`文件，获取token的时候，在axios上添加

```javascript
mutations: {
	……
    login(state, rawData) {
      const { token } = rawData.data
      state.token = token
      axios.defaults.headers.common.Authorization = `Bearer ${token}`
    }
  }
```

2、添加方法获取用户信息

```javascript
mutations: {
    fetchCurrentUser(state,rawData){
      state.user = {isLogin:true,...rawData.data.item}
    }
 },
 actions: {
    fetchCurrentUser({commit}){
      getAndCommit(`/user/current`,'fetchCurrentUser',commit)
    }
 }
```

3、使用组合action完成登录和获取用户信息

```javascript
loginAndFetch({dispatch},loginData){
    return dispatch('login',loginData).then(()=>{
        return dispatch('fetchCurrentUser')
    })
}
```

