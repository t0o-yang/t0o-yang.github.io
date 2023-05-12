---
title: 064Promise1
tags: ['Promise']
categories:
  - 百日博客计划
date: 2023-05-12 21:03:24
description: Promise的三个状态
---

#### Promise的三个状态及其变化

- pending, 待处理状态；
- resolved, 解决状态，触发 then 的回调；
- rejected, 拒绝状态，触发 catch 的回调。

then, catch 中状态的切换，正常返回 resolved 状态的promise，里面有报错则返回 rejected 状态的promise。