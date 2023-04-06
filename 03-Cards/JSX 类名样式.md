---
date: 2022-12-17
time: 10:11 晚上
tags: 前端 React/jsx
---

编写外部 `css` 样式
```css
.active {
  color: orange;
  font-size: 40px;
}
```
引入外部样式，在元素身上绑定一个 `className` 属性
```js
import './app.css'

function App() {
  return (
    <div className="App">
      <span className='active'>this is span</span>
    </div>)
}
```
PS：因为 `JSX` 本质上是 `js` 的扩展，所以在写 UI 的时候，不能使用 `class` 关键字，而应该用 `className` 绑定外部样式

---
##### 参考资料
- [B站-jsx 样式控制](https://www.bilibili.com/video/BV1Z44y1K7Fj/?p=12)
- [react 基础课-jsx 样式处理](https://www.yuque.com/fechaichai/qeamqf/xbai87#QtkKa)

##### 笔记链接
- [JSX 是什么](JSX%20是什么.md)
- [JSX 条件渲染](JSX%20条件渲染.md)