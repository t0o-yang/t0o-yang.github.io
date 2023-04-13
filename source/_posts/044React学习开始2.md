---
title: 044React学习2
tags: ["React"]
categories: 百日博客计划
date: 2023-04-12 00:00:00
description: react基础
---

# 前言

学习函数式组件，Hook 的内容

### 什么是 Hooks？

在函数式组件中，为组件引入特定操作
常用的有，useState, useEffect

#### 自定义hooks


### 什么是纯函数

给函数传入相同的参数，这个函数永远会返回相同的结果

### 什么是副作用

副作用 side effect。函数中处理了与返回值无关的事情。在函数式组件中通过`useEffect `实现异步更新

### 有状态和无状态组件的区别

无状态组件没有 state，数据来源外部注入的 props。没有生命周期，没有副作用，无法在组件内访问 API，进行数据异步更新

### 如何跨组件传递数据

使用 props
react的上下文 context，在需要的组件中引入并使用appContext.Consumer标签
hooks中的 useContext

### HOC 是什么意思

高阶组件，在组件外套一层组件。参数为组件，返回是个新组件的函数。
命名规范：以with为前缀
