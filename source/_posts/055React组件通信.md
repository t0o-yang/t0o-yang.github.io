---
title: 055React组件通信
tags: ["React"]
categories:
  - 百日博客计划
date: 2023-04-23 16:42:04
description: 总结React组件间通信
---

# 前言

对自己感兴趣的领域，要多学习

### 组件间通信

**父子**：父组件在调用子组件的时候，只需要在子组件标签内传递参数，子组件通过props属性就能接收父组件传递过来的参数；
**子父**：父组件向子组件传一个函数，然后通过这个函数的回调，拿到子组件传过来的值；
**兄弟**：通过父组件，传递数据；
**跨级**：使用`React.createContext`创建一个`context`，再通过`Provider`组件用于创建数据源，`Consumer`组件用于接收数据；
**非关系组件**：
- 如果项目复杂的话，采用`redux`进行全局的数据流管理
- 采用发布订阅的`Event Bus`来进行组件通信

这里举一个实现Event Bus的例子（[原文链接](https://blog.csdn.net/wengqt/article/details/80114590)）：A和B是两个互不相关的组件，A组件的功能是登录，B组件的功能是登录之后显示用户名，这里就需要A组件将用户名传递给B组件。那么我们应该怎么做呢？

在A组件中注册/发布一个type为login的事件；
在B组件中注册一个监听/订阅，监听login事件的触发；
然后当登录的时候login事件触发，然后B组件就可以触发这个事件的回调函数。
Event Bus 的实现：
```js
class EventBus {
  constructor() {
    this.events = this.events || new Object(); 
  }
}
//首先构造函数需要存储event事件，使用键值对存储
//然后我们需要发布事件，参数是事件的type和需要传递的参数
EventBus.prototype.emit = function(type, ...args) { 
    let e; 
    e = this.events[type];  
    // 查看这个type的event有多少个回调函数，如果有多个需要依次调用。
    if (Array.isArray(e)) {  
        for (let i = 0; i < e.length; i++) {   
            e[i].apply(this, args);    
          }  
   } else {
          e[0].apply(this, args);  
         }  
   };
 //然后我们需要写监听函数，参数是事件type和触发时需要执行的回调函数
 EventBus.prototype.addListener = function(type, fun) { 
       const e = this.events[type]; 

        if (!e) {   //如果从未注册过监听函数，则将函数放入数组存入对应的键名下
         this.events[type]= [fun];
        } else {  //如果注册过，则直接放入
           e.push(fun);
        }
  };
  const eventBus = new EventBus();
  export default eventBus;
```
A组件中注册：
```js
EventBus.emit('login',values.userName)
```
B组件中监听：
```js
EventBus.addListener('login',(name)=>{
  this.setState({user:name})
})
```
