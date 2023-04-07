---
title: 039keepalive
tags: ["keep-alive"]
categories: 百日博客计划
date: 2023-04-07 23:07:01
description: 说说对keep-alive的了解
---

### keep-alive的实现
作用：实现组件缓存，保持这些组件的状态，以避免反复渲染导致的性能问题。 需要缓存组件 频繁切换，不需要重复渲染
场景：tabs标签页 后台导航，vue性能优化
原理：Vue.js内部将DOM节点抽象成了一个个的VNode节点，keep-alive组件的缓存也是基于VNode节点的而不是直接存储DOM结构。它将满足条件（pruneCache与pruneCache）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点从cache对象中取出并渲染。