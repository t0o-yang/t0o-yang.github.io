---
title: 043React学习1
tags: ["React"]
categories: 百日博客计划
date: 2023-04-11 00:00:00
description: react基础
---

# 前言

没复习，React 学习开始，以下是一些问题，带着问题学习

### 说说对 React 的理解

- 是什么
- 特点
- 优缺点

是什么：JS 框架；

特点：
单向数据绑定；
虚拟 DOM；
组件化；

优缺点：优点，页面渲染更高效。缺点，学习成本较高。

### JSX

- 是什么
- 特点
- 与其他方案的对比

是什么：视图逻辑与 UI 的结构耦合
特点：
常规的 html 代码与 JSX 兼容；
可以直接嵌入表达式；
可以包含子元素；
命名规范（属性名要用驼峰，自定义属性名加上前缀`data-`）；
对比：
与 vueJS 的模板语法相比，不需要加入新的语法，不需要学习，直接上手。

### 虚拟 DOM

- 是什么
- React 中的使用

。。。。

### 什么是组件

静态结构和代码逻辑的组合，通过引入，可以在其他页面中快速使用
React 中组件分为类组件和函数组件

### state 和 props 的区别

state 是组件的数据模型，只能在组件内访问，可以认为是组件的“私有属性”。只能通过`setState`的方式修改数据，state 的更新是异步的。
props 用于组件间的通信，即数据传入。所有的 props 都是只读的 immutable

### react 组件生命周期

创建前后；挂载前后；更新前后；销毁前后

1. Mounting： 创建虚拟 DOM，渲染 UI；
2. Updating： 更新虚拟 DOM，重新渲染 UI；
3. Unmounting: 删除虚拟 DOM，移除 UI

**类组件**中的生命周期钩子：
componentDidMount
componentDidUpdate
componentWillUnmount
