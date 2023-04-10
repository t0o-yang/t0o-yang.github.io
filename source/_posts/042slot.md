---
title: 042slot
tags: ["Vue","slot"]
categories: 百日博客计划
date: 2023-04-10 23:55:28
description: slot插槽的使用
---

# 前言

坚持不下去了，有种被赶着走的感觉。没有复习。今天两场面试，岗位都一样。

### 插槽slot

网页结构占位符，分为`默认插槽`、`具名插槽`、`作用域插槽`：
- 默认插槽
- 具名插槽，`slot`上加上`name`属性，标注插槽名。父组件使用时通过`<template v-slot:default>默认插槽</template>`的方式，将具体内容加到子组件的指定位置上。`v-slot:`可简写成`#`
- 作用域插槽
```html
<!--子组件 Child.vue-->
<template> 
  <slot name="footer" testProps="子组件的值">
          <h3>没传footer插槽</h3>
    </slot>
</template>

<!--父组件 Parent.vue-->
<child> 
    <!-- 把v-slot的值指定为作⽤域上下⽂对象 -->
    <template v-slot:default="slotProps">
      来⾃⼦组件数据：{{slotProps.testProps}}
    </template>
    <template #default="slotProps">
      来⾃⼦组件数据：{{slotProps.testProps}}
    </template>
</child>
```