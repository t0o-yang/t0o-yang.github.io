---
title: 056React中setState机制
tags: ["React"]
categories:
  - 百日博客计划
date: 2023-04-24 17:22:27
description: setState机制
---

### setState机制

```js
setState(partialState[, callback])
```
`partialState`接收对象或函数或null;
callback：参数可选，更新后的回调函数;

setState更新数据的情况可分为：
- 异步更新：参数传入对象时属于异步，无法在更新后，紧接着获取最新数据。可以在callback中读取最新数据；
- 同步更新：将setState的调用包裹在setTimeout或原生dom事件中；
- 批量更新：当处于异步更新时，对同一个值进行多次 setState，后面的更新会覆盖前面的更新，最后只执行最后一次更新操作。可以通过传入函数完成批量更新。函数参数`(prevState, props)`，通过prevState访问当前值，并且return最新数据。实例代码如下

```js
onClick = () => {
  this.setState((prevState, props) => {
    return {count: prevState.count + 1};
  });
  this.setState((prevState, props) => {
    return {count: prevState.count + 1};
  });
}
```