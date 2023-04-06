---
UID: 20230406080001
title: "srAnnote@渲染页面：浏览器的工作原理"
date: 2023-04-06
alias: ["srAnnote@渲染页面：浏览器的工作原理"]
type: Simpread
index: 16
---

> [!timeline]+ 简介
>> **元数据**
>---
> **原文**:: [渲染页面：浏览器的工作原理](https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work)
> **日期**:: 2023-04-06
> **标签**:: #SimpRead 
>> **摘要**
>---
> 页面内容快速加载和流畅的交互是用户希望得到的 Web 体验，因此，开发者应力争实现这两个目标。

## 笔记

> [📌](<http://localhost:7026/reading/16#id=1680739201724>) <mark style="background-color: #ffeb3b">Highlight</mark> 
> 渲染步骤包括样式、布局、绘制，在某些情况下还包括合成。在解析步骤中创建的 CSSOM 树和 DOM 树组合成一个 Render 树，然后用于计算每个可见元素的布局，然后将其绘制到屏幕上。在某些情况下，可以将内容提升到它们自己的层并进行合成，通过在 GPU 而不是 CPU 上绘制屏幕的一部分来提高性能，从而释放主线程。

^sran-1680739201724
> [📌](<http://localhost:7026/reading/16#id=1680739232193>) <mark style="background-color: #ffeb3b">Highlight</mark> 
> 当 CSS 被解析并创建 CSSOM 时，其他资源，包括 JavaScript 文件正在下载（借助预加载扫描器）。JavaScript 被解释、编译、解析和执行。脚本被解析为抽象语法树。一些浏览器引擎使用[抽象语法树](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E8%AF%AD%E6%B3%95%E6%A0%91)并将其传递到解释器中，输出在主线程上执行的字节码。这就是所谓的 JavaScript 编译。

^sran-1680739232193

