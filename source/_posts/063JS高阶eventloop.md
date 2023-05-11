---
title: 063JS高阶eventloop
tags: ["JS"]
categories:
  - 百日博客计划
date: 2023-05-11 21:48:10
description: 复习 eventloop 内容
---

### eventloop 事件循环机制

`JS`是单线程的，为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步代码压入执行栈中，依次执行，将异步代码推入异步队列，异步队列又分为宏任务队列和微任务队列，因为宏任务队列的执行时间较长，所以微任务队列要优先于宏任务队列。微任务队列的代表就是，`Promise.then`，`MutationObserver`，宏任务的话就是`setImmediate setTimeout setInterval`

JS 运行的环境。一般为浏览器或者 Node。 在浏览器环境中，有 JS 引擎线程和渲染线程，且两个线程互斥。 Node 环境中，只有 JS 线程。 不同环境执行机制有差异，不同任务进入不同 Event Queue 队列。 当主程结束，先执行准备好微任务，然后再执行准备好的宏任务，一个轮询结束。

#### 浏览器中的事件环（Event Loop）

事件环的运行机制是，先会执行栈中的内容，栈中的内容执行后执行微任务，微任务清空后再执行宏任务，先取出一个宏任务，再去执行微任务，然后在取宏任务清微任务这样不停的循环。

- eventLoop 是由 JS 的宿主环境（浏览器）来实现的；

- 事件循环可以简单的描述为以下四个步骤:

  1. 函数入栈，当 Stack 中执行到异步任务的时候，就将他丢给 WebAPIs,接着执行同步任务,直到 Stack 为空；
  2. 此期间 WebAPIs 完成这个事件，把回调函数放入队列中等待执行（微任务放到微任务队列，宏任务放到宏任务队列）
  3. 执行栈为空时，Event Loop 把微任务队列执行清空；
  4. 微任务队列清空后，进入宏任务队列，取队列的第一项任务放入 Stack(栈）中执行，执行完成后，查看微任务队列是否有任务，有的话，清空微任务队列。重复 4，继续从宏任务中取任务执行，执行完成之后，继续清空微任务，如此反复循环，直至清空所有的任务。

  ![事件循环流程](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/342e581223d2471d9484fc48beb9f8e1~tplv-k3u1fbpfcp-zoom-1.image)

- 浏览器中的任务源(task):

  - `宏任务(macrotask)`：\
    宿主环境提供的，比如浏览器\
    ajax、setTimeout、setInterval、setTmmediate(只兼容 ie)、script、requestAnimationFrame、messageChannel、UI 渲染、一些浏览器 api
  - `微任务(microtask)`：\
    语言本身提供的，比如 promise.then\
    then、queueMicrotask(基于 then)、mutationObserver(浏览器提供)、messageChannel 、mutationObersve

传送门 ☞ [# 宏任务和微任务](https://juejin.cn/post/7001881781125251086)

#### Node 环境中的事件环（Event Loop)

`Node`是基于 V8 引擎的运行在服务端的`JavaScript`运行环境，在处理高并发、I/O 密集(文件操作、网络操作、数据库操作等)场景有明显的优势。虽然用到也是 V8 引擎，但由于服务目的和环境不同，导致了它的 API 与原生 JS 有些区别，其 Event Loop 还要处理一些 I/O，比如新的网络连接等，所以 Node 的 Event Loop(事件环机制)与浏览器的是不太一样。

![2020120317343116.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e362c1770f62428fbf3faabd99d2a64c~tplv-k3u1fbpfcp-zoom-1.image) 执行顺序如下：

- `timers`: 计时器，执行 setTimeout 和 setInterval 的回调
- `pending callbacks`: 执行延迟到下一个循环迭代的 I/O 回调
- `idle, prepare`: 队列的移动，仅系统内部使用
- `poll轮询`: 检索新的 I/O 事件;执行与 I/O 相关的回调。事实上除了其他几个阶段处理的事情，其他几乎所有的异步都在这个阶段处理。
- `check`: 执行`setImmediate`回调，setImmediate 在这里执行
- `close callbacks`: 执行`close`事件的`callback`，一些关闭的回调函数，如：socket.on('close', ...)

