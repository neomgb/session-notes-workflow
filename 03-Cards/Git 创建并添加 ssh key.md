---
date: 2023-02-15
time: 10:41 上午
tags: Git
---

一、创建 ssh key，一路回车
```shell
ssh-keygen -t rsa -C "youremail@example.com"
```
二、用户主目录里找到 `.ssh` 目录，里面有 `id_ras` 和 `id_rsa.pub` 两个文件
- `id_rsa` 是私钥，不能泄露
- `id_rsa.pub` 是公钥，可以放心告诉任何人

三、登录 Github，添加 ssh key，粘贴 `id_rsa.pub` 文件的内容


---
##### 参考资料
- [廖雪峰-创建并添加 ssh key](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

##### 笔记链接
