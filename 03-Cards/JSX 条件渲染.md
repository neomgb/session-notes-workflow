---
date: 2022-12-17
time: 11:05 上午
tags: 前端 React/jsx
---

作用：根据是否满足条件生成 `HTML` 结构，比如 Loading 效果
实现：
- 三元表达式
- 逻辑与(&&)运算符

```js
const flag = true

function App() {
  return (
    <div className="App">
      {/* 1.三元表达式 */}
      {flag ? 'react真有趣': 'vue真有趣'}
      {flag ? <span>this is span</span> : null}
      {/* 2.逻辑运算 */}
      {true && <span>this is span</span>}
    </div>
  );
}
```

---
##### 参考资料
- [尚硅谷-jsx 小练习](https://www.bilibili.com/video/BV1wy4y1D7JT?p=6)
- [语雀-react 基础课-jsx 条件渲染](https://www.yuque.com/fechaichai/qeamqf/xbai87#llLiU)

##### 笔记链接
