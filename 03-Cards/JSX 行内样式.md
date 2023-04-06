---
date: 2022-12-17
time: 10:10 晚上
tags: 前端 React/jsx
---

在元素身上绑定一个 `style` 属性
```js
const style = {
  color: 'green',
  fontSize: '30px'
}

function App() {
  return (
    <div className="App">
      <span style={style}>this is span</span>
    </div>)
}
```
如果不使用变量，要用 `style={{key: value}}` 的形式去写
- 第一层括号表示要写 `jsx` 语法
- 第二层括号表示一个对象

---
##### 参考资料
- [jsx-样式控制](https://www.bilibili.com/video/BV1Z44y1K7Fj/?p=12)
-  [语雀-react 基础课-jsx 样式处理](https://www.yuque.com/fechaichai/qeamqf/xbai87#QtkKa)

##### 笔记链接
