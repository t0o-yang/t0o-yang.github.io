---
title: 057React学习
tags: ["React"]
categories:
  - 百日博客计划
date: 2023-04-25 10:15:03
description: 生命周期、 Fiber 架构
---

todo: 明天写吧！！！

### 生命周期

创建阶段：
1. constructor
方法内部通过super获取父组件的props，通常在这里初始化state、挂载方法
2. getDerivedStateFromProps(newProps,prevState)
**接收参数**：newProps，即将更新的props；prevState，上一个状态的state
**返回值**：newState 新的state对象 | null，表示不需要更新
组件创建和更新阶段，不论是props变化还是state变化，也会调用。可以比较props、state，防止无用state更新
3. render

返回JSX对象，用于渲染DOM结构
4. componentDidMount

组件挂载到真实DOM后执行。多用于执行数据获取，事件监听等操作。


更新阶段：
1. getDerivedStateFromProps
2. shouldComponentUpdate

基于当前props和state确定组件是否需要更新
3. render
4. getSnapshotBeforeUpdate(prevProps, prevState)
返回的一个`Snapshot`值，作为componentDidUpdate第三个参数传入
执行之时DOM元素还没有被更新，目的在于获取组件更新前的一些信息，比如组件的滚动位置之类的，在组件更新后可以根据这些信息恢复一些UI视觉上的
5. componentDidUpdate
组件更新后触发，可以根据前后的props和state的变化做相应的操作，如获取数据，修改DOM样式等

销毁阶段：
componentUnmounted
组件卸载前，清理一些注册是监听事件，或者取消订阅的网络请求等

### Fiber /ˈfaɪbər/

**背景（解决什么问题）**
JS引擎和页面渲染引擎的两个线程是互斥的。如果JS线程长时间占用主线程，那么页面渲染就不能不等待，页面长期不更新，会导致页面响应性变差，用户可能会感觉卡顿。