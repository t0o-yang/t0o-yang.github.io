---
title: 016手写Promise
categories: 百日博客计划
---

# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

实现`Promise`

# 实现Promise

原文：[手撕Promise](https://juejin.cn/post/7032564107899322381#heading-0)

## 构造函数

```javascript
// Promise 构造函数

// 定义状态
const PENDING='pending'
const FULFILLED='fulfilled'
const REJECTED='rejected'

function Promise(executor){
    // 保证 this 指向实例
    const that=this 
    that.status=PENDING // 初始化状态
    that.data=undefined // 完成（成功或失败）后回调的值
    that.callbacks=[]    // 回调队列，用{ onResolve, onReject }对象存储指定的回调
    
    // resolve 改变实例状态为成功的函数
    function resolve(value) {
        if(that.status!==PENDING) return;
        that.status=FULFILLED;
        that.data=value;
        // 需要回调，调用 queueMicrotask 执行
        if(that.callbacks.length>0) {
            queueMicrotask(that.callbacks.forEach(cb => cb(that.data)))
        }
    }
}
```

其中提到了一个[微任务ueMicrotask()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)的概念。

## 微任务和任务

MDN上写的很难懂啊。
关键区别：首先，每当一个任务存在，事件循环都会检查该任务是否正把控制权交给其他 JavaScript 代码。如若不然，事件循环就会运行微任务队列中的所有微任。
其次，如果一个微任务通过调用 queueMicrotask(), 向队列中加入了更多的微任务，则那些新加入的微任务 会早于下一个任务运行。


# 彩蛋

中小企业对小程序、移动端的需求很大，先学学小程序。