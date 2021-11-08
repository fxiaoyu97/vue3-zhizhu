**HTML5 History API： https://developer.mozilla.org/zh-CN/docs/Web/API/History_API**

这个 API 帮助我们可以在不刷新页面的前提下动态改变浏览器地址栏中的URL地址，动态修改页面上所显示资源。

**history.pushState(state, title, url) 方法 ：添加一条历史记录，不刷新页面参数 **

- state : 一个于指定网址相关的状态对象，popstate事件触发时，该对象会传入回调函数中。如果不需要这个对象，此处可以填null。
- title : 新页面的标题，但是所有浏览器目前都忽略这个值，因此这里可以填null。
- url : 新的网址，必须与前页面处在同一个域。浏览器的地址栏将显示这个网址。

使用 history API 做的小例子地址： https://codesandbox.io/s/gallant-newton-kl9hj?file=/src/index.js

```javascript
const handleChange = (url, content) => {
  // go to url
  window.history.pushState(null, "hello there", url);

  // new data
  document.getElementById("app").innerHTML = `
    <h1>${content}</h1>
  `;
};
document.getElementById("change").addEventListener("click", e => {
  e.preventDefault();
  handleChange("create.html", "create");
});

document.getElementById("home").addEventListener("click", e => {
  e.preventDefault();
  handleChange("/", "home");
});
```

### SPA 的优点

- 速度快，第一次下载完成静态文件，跳转不需要再次下载静态文件
- 体验好，整个交互趋于无缝，更倾向于原生应用
- 为前后端分离提供了实践场所

