---
date: 2022-12-18
time: 9:25 上午
tags: 前端 React/jsx
---
实现：使用数组的 `map` 方法，map 重复渲染的是哪个模板，就 return 谁
注意：
- 需要为遍历项添加 `key` 属性
- `key` 不会出现在 `DOM` 结构中，`React` 内部用来进行性能优化时使用
- 如果列表中有 `id` 这种唯一值，就用 `id` 作为 `key`

```js
const songs = [
  { id: 1, name: '我的楼兰' },
  { id: 2, name: '想念自己' },
  { id: 3, name: '和你一样' }
]

function App() {
  return (
    <div className="App">
      <ul>
        {
          songs.map(song => <li key={song.id}>{song.name}</li>)
        }
      </ul>
    </div>
  );
}
```

---
##### 参考资料
- [B站-jsx 列表渲染](https://www.bilibili.com/video/BV1Z44y1K7Fj/?p=9)
- [语雀-react 基础课-jsx 列表渲染](https://www.yuque.com/fechaichai/qeamqf/xbai87#cMIcQ)

##### 笔记链接
