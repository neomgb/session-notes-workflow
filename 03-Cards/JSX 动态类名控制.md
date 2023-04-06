---
date: 2022-12-22
time: 4:54 下午
tags: 前端 React/jsx
---

动态控制类名样式的挂载，满足条件才有。如根据变量 `showName` 的值决定是否挂载 `active` 这个类名样式：
```js
function App() {
  const showName = true
  return (
    <div className="App">
      <span className={showName ? 'active': ''}>this is span</span>
    </div>)
}
```


---
##### 参考资料
- [B站-jsx 动态类名控制](https://www.bilibili.com/video/BV1Z44y1K7Fj/?p=13)

##### 笔记链接
