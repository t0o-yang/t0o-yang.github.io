---
title: 020uniapp前端请求
categories: 百日博客计划
---

# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

uniapp项目，前端请求方式（或 常用插件）。后端是用`nodejs`创建的服务器。

# uni.request

官方文档: [发起请求](https://uniapp.dcloud.net.cn/api/request/request.html)

# axios

vue项目，第一个想到的就是`axios`，使用了`uniapp-axios-adapter`这个插件，使用方式：
```javascript
// 业务代码中
import axios from "uniapp-axios-adapter";
axios.get({
  url: "https://example.com/api/getUserById",
  params: {
    id: 1,
  },
});
```
结果出错，`get`没有定义，最终选择了`uni-request`。

# todos

计划：点击选择框，事项状态转为完成，需要完成的功能点：
1. 事件完成，图标需要换成`checkbox`
2. 文字删除线
3. 输入框不可编辑
4. 更新 leftCount	

