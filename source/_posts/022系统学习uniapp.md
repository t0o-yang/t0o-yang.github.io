# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

系统学习一下uniapp的使用，正好找的教程的项目都是基于vue2的，刚好学习一下vue2的语法。

# uniapp、vue项目区别

## 自定义组件

1. 在uni-app工程根目录下的 components 目录，创建并存放自定义组件：

```shell
│─components            	符合vue组件规范的uni-app组件目录
│  └─componentA         	符合‘components/组件名称/组件名称.vue’目录结构，easycom方式可直接使用组件
│  		└─componentA.vue    可复用的componentA组件
│  └─component-a.vue      可复用的component-a组件

```
**必须components目录下，创建同名组件的同名目录**

2. 直接使用，区别于vue2的引入再注册

```javascript
<!-- 在index.vue引入 uni-badge 组件-->
<template>
    <view>
        <uni-badge text="1"></uni-badge><!-- 3.使用组件 -->
    </view>
</template>
<script>
    // 这里不用import引入，也不需要在components内注册uni-badge组件。template里就可以直接用
    export default {
        data() {
            return {
            }
        }
    }
</script>
```

# 彩蛋

以前在github上用`hexo`做了个人博客，访问：[个人博客](https://t0o-yang.github.io/)。还挺快的，但已经忘了怎么使用了！电脑也换了。明天用gitee重新整一个。之后博客就放在那了。

