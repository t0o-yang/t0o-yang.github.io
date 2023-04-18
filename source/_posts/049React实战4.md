---
title: 049React实战4
tags: ["React", "项目"]
categories:
  - 百日博客计划
  - 仿携程网站
date: 2023-04-18 09:36:21
description: 学习react开发
---

### react-thunk

处理异步请求的中间件，管理 axios

### 两次请求

React.StrictMode 的确会造成两次组件渲染，不过这个只会在开发阶段发生，打包之后，在生产环境中不会发送两次请求

```html
<React.StrictMode>
  <Provider store="{store}">
    <App />
  </Provider>
</React.StrictMode>
```
